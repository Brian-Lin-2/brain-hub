# API

## API Gateway

API Gateways (ie. `APISIX`, `NGINX`) acts as a traffic entry point for your applications.

### External vs Internal

External gateways are usually built for traffic coming from the public internet. In corporate, it's meant as a door for public requests to flood into the internal network.

Internal gateways are built for internal traffic. Perfect for low-latency and performance.

### APISIX

Dynamic, real-time, high-performance API Gateway that is designed to handle massive amounts of traffic with ultra-low latency.

**Features:**

- `Traffic Management` - Load balancing, dynamic routing, "canary" releases
- `Security` - Provides authentication (Key-Auth, JWT, OAuth2), IP whitelisting/blacklisting, and DDoS protection
- `Observability` - Integration with Prometheus for monitoring
- `Extensibility` - Supports plugins
- `Cloud Native` - Built for Kubernetes and microservice environments

#### B2B

Stands for Business to Business. Focuses on partner integration rather than consumer based integration.

**Key Notes:**

- `Custom Authentication` - Instead of standard logins, you use `mTLS` (mutual TLS) or fixed `HMAC` signatures to ensure both parties are exactly who they say they are
- `IP Whitelisting` - Since you know exactly where the traffic is from, you can whitelist static known IPs/subnets
- `Tiered Rate Limiting` - You can enforce specific quotas

## Authorization

Generally for most authorized HTTP requests to the API, a user would need to pass a `JWT Token` in the Authorization header to allow for access with the API endpoints.

### API Scope

An API Scope is a string that defines what access is allowed for a certain application.

For a valid `JWT Token` to be generated, an API Scope normally needs to be baked inside this token to allow for specific permissions.

### Expiration

Most `JWT Tokens` have an expiration date. The easiest way to cache the token is to store it in memory (also store its expiration time) and then before every API call, check if the token is valid.

## Swagger (Open API)

This serves as an interactive web UI for users to read and experiment with a specific API. Commonly used in industry.
