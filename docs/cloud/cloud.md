# Cloud

## Application Blueprint

Defines the software stack and its immediate dependencies. Focuses on what the application needs to run successfully.

Normally includes container definitions (Docker/Kubernetes), application code location, environment variables, local service dependencies (ie. database, cache).

**Goal:** An application blueprint helps make the app portable and reproducible. Any developer cna use the blueprint to spin up the exact same version of a service on their local.

## Environment Blueprint

Also known as IaC (Infrastructure as Code) defines the ecosystem an application lives on.

Typically includes Networking (VPCs, subnets), Security Groups, IAM roles/permissions, logging, and shared resources

**Goal:** Provide a "production-ready" stage that ensures any application deployed into this environment is automatically behind a firewall and integrated with the company's identity provider
