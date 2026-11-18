# Commands used

## Connect to the instance (Windows PowerShell)

Restrict the key file's permissions (SSH refuses keys other users can read):

```powershell
icacls .\nginx-key.pem /inheritance:r
icacls .\nginx-key.pem /grant:r "$($env:USERNAME):(R)"
```

Connect with the instance's **public IPv4** (Ubuntu's default user is `ubuntu`):

```powershell
ssh -i .\nginx-key.pem ubuntu@<EC2-PUBLIC-IP>
```

## Install and run NGINX (Ubuntu)

```bash
sudo apt update
sudo apt install -y nginx
sudo systemctl enable nginx
sudo systemctl start nginx
sudo systemctl status nginx
```

## Test and troubleshoot (PowerShell)

Is port 22 reachable? (`TcpTestSucceeded` is the line that matters; ping is blocked by default on EC2.)

```powershell
Test-NetConnection <EC2-PUBLIC-IP> -Port 22
```

What does the domain resolve to? With the Cloudflare proxy on, this returns Cloudflare IPs, not the EC2 IP:

```powershell
nslookup nginx.mahadgaal.co.uk 1.1.1.1
```

Clear the local DNS cache:

```powershell
ipconfig /flushdns
```

Check the web server over HTTP:

```powershell
curl.exe -I http://nginx.mahadgaal.co.uk
```

Find my public IPv4 (for the SSH security group rule):

```powershell
curl.exe -4 ifconfig.me
```

## Cloudflare DNS record

| Type | Name | Content | Proxy |
|------|------|---------|-------|
| A | `nginx` | EC2 public IPv4 | Proxied (SSL/TLS mode: Flexible) |