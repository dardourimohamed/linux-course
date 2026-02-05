# Chapter 11: Networking Basics

## Learning Objectives

By the end of this chapter, you will be able to:

- [x] Understand IP addresses, ports, and network interfaces
- [x] Check network connectivity and diagnose issues
- [x] Use SSH for secure remote access
- [x] Transfer files securely over the network
- [x] Troubleshoot common network problems
- [x] Understand basic network security concepts

## Prerequisites

- Completed Chapter 10: Shell Scripting
- Basic understanding of the file system
- Comfortable with terminal commands

---

## Understanding Networking

### What is a Network?

A **network** connects computers together so they can share resources. Your Linux machine is likely connected to:

```
┌─────────────────────────────────────────────────────────┐
│                      The Internet                        │
│                        (Cloud)                           │
│                           ↑                              │
│                    ┌──────┴──────┐                       │
│                    │   Router    │                       │
│                    │  192.168.1.1│                       │
│                    └──────┬──────┘                       │
│                           │                              │
┌───────────────────────────┼──────────────────────────────┐
│ Local Network (LAN)        │   192.168.1.0/24            │
│                            │                             │
│  ┌─────────────────────┐   │   ┌─────────────────────┐   │
│  │ Your Linux Machine  │───┼───│    Other Device     │   │
│  │   192.168.1.100     │       │   192.168.1.101     │   │
│  └─────────────────────┘       └─────────────────────┘   │
└─────────────────────────────────────────────────────────┘
```

### Key Networking Concepts

| Concept | Description | Example |
|---------|-------------|---------|
| **IP Address** | Unique identifier for a device | 192.168.1.100 |
| **Subnet Mask** | Defines network range | 255.255.255.0 |
| **Gateway** | Route to other networks | 192.168.1.1 (router) |
| **DNS** | Translates names to IPs | google.com → 142.250.x.x |
| **Port** | Specific service on a machine | 22 for SSH, 80 for web |
| **MAC Address** | Hardware network ID | 00:1A:2B:3C:4D:5E |

---

## Viewing Network Configuration

### ip Command (Modern)

The `ip` command is the modern replacement for `ifconfig`.

```bash
# Show all network interfaces
ip addr

# Show IPv4 addresses only
ip -4 addr

# Show specific interface
ip addr show eth0
ip a show wlan0    # short form

# Show routing table
ip route

# Show neighbors (ARP table)
ip neigh
```

### Understanding ip addr Output

```bash
$ ip addr
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536
    inet 127.0.0.1/8 scope host lo
           ^^^^^^^^^^^^  Local loopback (your own machine)

2: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel
    inet 192.168.1.100/24 brd 192.168.1.255 scope global dynamic eth0
           ^^^^^^^^^^^^^^^^  Your IP on local network
           ^^^^             Subnet mask (24 = 255.255.255.0)
```

### Interface States

| State | Meaning |
|-------|---------|
| **UP** | Interface is active |
| **DOWN** | Interface is disabled |
| **UNKNOWN** | Connection state unknown |

### Network Interface Types

| Interface | Description |
|-----------|-------------|
| **lo** | Loopback (localhost) |
| **eth0** | Ethernet (wired) |
| **wlan0** | Wireless (WiFi) |
| **enp0s3** | Modern naming for ethernet |
| **wlp3s0** | Modern naming for wireless |

---

## Checking Connectivity

### ping - Test Reachability

`ping` sends ICMP packets to test if a host is reachable.

```bash
# Ping Google (Ctrl+C to stop)
ping google.com

# Ping specific number of times
ping -c 4 8.8.8.8

# Ping with interval
ping -i 2 192.168.1.1
```

