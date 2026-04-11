# API

## Authorization

Generally for most authorized HTTP requests to the API, a user would need to pass a `JWT Token` in the Authorization header to allow for access with the API endpoints.

### API Scope

An API Scope is a string that defines what access is allowed for a certain application.

For a valid `JWT Token` to be generated, an API Scope normally needs to be baked inside this token to allow for specific permissions.

## Swagger (Open API)

This serves as an interactive web UI for users to read and experiment with a specific API. Commonly used in industry.
