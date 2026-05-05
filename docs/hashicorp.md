# Hashicorp

Hashicorp is an open source highly secure vault you can use to store secrets. Think of it as a highly secure, virtual filesystem where every action is a `path` and every component is a `plugin`,

## Overview

### Cluster

A Vault Cluster is the giant unit. It usually consists of multiple Vault nodes (one active, multiple backups) that share a single data storage backend.

**Important Concepts:**

- `Barrier` - This layer acts as a vault door. All data goes through this and is encrypted before leaving the cluster.
- `Unsealing` - When a cluster starts, it is "sealed" meaning you must provide "unseal keys" to reconstruct the master key and begin processing requests.

### Secrets Engine

This is a plugin that handles specific types of data. You "mount" them at specific paths.

**Common Ones:**

- `Key-Value`: Stores static secrets (ie. `passwords`)
- `Dynamic Engines`: Generate secrets on the fly
- `Transit Engine`: Handles "encryption as a service" without storing the data itself

### Paths

Everything in Vault is a path. You need to use a URL-like structure to retrieve it since everything in a Vault is stored like a filesystem.

### Permissions & Policies

Vault is `Deny by Default`. An entity will need an attached Policy which defines what Capabilities are allowed on specific paths.

```Terraform
# Example Policy
path "secret/data/mlops/*" {
  capabilities = ["create", "read", "update"]
}

path "auth/token/lookup-self" {
  capabilities = ["read"]
}
```

### Roles

A Role is a collection of settings that defines how a user or machine interacts with a specific engine.

## Full Flow

1. `Authenticate` - Client needs to log in via an Auth Method. A common one is JWT.
2. `Token Issue` - The Vault then looks at the Role assigned to that identity, finds the associated Policies, and then issues a token
3. `Request` - Client sends a request to a Path
4. `Authorize` - Vault checks the Policies attached to the token to see if the user has proper permissions
5. `Generate` - If authorized, the Secrets Engine then generates the credential and returns it to the client
