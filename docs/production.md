# Production

For a production release, there's a formal process to follow before you can successfully deploy an application/model.

## Production Readiness Review (PRR)

Happens at the end of the development cycle (right before launch). Answers the question of: \*Is this app stable enough to survive in the real world?\*

**It's mainly just a checklist:**

- `Monitoring & Alerting` - Do we have dashboards? Will someone get paged if the model starts returning 400s/500s?
- `Scalability` - Can the network handle spikes in traffic? This is typically where load testing is performed.
- `Security` - Have all the appropriate Firewall and WAF requests been approved? Is traffic encrypted?
- `BCDR` - Do we have disaster recovery in place?
- `Documentation` - Is there detailed docs about the product?
