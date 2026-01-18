---
title: API Errors
description: Error codes, troubleshooting, and best practices for handling API errors gracefully.
---

# API Errors

Understanding and handling API errors correctly ensures your integration is robust and user-friendly.

## Error Response Format

All errors follow a consistent structure:

```json
{
  "version": "1.0",
  "error": {
    "code": "ERROR_CODE",
    "message": "Human-readable error message",
    "details": "Additional context (optional)"
  }
}
```

## Error Codes

### INVALID_INPUT

The request body or parameters are invalid.

**HTTP Status:** `400`

**Causes:**
- Missing required fields
- Invalid data types
- Malformed JSON

**Example:**

```json
{
  "version": "1.0",
  "error": {
    "code": "INVALID_INPUT",
    "message": "Both techA and techB are required."
  }
}
```

**Solution:** Ensure all required fields are present and correctly formatted.

---

### RATE_LIMIT_EXCEEDED

Request exceeds rate limit for the endpoint.

**HTTP Status:** `429`

**Causes:**
- Too many requests in the rate limit window
- Multiple clients sharing the same IP

**Example:**

```json
{
  "version": "1.0",
  "error": {
    "code": "RATE_LIMIT_EXCEEDED",
    "message": "Rate limit exceeded. Please try again later.",
    "details": "Reset at 2025-01-15T10:31:00Z"
  }
}
```

**Headers:**
```
Retry-After: 45
X-RateLimit-Remaining: 0
X-RateLimit-Reset: 2025-01-15T10:31:00Z
```

**Solution:** Wait until the `X-RateLimit-Reset` time or `Retry-After` seconds have passed.

See [Rate Limits](/docs/api/rate-limits) for handling strategies.

---

### INTERNAL_ERROR

An unexpected server error occurred.

**HTTP Status:** `500`

**Causes:**
- Temporary server issues
- Database connection problems
- Unexpected data conditions

**Example:**

```json
{
  "version": "1.0",
  "error": {
    "code": "INTERNAL_ERROR",
    "message": "An error occurred while checking compatibility",
    "details": "Specific error message for debugging"
  }
}
```

**Solution:** Retry the request after a delay. If the issue persists, contact support.

---

### NOT_FOUND

Requested resource does not exist.

**HTTP Status:** `404`

**Causes:**
- Invalid endpoint path
- Removed compatibility data

**Example:**

```json
{
  "version": "1.0",
  "error": {
    "code": "NOT_FOUND",
    "message": "The requested resource was not found."
  }
}
```

**Solution:** Verify the endpoint URL and technology names are correct.

---

### NO_TECH_DETECTED

Scan request found no recognizable technologies.

**HTTP Status:** `400`

**Causes:**
- No packages in dependencies
- Unrecognized package names
- Empty package.json

**Example:**

```json
{
  "version": "1.0",
  "error": {
    "code": "NO_TECH_DETECTED",
    "message": "No recognized technologies found in dependencies",
    "details": "Ensure you have valid packages in your package.json"
  }
}
```

**Solution:** Verify your dependencies array or package.json contains valid package names.

---

## HTTP Status Codes

| Status | Meaning | Action |
|--------|---------|--------|
| `200` | Success | Process the response |
| `400` | Bad Request | Fix request parameters |
| `404` | Not Found | Verify endpoint URL |
| `429` | Rate Limited | Wait and retry |
| `500` | Server Error | Retry with backoff |

## Error Handling Best Practices

### 1. Always Check HTTP Status

```javascript
const response = await fetch(url, options);

if (!response.ok) {
  const error = await response.json();
  // Handle error based on status code
  switch (response.status) {
    case 400:
      console.error('Invalid input:', error.error.message);
      break;
    case 429:
      console.error('Rate limited. Retry at:', error.error.details);
      break;
    default:
      console.error('Unexpected error:', error.error.message);
  }
  return;
}

// Process successful response
const data = await response.json();
```

