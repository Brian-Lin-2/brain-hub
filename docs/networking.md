# Networking

## VPC & Subnets

Everything in the cloud lives inside a VPC. A VPC is just a secure, isolated private network hosted within a public cloud provider's infrastructure. This is just the network itself basically.

### Public vs Private Subnets

In corporate, databases and internal APIs should always be in private subnets with no direct internet access. Only Load Balances sit in public subnets

### CIDR Notation

This is important to understand what IPs fall within your VPC

## Security Groups and Network Access Control List (NACL)

Security groups are stateful and operate at the instance level. If controls what traffic is allowed in and out of specific applications.

NACLs are stateless and operate at the subnet level. They help block broader ranges.

## DNS

This one is self-explanatory. This is how applications talk to each other both privately and publicly.

## Load Balancing and Ingress

You never point traffic directly to a server. Normally it goes through a load balancer or ingress controller that then decides where to redirect the traffic.

Health checks are normally sent here to ensure that the deployment is still up and running.

## Network Firewall Request

This is the standard request you need to make to allow two applications to talk to each other. Typically operates at the Link Layer

**Things to provide:**

1. `Source IP/Subnet` - This tells the networking team exactly where the traffic is coming from
2. `Destination IP/Subnet` - This tells the networking team exactly where the traffic is being route to
3. `Port` - This tells the networking team exactly which port on the ip/subnet we need to allow traffic in
4. `Protocol` - This tells the networking team exactly which protocol to allow these requests to be sent to

> Firewall requests "open the door" for guests to enter

## WAF (Web Application Firewall) Requests

Operates at the Application Layer. Doesn't care much about IPs but more so about the contents of the web request (HTTP/HTTPs). You typically make these requests for applications that need to accept/send HTTP requests. You use this in conjunction with firewall requests. While firewall requests, allow ANY traffic in as long as it matches the correct ips/ports, a WAF will only allow correct HTTP/HTTPS requests through. Malformed ones get rejected.

> WAF requests inspect the guests that are entering

## DNS Requests

This is straightforward, you're just provisioning a new `A Record` or `CNAME` for your IP.

## SSL/TLS Certificate Requests

When you have a domain, you need to request for a cert to make sure traffic is encrypted.
