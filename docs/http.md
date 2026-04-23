# HTTP

HTTP (Hypertext Transfer Protocol) is the standard way clients and servers communicate on the web. Understanding requests, responses, and how APIs behave in production is very important.

## Python (requests library)

`requests` is the most common Python library for making HTTP calls in scripts, services, and automation jobs.

### Common request methods

- `requests.get(url, ...)`
- `requests.post(url, ...)`
- `requests.patch(url, ...)`
- `requests.put(url, ...)`
- `requests.delete(url, ...)`

### Most used arguments

- `params={...}`: query string values (for example, `?page=2`)
- `json={...}`: send JSON body (auto sets JSON encoding)
- `data={...}`: send form-like body
- `headers={...}`: send custom headers like `Authorization`
- `timeout=(connect_timeout, read_timeout)` or `timeout=10`

### Reading the response

- `response.status_code`: HTTP status code
- `response.json()`: parse JSON response body
- `response.text`: response as string
- `response.headers`: response headers

### Error handling pattern

- Use `response.raise_for_status()` to fail on non-2xx status codes.
- Wrap calls in `try/except requests.exceptions.RequestException` for network/timeouts.

### Auth and sessions

- Bearer token pattern:
  - `headers={"Authorization": f"Bearer {token}"}`
- Use `requests.Session()` when making many calls:
  - Reuses TCP connections (faster)
  - Keeps shared headers/cookies in one place

### Practical habits

- Always set a timeout.
- Log method, URL, status code, and request ID for debugging.
- Retry carefully (usually only idempotent operations).

### Basic Python examples

```python
import requests

url = "https://api.example.com/users"
params = {"page": 1, "limit": 20}

response = requests.get(url, params=params, timeout=10)
response.raise_for_status()

users = response.json()
print(users)
```

```python
import requests

token = "your_jwt_token"
url = "https://api.example.com/users"
payload = {"name": "Brian", "email": "brian@example.com"}
headers = {"Authorization": f"Bearer {token}"}

response = requests.post(url, json=payload, headers=headers, timeout=10)
response.raise_for_status()

created_user = response.json()
print(created_user)
```

## URL Basics

- **Scheme**: `https://` (secure) or `http://`
- **Host**: domain name (for example, `api.company.com`)
- **Path**: resource route (for example, `/users/123`)
- **Query params**: filters/options (for example, `?page=2&sort=name`)

## Core HTTP Methods

- `GET`: read data
- `POST`: create new data or trigger an action
- `PUT`: replace a resource
- `PATCH`: partially update a resource
- `DELETE`: remove a resource

In practice, most work APIs rely heavily on `GET`, `POST`, `PATCH`, and `DELETE`.

## Status Codes You Should Recognize

### 2xx Success

- `200 OK`: request succeeded
- `201 Created`: resource created
- `204 No Content`: succeeded with no body

### 4xx Client Errors

- `400 Bad Request`: invalid input/format
- `401 Unauthorized`: missing or invalid auth
- `403 Forbidden`: authenticated but not allowed
- `404 Not Found`: resource or route missing
- `409 Conflict`: request conflicts with current state
- `422 Unprocessable Entity`: validation failed
- `429 Too Many Requests`: rate limit hit

### 5xx Server Errors

- `500 Internal Server Error`: generic server failure
- `502 Bad Gateway`: upstream service error
- `503 Service Unavailable`: overloaded/down/maintenance
- `504 Gateway Timeout`: upstream timed out

## Headers That Matter Often

- `Authorization`: carries credentials (for example, Bearer token)
- `Content-Type`: format of request body (commonly `application/json`)
- `Accept`: expected response type
- `Cache-Control`: caching instructions
- `X-Request-ID` or trace headers: request tracing across services

## Body and Data Formats

`application/json` - This is the most common body format that is sent for HTTP requests
`multipart/form-data` - Same method as when you click `Upload` on a website. Request body is divided into parts, where one part can be text fields and another is the raw binary fo the image.

## Authentication and Authorization

- **Authentication** = who you are (token/session/cookie).
- **Authorization** = what you can do (roles/scopes/policies).
- Common API pattern: `Authorization: Bearer <token>`.

## Idempotency (Important for Reliability)

- A request is **idempotent** if repeating it has the same effect.
- `GET`, `PUT`, and `DELETE` are typically idempotent.
- `POST` is usually not idempotent unless designed with an idempotency key.

This matters for retries: clients and gateways may retry on timeouts.

## Caching and Performance Basics

- HTTP caching can reduce latency and server load.
- Common headers: `Cache-Control`, `ETag`, `Last-Modified`.
- Even if you do not design caching deeply, know when responses are cacheable and when they should not be.

## Timeouts, Retries, and Rate Limits

- Set reasonable client timeouts; avoid waiting forever.
- Retry only safe/idempotent operations by default.
- Respect `429` responses and back off.

## Cookies vs Tokens (Quick Mental Model)

- **Cookies** are common in browser session auth.
- **Bearer tokens** are common in service-to-service and API auth.
- Both can be secure when implemented correctly.

## Debugging HTTP Quickly

When something fails, check in this order:

1. URL/method correctness
2. Auth headers and token expiry
3. Request body shape and required fields
4. Status code category (4xx vs 5xx)
5. Response error message and server logs/traces
