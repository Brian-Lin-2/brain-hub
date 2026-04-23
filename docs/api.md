# API

## Authorization

Generally for most authorized HTTP requests to the API, a user would need to pass a `JWT Token` in the Authorization header to allow for access with the API endpoints.

### API Scope

An API Scope is a string that defines what access is allowed for a certain application.

For a valid `JWT Token` to be generated, an API Scope normally needs to be baked inside this token to allow for specific permissions.

### Expiration

Most `JWT Tokens` have an expiration date. The easiest way to cache the token is to store it in memory (also store its expiration time) and then before every API call, check if the token is valid.

## Swagger (Open API)

This serves as an interactive web UI for users to read and experiment with a specific API. Commonly used in industry.