```bash
$ ping -c 4 google.com
PING google.com (142.250.185.46) 56(84) bytes of data.
64 bytes from lga25s72-in-f14.1e100.net (142.250.185.46): icmp_seq=1 ttl=115 time=12.3 ms
64 bytes from lga25s72-in-f14.1e100.net (142.250.185.46): icmp_seq=2 ttl=115 time=11.8 ms
64 bytes from lga25s72-in-f14.1e100.net (142.250.185.46): icmp_seq=3 ttl=115 time=13.1 ms
64 bytes from lga25s72-in-f14.1e100.net (142.250.185.46): icmp_seq=4 ttl=115 time=12.9 ms

--- google.com ping statistics ---
4 packets transmitted, 4 received, 0% packet loss
round-trip min/avg/max/stddev = 11.8/12.5/13.1/0.5 ms
```

### traceroute - Trace Packet Path

Shows the route packets take to reach a destination.

```bash
# Trace route to Google
traceroute google.com

# Or use tracepath (simpler)
tracepath google.com
```

```bash
$ traceroute google.com
traceroute to google.com (142.250.185.46), 30 hops max
 1  _gateway (192.168.1.1)  0.123 ms
 2  10.0.0.1  5.432 ms
 3  72.14.215.85  12.123 ms
 ...
```

### nslookup / dig - DNS Queries

Query DNS servers to resolve domain names.

```bash
# Look up IP for domain
nslookup google.com

# More detailed query
dig google.com

# Query specific DNS record
dig mx gmail.com    # Mail servers
```

```bash
$ nslookup google.com
Server:         127.0.0.53
Address:        127.0.0.53#53

Name:   google.com
Address: 142.250.185.46
```

### Testing Web Connectivity

```bash
# Test HTTP request
curl -I https://google.com

# Test with details
curl -v https://example.com

# Test port connectivity
nc -zv google.com 80
```

---

## SSH - Secure Remote Access

**SSH** (Secure Shell) lets you securely connect to remote machines.

### Basic SSH Connection

```bash
ssh user@hostname

# Examples
ssh alice@192.168.1.100
ssh user@example.com
ssh root@server.example.com
```

### First Connection - Host Key Verification

```bash
$ ssh alice@192.168.1.100
The authenticity of host '192.168.1.100' can't be established.
ED25519 key fingerprint is SHA256:abc123...
This key is not known by any other names.
Are you sure you want to continue (yes/no/[fingerprint])? yes
Warning: Permanently added '192.168.1.100' (ED25519) to the list of known hosts.
alice@192.168.1.100's password:
```

### SSH Key-Based Authentication

Generate SSH keys to avoid typing passwords.

```bash
# Generate new key pair
ssh-keygen -t ed25519    # Modern, secure
# or
ssh-keygen -t rsa -b 4096

# Copy public key to remote server
ssh-copy-id alice@192.168.1.100

# Now login without password
ssh alice@192.168.1.100
```

### SSH Keys Explained

```
┌─────────────────────────────────────────────────────────┐
│                  Your Machine                           │
│  ┌────────────────┐    ┌────────────────┐             │
│  │ Private Key    │    │ Public Key     │             │
│  │ (id_ed25519)   │    │ (id_ed25519.pub)│             │
│  │ KEEP SECRET!   │    │ Share freely   │             │
│  └────────────────┘    └────────────────┘             │
│                                │                        │
│                                │ Copy this to server    │
│                                ▼                        │
└─────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────┐
│                  Remote Server                          │
│           ┌────────────────┐                            │
│           │ Public Key     │                            │
│           │ In ~/.ssh/     │                            │
│           │ authorized_keys│                            │
│           └────────────────┘                            │
└─────────────────────────────────────────────────────────┘
```

### Useful SSH Options

```bash
# Specify port (default is 22)
ssh -p 2222 user@host

# Connect with specific key
ssh -i ~/.ssh/mykey.pem user@host

# Verbose mode (debugging)
ssh -v user@host

# Execute command remotely
ssh user@host "ls -la /tmp"

# Local port forwarding
ssh -L 8080:localhost:80 user@host    # Access remote port 80 locally
```

### SSH Config File

Simplify connections with `~/.ssh/config`:

```bash
# Create config file
nano ~/.ssh/config
```

```ssh
Host myserver
    HostName 192.168.1.100
    User alice
    Port 22
    IdentityFile ~/.ssh/id_ed25519

Host webserver
    HostName example.com
    User admin
    Port 2222
```

