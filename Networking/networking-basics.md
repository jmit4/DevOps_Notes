# Networking Basics

Core networking concepts relevant to DevOps and infrastructure work.

---

## OSI Model (Quick Reference)

| Layer | Name | Examples |
|---|---|---|
| 7 | Application | HTTP, HTTPS, DNS, SSH, FTP |
| 6 | Presentation | TLS/SSL, encoding |
| 5 | Session | Session management |
| 4 | Transport | TCP, UDP |
| 3 | Network | IP, ICMP, routing |
| 2 | Data Link | Ethernet, MAC addresses |
| 1 | Physical | Cables, Wi-Fi signals |

---

## IP Addressing

### IPv4

- Format: `192.168.1.10`
- 32-bit address split into 4 octets
- Private ranges (RFC 1918):
  - `10.0.0.0/8`
  - `172.16.0.0/12`
  - `192.168.0.0/16`

### CIDR Notation

| CIDR | Subnet Mask | Addresses |
|---|---|---|
| `/32` | 255.255.255.255 | 1 (single host) |
| `/24` | 255.255.255.0 | 256 (254 usable) |
| `/16` | 255.255.0.0 | 65,536 |
| `/8` | 255.0.0.0 | 16,777,216 |

### Common Addresses

| Address | Purpose |
|---|---|
| `127.0.0.1` | Loopback (localhost) |
| `0.0.0.0` | All interfaces |
| `255.255.255.255` | Broadcast |

---

## TCP vs UDP

| Feature | TCP | UDP |
|---|---|---|
| Connection | Connection-oriented | Connectionless |
| Reliability | Guaranteed delivery | Best-effort |
| Order | In-order | No guarantee |
| Speed | Slower | Faster |
| Use cases | HTTP, SSH, databases | DNS, video streaming, gaming |

---

## Common Ports

| Port | Protocol | Service |
|---|---|---|
| 22 | TCP | SSH |
| 25 | TCP | SMTP |
| 53 | TCP/UDP | DNS |
| 80 | TCP | HTTP |
| 443 | TCP | HTTPS |
| 3306 | TCP | MySQL |
| 5432 | TCP | PostgreSQL |
| 6379 | TCP | Redis |
| 8080 | TCP | HTTP (alternate) |
| 27017 | TCP | MongoDB |

---

## DNS

The Domain Name System translates domain names to IP addresses.

```
Browser → Recursive Resolver → Root Server → TLD Server → Authoritative Server → IP
```

### Record Types

| Record | Purpose | Example |
|---|---|---|
| A | Domain → IPv4 | `example.com → 93.184.216.34` |
| AAAA | Domain → IPv6 | `example.com → 2606:...` |
| CNAME | Alias → another domain | `www → example.com` |
| MX | Mail server | Priority + mail host |
| TXT | Arbitrary text | SPF, DKIM, domain verification |
| NS | Name servers for domain | `ns1.example.com` |
| PTR | Reverse DNS (IP → domain) | `34.216.184.93.in-addr.arpa` |

---

## HTTP/HTTPS

### HTTP Methods

| Method | Purpose |
|---|---|
| GET | Retrieve a resource |
| POST | Create a resource |
| PUT | Replace a resource |
| PATCH | Partially update a resource |
| DELETE | Delete a resource |

### Common Status Codes

| Code | Meaning |
|---|---|
| 200 | OK |
| 201 | Created |
| 301 | Moved Permanently |
| 302 | Found (temporary redirect) |
| 400 | Bad Request |
| 401 | Unauthorized |
| 403 | Forbidden |
| 404 | Not Found |
| 500 | Internal Server Error |
| 502 | Bad Gateway |
| 503 | Service Unavailable |

---

## Load Balancing

| Algorithm | Description |
|---|---|
| Round Robin | Distribute requests sequentially |
| Least Connections | Send to server with fewest active connections |
| IP Hash | Same client always goes to same server |
| Weighted | Distribute based on server capacity |

### Types

- **Layer 4** (Transport) — routes based on IP and TCP/UDP port
- **Layer 7** (Application) — routes based on HTTP headers, URL, cookies

---

## Firewalls & Security Groups

Rules are evaluated top-down; first match wins (in most systems).

```
ALLOW  tcp  0.0.0.0/0  → port 443
ALLOW  tcp  10.0.0.0/8 → port 22
DENY   all  0.0.0.0/0  → all
```

- **Security Groups** (AWS) — stateful; return traffic automatically allowed
- **NACLs** (AWS) — stateless; must explicitly allow inbound AND outbound
- **iptables** (Linux) — rule-based packet filtering

---

## VPN & Tunnelling

| Technology | Use Case |
|---|---|
| VPN (IPSec, WireGuard, OpenVPN) | Secure remote access or site-to-site |
| SSH Tunnel | Port forwarding through an SSH connection |
| TLS | Encrypt data in transit (HTTPS, etc.) |