### 2. Implement Retry Logic

```javascript
async function fetchWithRetry(url, options, maxRetries = 3) {
  for (let i = 0; i < maxRetries; i++) {
    try {
      const response = await fetch(url, options);

      if (response.ok || response.status === 400) {
        // Success or client error - don't retry
        return response;
      }

      if (response.status === 429) {
        // Rate limited - wait for reset time
        const resetAt = response.headers.get('X-RateLimit-Reset');
        const waitMs = new Date(resetAt) - Date.now() + 1000;
        await new Promise(resolve => setTimeout(resolve, waitMs));
        continue;
      }

      if (response.status >= 500) {
        // Server error - exponential backoff
        const waitMs = Math.pow(2, i) * 1000;
        await new Promise(resolve => setTimeout(resolve, waitMs));
        continue;
      }

    } catch (error) {
      if (i === maxRetries - 1) throw error;
      // Network error - exponential backoff
      await new Promise(resolve => setTimeout(resolve, Math.pow(2, i) * 1000));
    }
  }

  throw new Error('Max retries exceeded');
}
```

### 3. Log Errors Contextually

```javascript
function logApiError(endpoint, error, context = {}) {
  console.error({
    timestamp: new Date().toISOString(),
    endpoint,
    errorCode: error.error?.code,
    message: error.error?.message,
    ...context
  });
}

// Usage
try {
  const data = await checkCompatibility('nextjs', 'prisma');
} catch (error) {
  logApiError('/api/v1/check', error, {
    techA: 'nextjs',
    techB: 'prisma'
  });
}
```

### 4. Provide User-Friendly Messages

```javascript
function getErrorMessage(error, context = {}) {
  switch (error.error?.code) {
    case 'INVALID_INPUT':
      return 'Please check your input and try again.';
    case 'RATE_LIMIT_EXCEEDED':
      return 'Too many requests. Please wait a moment.';
    case 'INTERNAL_ERROR':
      return 'Service temporarily unavailable. Please try again.';
    default:
      return 'An unexpected error occurred.';
  }
}
```

## Testing Error Handling

### Test Scenarios

| Scenario | How to Test | Expected Behavior |
|----------|-------------|-------------------|
| Invalid input | Send empty techA/techB | Returns 400 with INVALID_INPUT |
| Rate limit | Make 60+ requests in a minute | Returns 429 with RATE_LIMIT_EXCEEDED |
| Unknown tech | Use non-existent technology names | Returns 200 with status="unknown" |
| Server error | (Contact support to simulate) | Returns 500 with INTERNAL_ERROR |

### Example Test

```javascript
async function testInvalidInput() {
  const response = await fetch('https://devradar.dev/api/v1/check', {
    method: 'POST',
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify({ techA: '', techB: '' })
  });

  console.assert(response.status === 400, 'Expected 400 status');
  const error = await response.json();
  console.assert(error.error.code === 'INVALID_INPUT', 'Expected INVALID_INPUT');
  console.log('Test passed!');
}
```

## Getting Help

### Persistent Errors

If you encounter errors consistently:

1. **Check Status Page** - Verify service is operational
2. **Review Your Code** - Ensure proper error handling
3. **Check Rate Limits** - Verify you're within limits
4. **Contact Support** - Include error code and request details

### Support Contact

- **Email:** support@devradar.dev
- **GitHub:** [github.com/devradar-dev/website/issues](https://github.com/devradar-dev/website/issues)
- **Include in report:**
  - Error code
  - Request details (sanitize sensitive data)
  - Timestamp
  - Your account ID (if applicable)

## What's Next

- [Rate Limits](/docs/api/rate-limits) - Understanding rate limiting
- [Check Endpoint](/docs/api/check-endpoint) - Endpoint reference
- [API Overview](/docs/api/overview) - API introduction

---

**Need help?** [Contact Support](mailto:support@devradar.dev) →