```bash
# Now just use:
ssh myserver
ssh webserver
```

---

## File Transfer Over SSH

### scp - Secure Copy

Copy files between machines over SSH.

```bash
# Copy file to remote
scp localfile.txt user@host:/remote/path/

# Copy file from remote
scp user@host:/remote/file.txt /local/path/

# Copy directory (recursive)
scp -r localdir/ user@host:/remote/path/

# Copy with preserved attributes
scp -p file.txt user@host:/path/

# Specify port
scp -P 2222 file.txt user@host:/path/
```

### rsync - Sync Files

More efficient for large transfers and can sync directories.

```bash
# Sync local to remote
rsync -avz localdir/ user@host:remotedir/

# Sync remote to local
rsync -avz user@host:remotedir/ localdir/

# Dry run (preview)
rsync -avz --dry-run localdir/ user@host:remotedir/

# Delete files in destination that don't exist in source
rsync -avz --delete localdir/ user@host:remotedir/

# Show progress
rsync -avz --progress localdir/ user@host:remotedir/
```

### rsync Options

| Option | Meaning |
|--------|---------|
| `-a` | Archive mode (preserves permissions, times) |
| `-v` | Verbose (show what's happening) |
| `-z` | Compress during transfer |
| `--progress` | Show progress |
| `--delete` | Delete extra files in destination |
| `--dry-run` | Preview without copying |

---

## Network Troubleshooting

### Diagnostic Workflow

```
1. Check interface is up
   ip addr

2. Check local connectivity
   ping 127.0.0.1

3. Check gateway
   ping 192.168.1.1

4. Check DNS
   ping 8.8.8.8

5. Check name resolution
   ping google.com

6. Trace path
   traceroute google.com
```

### Common Issues and Solutions

| Problem | Diagnosis | Solution |
|---------|-----------|----------|
| No connection | `ip addr` shows DOWN | Bring interface up |
| Can't reach internet | Gateway unreachable | Check router connection |
| Can't browse sites | DNS failure | Use 8.8.8.8 as DNS |
| Port blocked | `nc -zv` fails | Check firewall |
| Slow connection | High `ping` times | Check bandwidth usage |

### Managing Network Interfaces

```bash
# Bring interface down
sudo ip link set eth0 down

# Bring interface up
sudo ip link set eth0 up

# Assign IP address
sudo ip addr add 192.168.1.100/24 dev eth0

# Remove IP address
sudo ip addr del 192.168.1.100/24 dev eth0
```

### Checking Open Ports

```bash
# List listening ports
ss -tulpn

# Show specific port
ss -tulpn | grep :22

# Alternative with netstat
netstat -tulpn
```

```bash
$ ss -tulpn
Netid State  Recv-Q Send-Q Local Address:Port    Peer Address:Port
tcp   LISTEN 0      128          0.0.0.0:22          0.0.0.0:*
tcp   LISTEN 0      5            127.0.0.1:631       0.0.0.0:*
```

### Firewall Basics

```bash
# Fedora (firewalld)
sudo firewall-cmd --list-all              # Show rules
sudo firewall-cmd --add-port=8080/tcp    # Open port
sudo firewall-cmd --reload                # Apply changes

# Debian (ufw)
sudo ufw status                           # Show status
sudo ufw allow 22                         # Allow SSH
sudo ufw enable                           # Enable firewall
```

---

## Network Configuration Files

### Understanding /etc/hosts

Local hostname to IP mapping (before DNS).

```bash
$ cat /etc/hosts
127.0.0.1   localhost
127.0.1.1   mycomputer
192.168.1.100  server.local

# You can add custom entries
```

### resolv.conf - DNS Configuration

```bash
$ cat /etc/resolv.conf
nameserver 127.0.0.53
options edns0 trust-ad
search .
```

---

## Practical Examples

### Example 1: Remote Server Management

```bash
# Connect to server
ssh admin@server.example.com

# Once connected, run commands remotely
sudo systemctl status nginx
sudo tail -f /var/log/nginx/access.log
```

### Example 2: Deploy Website

```bash
# Build locally
npm run build

# Copy to server
scp -r dist/* user@server:/var/www/html/

# Restart service on server
ssh user@server "sudo systemctl restart nginx"
```

### Example 3: Backup Remote Files

```bash
# Sync remote files to local backup
rsync -avz --delete user@server:/var/www/ ~/backups/server/
```

### Example 4: Network Diagnosis Script

```bash
#!/bin/bash
# network_check.sh

echo "=== Network Diagnosis ==="

# Check interfaces
echo "--- Network Interfaces ---"
ip addr | grep -E "^[0-9]|inet "

# Check gateway
echo ""
echo "--- Gateway Ping ---"
ping -c 2 $(ip route | grep default | awk '{print $3}')

# Check DNS
echo ""
echo "--- DNS Check ---"
nslookup google.com > /dev/null 2>&1
if [ $? -eq 0 ]; then
    echo "DNS: OK"
else
    echo "DNS: FAILED"
fi

# Check internet
echo ""
echo "--- Internet Connectivity ---"
ping -c 2 8.8.8.8 > /dev/null 2>&1
if [ $? -eq 0 ]; then
    echo "Internet: OK"
else
    echo "Internet: FAILED"
fi
```

### Example 5: Port Forwarding for Remote Access

```bash
# Forward local port 8080 to remote server's localhost:80
ssh -L 8080:localhost:80 user@server

# Now access in browser: http://localhost:8080
# (Routes to server's port 80)
```

---

## Security Best Practices

### SSH Security

1. **Use key-based auth, not passwords**
   ```bash
   ssh-keygen -t ed25519
   ssh-copy-id user@host
   ```

2. **Disable password authentication** (server-side)
   ```bash
   # Edit /etc/ssh/sshd_config
   PasswordAuthentication no
   ```

3. **Change default SSH port**
   ```bash
   # Edit /etc/ssh/sshd_config
   Port 2222
   ```

4. **Use firewall to limit access**
   ```bash
   sudo ufw allow from 192.168.1.0/24 to any port 22
   ```

### General Network Security

| Practice | Why |
|----------|-----|
| Use SSH, not Telnet | Encrypted vs plain text |
| Keep system updated | Security patches |
| Use firewall | Block unwanted access |
| Monitor logs | Detect intrusion |
| Use VPN for public WiFi | Encrypt traffic |

---

## Summary

In this chapter, you learned:

- **Network Concepts**: IP addresses, ports, gateways, DNS
- **Viewing Configuration**: `ip addr`, `ip route`
- **Testing Connectivity**: `ping`, `traceroute`, `nslookup`
- **SSH Remote Access**: `ssh`, key-based authentication
- **File Transfer**: `scp`, `rsync`
- **Troubleshooting**: Diagnostic workflow, common issues
- **Security**: SSH best practices, firewall basics

---

## Chapter Quiz

Test your understanding of networking basics:

{{#quiz ../../quizzes/chapter-11.toml}}

---

## Exercises

### Exercise 1: Network Information

1. Check your IP address
2. Check your default gateway
3. Check your DNS servers
4. List all network interfaces

### Exercise 2: Connectivity Testing

1. Ping your local gateway
2. Ping 8.8.8.8 (Google DNS)
3. Ping google.com
4. Trace route to google.com

### Exercise 3: SSH Connection

1. Generate SSH key pair (if you don't have one)
2. Copy your public key to a remote machine (or VM)
3. Connect using SSH
4. Run a remote command via SSH

### Exercise 4: File Transfer

1. Create a test file
2. Copy it to remote machine using scp
3. Sync a directory using rsync
4. Verify the transfer

### Exercise 5: Troubleshooting

1. Create a network diagnostic script
2. Check interface status
3. Test gateway connectivity
4. Test DNS resolution
5. Test internet connectivity

---

## Expected Output

### Exercise 1 Solution

```bash
$ ip addr show eth0
2: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP>
    inet 192.168.1.100/24 brd 192.168.1.255 scope global dynamic eth0

$ ip route | grep default
default via 192.168.1.1 dev eth0

$ cat /etc/resolv.conf
nameserver 127.0.0.53

$ ip addr
1: lo: <LOOPBACK,UP,LOWER_UP>
2: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP>
3: wlan0: <BROADCAST,MULTICAST>
```

### Exercise 2 Solution

```bash
$ ping -c 2 192.168.1.1
PING 192.168.1.1 (192.168.1.1) 56(84) bytes of data.
64 bytes from 192.168.1.1: icmp_seq=1 ttl=64 time=0.123 ms

$ ping -c 2 8.8.8.8
PING 8.8.8.8 (8.8.8.8) 56(84) bytes of data.
64 bytes from 8.8.8.8: icmp_seq=1 ttl=115 time=12.3 ms

$ ping -c 2 google.com
PING google.com (142.250.185.46) 56(84) bytes of data.
64 bytes from lga25s72-in-f14.1e100.net: icmp_seq=1 ttl=115 time=12.5 ms

$ traceroute -n google.com
traceroute to google.com (142.250.185.46), 30 hops max
 1  192.168.1.1  0.123 ms
 2  10.0.0.1  5.432 ms
```

### Exercise 3 Solution

```bash
$ ssh-keygen -t ed25519
Generating public/private ed25519 key pair.
Enter file in which to save the key (/home/user/.ssh/id_ed25519):
Enter passphrase (empty for no passphrase):
Created directory '/home/user/.ssh'.

$ ssh-copy-id alice@192.168.1.100
alice@192.168.1.100's password:
Number of key(s) added: 1

$ ssh alice@192.168.1.100
Welcome to Ubuntu 22.04 LTS
alice@host:~$

$ ssh alice@192.168.1.100 "uname -a"
Linux host 5.15.0-76-generic #83-Ubuntu SMP x86_64 GNU/Linux
```

### Exercise 4 Solution

```bash
$ echo "Test content" > testfile.txt

$ scp testfile.txt alice@192.168.1.100:/home/alice/
testfile.txt                     100%   13     0.1KB/s   00:00

$ mkdir files && cp testfile.txt files/
$ rsync -avz files/ alice@192.168.1.100:/home/alice/backups/
sending incremental file list
./
testfile.txt

$ ssh alice@192.168.1.100 "cat /home/alice/testfile.txt"
Test content
```

### Exercise 5 Solution

```bash
#!/bin/bash
# diag.sh

echo "=== Network Diagnostic ==="

# Interfaces
echo -e "\n[1] Interface Status:"
ip addr show | grep -E "^[0-9]|inet " | head -20

# Gateway
echo -e "\n[2] Gateway Ping:"
GATEWAY=$(ip route | grep default | awk '{print $3}')
if ping -c 1 -W 2 $GATEWAY > /dev/null 2>&1; then
    echo "Gateway ($GATEWAY): OK"
else
    echo "Gateway ($GATEWAY): FAILED"
fi

# Internet
echo -e "\n[3] Internet Check:"
if ping -c 1 -W 2 8.8.8.8 > /dev/null 2>&1; then
    echo "Internet (8.8.8.8): OK"
else
    echo "Internet (8.8.8.8): FAILED"
fi

# DNS
echo -e "\n[4] DNS Resolution:"
if nslookup google.com > /dev/null 2>&1; then
    echo "DNS: OK"
else
    echo "DNS: FAILED"
fi

# Open ports
echo -e "\n[5] Listening Ports:"
ss -tulpn | grep LISTEN | head -5
```

```bash
$ chmod +x diag.sh
$ ./diag.sh
=== Network Diagnostic ===

[1] Interface Status:
2: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP>
    inet 192.168.1.100/24

[2] Gateway Ping:
Gateway (192.168.1.1): OK

[3] Internet Check:
Internet (8.8.8.8): OK

[4] DNS Resolution:
DNS: OK

[5] Listening Ports:
tcp   LISTEN 0  128  0.0.0.0:22
tcp   LISTEN 0  5    127.0.0.1:631
```

---

## Next Chapter

In Chapter 12, you'll learn **Git Version Control** - tracking changes to your code, collaborating with others, and managing project history.
