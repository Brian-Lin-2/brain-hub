# Authentication

This is important because it let's your code connect to Azure resources.

## Core Concepts

- `Connection Strings` - Single text string that grants you access to a service. Usually consists of the endpoint, account name, access, and secret key. Used in `Azure SQL`, `Azure Storage Accounts`, and `Azure Service Bus (ASB)`.
- `Account Keys` - String that gives full admin access to an `Azure Storage Account`. Best practice is to use this to generate a `Shared Access Signature (SAS) Token` that grants temporary and restricted access to the storage account.
- `Client Secret` - Key that acts as a password a `Service Principal` uses to authenticate.
- `Certificates` - A `.pfx` or `.pem` file that uses asymmetric encryption (public/private keys) to store permissions. Mainly used for enterprise-grade auth.
