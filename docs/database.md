# Database

## Snowflake

Cloud native SaaS solution for databases. Serves as an intermediary between you and the cloud provider's infrastructure.

Unique architecture helps separate storage from compute. Also the it queries based on column not rows allowing for efficient performance.

**Key Features:**

`Database Storage` - Stored on the cloud allowing for near infinite scaling of storage that is independent of performance. However, even though its stored on the cloud provider you can only access the data through Snowflake.
`Virtual Warehouses` - Snowflake uses "Virtual Warehouses" to perform processing tasks and queries. This keeps compute separate from storage allowing for autoscaling and parallelism.
`Cloud Services` - This is the control plane of the system. Helps coordinate everything across the platform such as auth, IAM, query optimization, and metadata management.
