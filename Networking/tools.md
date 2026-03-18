# Networking Tools

Common CLI tools for network diagnostics and troubleshooting.

---

## ping

Test connectivity to a host using ICMP.

```bash
ping google.com               # Continuous ping
ping -c 4 google.com          # Send 4 packets then stop
ping -i 0.5 google.com        # Ping every 0.5 seconds
```

---

## traceroute / tracepath

Show the route packets take to reach a destination.

```bash
traceroute google.com
tracepath google.com          # Similar, doesn't require root
```

---

## nslookup / dig

DNS lookups.

```bash
nslookup google.com                          # Basic lookup
dig google.com                               # Detailed DNS lookup
dig google.com A                             # Query A record
dig google.com MX                            # Query MX records
dig @8.8.8.8 google.com                      # Use specific DNS server (Google)
dig +short google.com                        # Short output (just IPs)
dig -x 8.8.8.8                               # Reverse DNS lookup
```

---

## curl

Transfer data with URLs — ideal for testing HTTP APIs.

```bash
curl https://example.com                     # GET request
curl -I https://example.com                  # HEAD request (headers only)
curl -X POST https://api.example.com/users \
  -H "Content-Type: application/json" \
  -d '{"name": "Alice"}'                     # POST with JSON body
curl -u user:pass https://example.com        # Basic authentication
curl -o output.html https://example.com      # Save response to file
curl -L https://example.com                  # Follow redirects
curl -v https://example.com                  # Verbose (shows request/response headers)
curl -w "%{http_code}" -o /dev/null -s https://example.com  # Just print status code
```

---

## wget

Download files from the internet.

```bash
wget https://example.com/file.tar.gz         # Download a file
wget -O custom-name.tar.gz https://...       # Save with custom name
wget -r https://example.com                  # Recursive download
wget -q https://...                          # Quiet mode
```

---

## netstat / ss

View network connections, routing tables, and interface statistics.

```bash
ss -tuln                    # Show listening TCP/UDP sockets
ss -tp                      # Show TCP sockets with process info
ss -s                       # Summary statistics
netstat -tuln               # (older tool, similar to ss)
netstat -rn                 # Show routing table
```

---

## ip

Modern replacement for `ifconfig` and `route`.

```bash
ip a                        # Show all interfaces and IP addresses
ip link                     # Show link-layer info
ip route                    # Show routing table
ip route add 10.0.0.0/8 via 192.168.1.1    # Add a static route
ip addr add 192.168.1.100/24 dev eth0      # Assign an IP to an interface
```

---

## nmap

Network scanner — use only on networks you own or have permission to scan.

```bash
nmap 192.168.1.1              # Scan a single host
nmap 192.168.1.0/24           # Scan a subnet
nmap -p 80,443 192.168.1.1    # Scan specific ports
nmap -sV 192.168.1.1          # Detect service versions
nmap -O 192.168.1.1           # OS detection
nmap -A 192.168.1.1           # Aggressive scan (OS + version + scripts)
```

---

## tcpdump

Capture and analyse network traffic on an interface.

```bash
tcpdump -i eth0               # Capture on interface eth0
tcpdump -i any                # Capture on all interfaces
tcpdump port 80               # Filter by port
tcpdump host 192.168.1.10    # Filter by host
tcpdump -w capture.pcap       # Save to file (open with Wireshark)
tcpdump -r capture.pcap       # Read from file
tcpdump -n -v -i eth0 port 443  # Verbose, numeric output, HTTPS traffic
```

---

## nc (netcat)

Versatile tool for TCP/UDP connections — often called the "Swiss Army knife" of networking.

```bash
nc -zv 192.168.1.10 22        # Check if port 22 is open
nc -l 9000                    # Listen on port 9000
nc 192.168.1.10 9000          # Connect to port 9000
echo "Hello" | nc 192.168.1.10 9000  # Send data
```

---

## openssl

Test TLS connections and inspect certificates.

```bash
openssl s_client -connect example.com:443         # Connect and show cert chain
openssl s_client -connect example.com:443 </dev/null 2>/dev/null | openssl x509 -noout -dates  # Cert expiry
openssl x509 -in cert.pem -text -noout           # Inspect a local certificate file
```

---

## iptables (Linux Firewall)

```bash
iptables -L -n -v             # List all rules with details
iptables -A INPUT -p tcp --dport 22 -j ACCEPT    # Allow SSH inbound
iptables -A INPUT -j DROP                         # Drop all other inbound
iptables -D INPUT 3                               # Delete rule number 3
iptables-save > /etc/iptables/rules.v4           # Save rules
iptables-restore < /etc/iptables/rules.v4        # Restore rules
```
