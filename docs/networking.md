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

## Useful CLI Commands

### curl

`curl` is the most common HTTP/HTTPS client on the command line. It also speaks many other protocols (FTP, SFTP, LDAP, SMTP, WebSockets with the right flags, etc.).

#### Basics

| Syntax | Purpose |
|--------|---------|
| `curl https://example.com` | GET; prints the body to stdout. |
| `curl -I https://example.com` | HEAD only (headers, no body). |
| `curl -X POST https://api.example.com/items` | Set the HTTP method (GET is default without a body). |
| `curl -v https://example.com` | Verbose (request + response headers); `-vv` is extra noisy. |
| `curl -s https://example.com` | Silent progress meter; pairs well with `-S` (`-sS`) so errors still print. |

#### Headers, body, and JSON

| Syntax | Purpose |
|--------|---------|
| `curl -H "Authorization: Bearer $TOKEN" URL` | Add or override a header; repeat `-H` for multiple headers. |
| `curl -H "Content-Type: application/json" -d '{"a":1}' URL` | Send a POST body (`Content-Type` is explicit). |
| `curl --json '{"a":1}' URL` | Shorthand for JSON: sets `Content-Type: application/json` and sends the body (curl 7.82+). |
| `curl --data-urlencode "q=hello world" "https://example.com/search"` | Encode form/query values safely (often with `-G` to append query string — see below). |
| `-d @file.json` | Body from file; `@-` reads stdin. |
| `-d`, `--data`, `--data-raw`, `--data-binary` | Default POSTs as `application/x-www-form-urlencoded` unless you set `Content-Type`. `--data-raw` does not strip a leading `@` from the string literal. `--data-binary` sends bytes as-is without extra processing. |

#### Query string vs POST body

| Syntax | Purpose |
|--------|---------|
| `curl -G URL --data-urlencode "foo=bar"` | `-G`: turn `-d/--data*` into GET query parameters instead of a POST body. |

#### Redirects, output, downloads

| Syntax | Purpose |
|--------|---------|
| `curl -L URL` | Follow redirects (`301/302`). |
| `-o out.html URL` | Write response body to `out.html`. |
| `-O URL` | Save using the remote filename from the URL path. |
| `-C - -O URL` | Resume interrupted download (`-` after `-C` means auto). |
| `-J -O URL` | Use filename from `Content-Disposition` when present (`-J` implies `-O` behavior carefully — read `man curl`). |

#### Timeouts and failures

| Syntax | Purpose |
|--------|---------|
| `--connect-timeout 5 --max-time 30 URL` | Cap connect vs total transfer time (seconds); good for scripts. |
| `-f`, `--fail` | Exit nonzero on HTTP 4xx/5xx even if a body is returned (silent unless `-S`/`--show-error`). |

#### TLS, certs, SNI / local testing

| Syntax | Purpose |
|--------|---------|
| `curl -k https://bad-cert.example.com` | **Insecure**: skip TLS cert verification (`--insecure`). Use only when you understand the risk. |
| `curl --cacert chain.pem URL` | Trust a specific CA bundle. |
| `curl --cert client.pem --key key.pem URL` | Client certificate authentication (paths vary by PEM layout). |
| `curl --resolve example.com:443:127.0.0.1 https://example.com/` | Force `example.com:443` to resolve to `127.0.0.1` (test vhosts/ingress locally without editing `/etc/hosts`). |

#### Auth, cookies, proxies

| Syntax | Purpose |
|--------|---------|
| `-u user:password URL` | HTTP Basic (`Authorization: Basic ...`); `user:` with blank password avoids prompting in some setups. |
| `-b 'name=value' URL` or `-b cookies.txt` | Send cookies (string or Netscape-format file from a prior request). |
| `-c jar.txt URL` | Save response `Set-Cookie` lines to `jar.txt` for reuse with `-b`. |
| `-x http://proxy:8080 URL` or `http_proxy` / `https_proxy` env | HTTP(S) proxy (corporate forward proxies often here). |
| `-x socks5://host:1080 URL` | SOCKS5 proxy. |
| `-U user:password` | Proxy authentication (with `-x`). |
| `--noproxy '*'` or `no_proxy` env | Bypass proxy for listed hosts (shell env is a common pattern: `NO_PROXY=localhost,127.0.0.1`). |

#### Debugging & HTTP options

| Syntax | Purpose |
|--------|---------|
| `curl -D - URL` | Write response headers to stdout (`-` means stdout); body still prints unless redirected with `-o`. |
| `-i`, `--include` | Include response headers *before* the body in stdout. |
| `-A 'MyAgent/1.0' URL` | Set `User-Agent` (servers often behave differently based on UA). |
| `-e https://referer.example/` | Set `Referer` header (`--referer`). |
| `--compressed` | Ask for compressed transfer and decompress if the response supports it. |
| `--http1.1` / `--http2` | Pin HTTP version (availability depends on `curl`/OpenSSL/nghttp2 build). |

`-X` caveat: **`curl -X POST` does not magically add a body**; you still need `-d`/`-F`/`--json` etc. Avoid `-X GET`; default is GET and forcing it can confuse redirect handling.

`-F`: multipart form upload (`-F file=@photo.jpg -F meta=hello`).

Timing one-liner (sizes and times): `-w '%{http_code} connect:%{time_connect} ttfb:%{time_starttransfer} total:%{time_total}\n' -o /dev/null -sS URL`

