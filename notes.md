# Networking notes

## How a request reaches the server

1. The browser asks DNS for `nginx.mahadgaal.co.uk`.
2. Cloudflare is the authoritative DNS provider and returns an address. With the proxy on, that is a Cloudflare IP; with it off (DNS only), it is the EC2 IP.
3. The browser connects to that IP on port 80 (HTTP) or 443 (HTTPS).
4. The security group allows or drops the traffic before it reaches the instance.
5. NGINX, listening on port 80, serves the default page.

## Key concepts

- **A record:** hostname to IPv4 address. (AAAA is the IPv6 equivalent; CNAME is an alias to another name.)
- **Ports:** 22 SSH, 80 HTTP, 443 HTTPS.
- **Security group:** stateful, allow-only firewall attached to the instance. Return traffic for an allowed connection is permitted automatically.
- **CIDR:** `/32` means a single address; `0.0.0.0/0` means every IPv4 address.
- **Public vs private IP:** the public address is auto-assigned and changes on stop/start; an Elastic IP stays fixed.
- **Cloudflare proxy modes:** DNS only (grey cloud) returns the origin IP. Proxied (orange cloud) returns Cloudflare IPs and forwards traffic to the origin.
- **Cloudflare SSL/TLS modes:** Flexible (HTTP to origin), Full (HTTPS, any certificate), Full (strict) (HTTPS, valid certificate).

## Error codes seen

- **Connection timed out (SSH):** packets not reaching port 22, so check the security group and the IP.
- **Permission denied (publickey):** the network path works but the key or username is wrong.
- **Cloudflare 522:** Cloudflare reached the edge but could not connect to the origin.

## My lesson notes

<!-- Add notes from the course lessons here. -->