### Other CLI tools (networking)

- **`dig` / `host` / `nslookup`** — DNS: e.g. `dig +short api.example.com A`, `dig +trace` to trace resolution. Prefer `dig` and `host` for scripting; `nslookup` is interactive-friendly.
- **`ping`** — Reachability and round-trip time (often ICMP). Use `-c` for a fixed count. Blocked ICMP does **not** always mean HTTP/HTTPS is down.
- **`traceroute` / `tracert` (Windows)** — Per-hop path to a destination; missing hops are common behind firewalls or load balancers.
- **`mtr`** — Combines ping and traceroute over time; useful for intermittent packet loss.
- **`nc` (`netcat`)** — Raw TCP/UDP: listen `nc -l 9000`, connect `nc host 443`, quick open-port check `nc -vz host 443` (flags differ: BSD `nc` vs GNU `netcat`).
- **`nmap`** — Port scan and service detection; use only on networks you are allowed to test.
- **`ss`** — Socket stats on Linux: e.g. `ss -tlnp` (listening TCP + process). **`netstat`** is the older cross-platform cousin; on macOS, `netstat -an` is still common.
- **`ip route` / `route`** — Default gateway and routing table. On macOS, `netstat -rn` or `route -n get default` for quick checks.
- **`ifconfig` / `ip addr`** — Interface IP addresses and link state. Linux: prefer `ip link` / `ip addr`; macOS: `ifconfig` is typical.
- **`openssl s_client`** — TLS/debug: `openssl s_client -connect host:443 -servername host` checks **SNI** and shows the cert chain.
- **`ssh` / `scp` / `sftp`** — Remote shell and file copy. Tunnels: `ssh -L 8080:internal:80 bastion` (local forward), `-R` (remote forward), `-D` (SOCKS proxy on the client).
- **`tcpdump` / `tshark`** — Packet capture (usually needs admin); powerful filters; follow corporate policy before capturing on production networks.
- **`wget`** — Recursive mirroring and simple downloads; overlaps with `curl` for basic HTTP fetches.
- **`whois`** — Domain / IP registration data (varies by TLD and RDAP migration).
- **`arp -a`** — IPv4 neighbors on the local L2 segment (less central with modern Wi‑Fi and routing, still useful on LANs).

Corporate environments: `curl -x`, `HTTPS_PROXY` / `HTTP_PROXY` / `NO_PROXY`, and VPN/ZTNA interact; wrong proxy settings often produce timeouts or `407` proxy auth errors.

## Corporate Networking

The flow normally goes like this:

`User Request` -> `VPN/ZTNA` -> `Firewall` -> `Proxy`

### Proxy

A proxy acts as a middleman between a client and a server. A `Corporate Proxy` is a special type of proxy that is between you and a server. Everytime you visit something, the corporate proxy determines if its allowed.

At work, almost every endpoint cannot be routed to directly (will get a network error) and instead must be always routed to the proxy which will then reroute it to the appropriate endpoint.

### Firewall

A firewall focuses on blocking or allowing traffic based on security rules. Typically, a request must pass through the firewall as a first step. From there, the proxy will determine where it is routed to.

### VPN

A private networking tunnel that allows you to create a secure connection between your internet and another network (typically a corporate network). This allows you to access applications that can normally only be accessible through the corporate internet.

There are many corporate applications that are hosted in the corporation's private network. This means if you're on the regular internet, there's no public IP address for you to connect to. In order to solve this, you must be on VPN to have direct access to the private network.

### ZTNA

ZTNA (Zero Trust Network Access) is the modern successor to VPN. While VPN creates an encrypted tunnel which gives you access to all resources inside a network, ZTNA never trusts your device. When you want to connect to a specific application, ZTNA looks at a combination of "signals" before it lets you access the application.

These signals usually consist of `Identity, Device Health, Context, Least Privilege`.

The main difference between VPN and ZTNA is full network access vs app-specific access. When using ZTNA, you always verify before accessing each specific application. Other applications in the network are not visible to you until those checks are complete.

## Corporate Networking Requests

Many times you are experiencing networking related issues in your application, it must be solved by going to the appropriate networking team and creating a request.

### Network Firewall Request

This is the standard request you need to make to allow two applications to talk to each other. Typically operates at the Link Layer

**Things to provide:**

1. `Source IP/Subnet` - This tells the networking team exactly where the traffic is coming from
2. `Destination IP/Subnet` - This tells the networking team exactly where the traffic is being route to
3. `Port` - This tells the networking team exactly which port on the ip/subnet we need to allow traffic in
4. `Protocol` - This tells the networking team exactly which protocol to allow these requests to be sent to

> Firewall requests "open the door" for guests to enter

### WAF (Web Application Firewall) Requests

Operates at the Application Layer. Doesn't care much about IPs but more so about the contents of the web request (HTTP/HTTPs). You typically make these requests for applications that need to accept/send HTTP requests. You use this in conjunction with firewall requests. While firewall requests, allow ANY traffic in as long as it matches the correct ips/ports, a WAF will only allow correct HTTP/HTTPS requests through. Malformed ones get rejected.

> WAF requests inspect the guests that are entering

### DNS Requests

This is straightforward, you're just provisioning a new `A Record` or `CNAME` for your IP.

### SSL/TLS Certificate Requests

When you have a domain, you need to request for a cert to make sure traffic is encrypted.
