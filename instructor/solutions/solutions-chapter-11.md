# Chapter 11 Solutions: Networking Basics

## Exercise Solutions

### Exercise 1: Network Information

**Task:** Check IP address, gateway, DNS, and interfaces.

**Solution:**

```bash
# Check your IP address
ip addr show eth0
```

**Expected Output:**
```
2: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP>
    inet 192.168.1.100/24 brd 192.168.1.255 scope global dynamic eth0
```

```bash
# Or show all interfaces
ip addr
```

**Expected Output:**
```
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN
    inet 127.0.0.1/8 scope host lo
2: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP
    inet 192.168.1.100/24 brd 192.168.1.255 scope global dynamic eth0
```

```bash
# Check your default gateway
ip route | grep default
```

**Expected Output:**
```
default via 192.168.1.1 dev eth0
```

```bash
# Check your DNS servers
cat /etc/resolv.conf
```

**Expected Output:**
```
nameserver 127.0.0.53
options edns0 trust-ad
search .
```

```bash
# List all network interfaces
ip link show
```

**Expected Output:**
```
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN mode DEFAULT
2: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP mode DEFAULT
3: wlan0: <BROADCAST,MULTICAST> mtu 1500 qdisc noop state DOWN mode DEFAULT
```

**Explanation:**
- `ip addr`: Show IP addresses for all interfaces
- `ip route`: Show routing table (gateway is default route)
- `/etc/resolv.conf`: DNS configuration
- `ip link`: Show network interfaces (up/down status)

---

### Exercise 2: Connectivity Testing

**Task:** Test connectivity to gateway, DNS, and internet.

**Solution:**

```bash
# Ping your local gateway
ping -c 2 192.168.1.1
```

**Expected Output:**
```
PING 192.168.1.1 (192.168.1.1) 56(84) bytes of data.
64 bytes from 192.168.1.1: icmp_seq=1 ttl=64 time=0.123 ms
64 bytes from 192.168.1.1: icmp_seq=2 ttl=64 time=0.115 ms

--- 192.168.1.1 ping statistics ---
2 packets transmitted, 2 received, 0% packet loss
round-trip min/avg/max/stddev = 0.115/0.119/0.123/0.004 ms
```

```bash
# Ping 8.8.8.8 (Google DNS)
ping -c 2 8.8.8.8
```

**Expected Output:**
```
PING 8.8.8.8 (8.8.8.8) 56(84) bytes of data.
64 bytes from 8.8.8.8: icmp_seq=1 ttl=115 time=12.3 ms
64 bytes from 8.8.8.8: icmp_seq=2 ttl=115 time=11.9 ms

--- 8.8.8.8 ping statistics ---
2 packets transmitted, 2 received, 0% packet loss
round-trip min/avg/max/stddev = 11.9/12.1/12.3/0.200 ms
```

```bash
# Ping google.com (tests DNS)
ping -c 2 google.com
```

**Expected Output:**
```
PING google.com (142.250.185.46) 56(84) bytes of data.
64 bytes from lga25s72-in-f14.1e100.net: icmp_seq=1 ttl=115 time=12.5 ms
64 bytes from lga25s72-in-f14.1e100.net: icmp_seq=2 ttl=115 time=13.1 ms

--- google.com ping statistics ---
2 packets transmitted, 2 received, 0% packet loss
round-trip min/avg/max/stddev = 12.5/12.8/13.1/0.300 ms
```

```bash
# Trace route to google.com
traceroute -n google.com
```

**Expected Output:**
```
traceroute to google.com (142.250.185.46), 30 hops max
 1  192.168.1.1  0.123 ms  0.115 ms  0.120 ms
 2  10.0.0.1  5.432 ms  5.389 ms  5.410 ms
 3  72.14.215.85  12.123 ms  12.089 ms  12.156 ms
 ...
```

**Explanation:**
- `ping -c N`: Send N packets (then stop)
- Gateway ping tests local network connectivity
- 8.8.8.8 ping tests internet connectivity (bypassing DNS)
- google.com ping tests DNS resolution + internet
- `traceroute`: Shows path packets take to destination

---

### Exercise 3: SSH Connection

**Task:** Generate SSH keys and connect to remote machine.

**Solution:**

```bash
# Generate SSH key pair (if you don't have one)
ls ~/.ssh/id_rsa* ~/.ssh/id_ed25519* 2>/dev/null
```

**If keys don't exist:**

```bash
ssh-keygen -t ed25519
```

**Expected Output:**
```
Generating public/private ed25519 key pair.
Enter file in which to save the key (/home/user/.ssh/id_ed25519):
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in /home/user/.ssh/id_ed25519
Your public key has been saved in /home/user/.ssh/id_ed25519.pub
The key fingerprint is:
SHA256:abc123... user@hostname
The key's randomart image is:
+--[ED25519 256]--+
|       .o.       |
|      ...        |
...
```

```bash
# Copy public key to remote server
ssh-copy-id alice@192.168.1.100
```

**Expected Output:**
```
/usr/bin/ssh-copy-id: INFO: Source of key(s) to be installed: "/home/user/.ssh/id_ed25519.pub"
alice@192.168.1.100's password:
Number of key(s) added: 1

Now try logging into the machine, with:   "ssh 'alice@192.168.1.100'"
```

```bash
# Connect using SSH (passwordless now)
ssh alice@192.168.1.100
```

**Expected Output:**
```
Welcome to Ubuntu 22.04 LTS
Last login: Wed Feb  7 10:00:00 2025 from 192.168.1.100
alice@host:~$
```

```bash
# Run a remote command via SSH
ssh alice@192.168.1.100 "uname -a"
```

**Expected Output:**
```
Linux host 5.15.0-76-generic #83-Ubuntu SMP x86_64 GNU/Linux
```

**Explanation:**
- `ssh-keygen`: Generate cryptographic key pair
- `ssh-copy-id`: Copy public key to remote server's authorized_keys
- `ssh user@host`: Connect to remote system
- SSH keys eliminate password typing and are more secure

---

### Exercise 4: File Transfer

**Task:** Transfer files using scp and rsync.

**Solution:**

```bash
# Create a test file
echo "Test content" > testfile.txt
```

```bash
# Copy file to remote machine using scp
scp testfile.txt alice@192.168.1.100:/home/alice/
```

**Expected Output:**
```
testfile.txt                     100%   13     0.1KB/s   00:00
```

```bash
# Copy file back from remote
scp alice@192.168.1.100:/home/alice/testfile.txt ./testfile_copy.txt
```

```bash
# Create a directory with multiple files
mkdir files && cp testfile.txt files/
```

```bash
# Sync directory using rsync
rsync -avz files/ alice@192.168.1.100:/home/alice/backups/
```

**Expected Output:**
```
sending incremental file list
./
testfile.txt

sent 234 bytes  received 38 bytes  108.00 bytes/sec
total size is 13  speedup is 0.05
```

```bash
# Verify the transfer
ssh alice@192.168.1.100 "cat /home/alice/backups/testfile.txt"
```

**Expected Output:**
```
Test content
```

**Explanation:**
- `scp local-file user@host:remote-path`: Copy file to remote
- `scp user@host:remote-file local-path`: Copy file from remote
- `rsync -avz`: Archive mode, verbose, compressed
  - More efficient than scp for large transfers
  - Can sync entire directory trees

---

### Exercise 5: Troubleshooting

**Task:** Create a network diagnostic script.

**Solution:**

```bash
#!/bin/bash
# diag.sh

echo "=== Network Diagnostic ==="

# Interfaces
echo -e "\n[1] Interface Status:"
ip addr show | grep -E "^[0-9]|inet " | head -20
```

**Expected Output:**
```
=== Network Diagnostic ===

[1] Interface Status:
1: lo: <LOOPBACK,UP,LOWER_UP>
    inet 127.0.0.1/8
2: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP>
    inet 192.168.1.100/24
```

```bash
# Gateway
echo -e "\n[2] Gateway Ping:"
GATEWAY=$(ip route | grep default | awk '{print $3}')
if ping -c 1 -W 2 $GATEWAY > /dev/null 2>&1; then
    echo "Gateway ($GATEWAY): OK"
else
    echo "Gateway ($GATEWAY): FAILED"
fi
```

**Expected Output:**
```
[2] Gateway Ping:
Gateway (192.168.1.1): OK
```

```bash
# Internet
echo -e "\n[3] Internet Check:"
if ping -c 1 -W 2 8.8.8.8 > /dev/null 2>&1; then
    echo "Internet (8.8.8.8): OK"
else
    echo "Internet (8.8.8.8): FAILED"
fi
```

```bash
# DNS
echo -e "\n[4] DNS Resolution:"
if nslookup google.com > /dev/null 2>&1; then
    echo "DNS: OK"
else
    echo "DNS: FAILED"
fi
```

```bash
# Open ports
echo -e "\n[5] Listening Ports:"
ss -tulpn | grep LISTEN | head -5
```

**Expected Output:**
```
[5] Listening Ports:
tcp   LISTEN 0  128  0.0.0.0:22  0.0.0.0:*   users:(("sshd",pid=789,fd=3))
tcp   LISTEN 0  5    127.0.0.1:631 0.0.0.0:*   users:(("cups",pid=456,fd=5))
```

**Complete script:**

```bash
chmod +x diag.sh
./diag.sh
```

**Expected Output:**
```
=== Network Diagnostic ===

[1] Interface Status:
1: lo: <LOOPBACK,UP,LOWER_UP>
    inet 127.0.0.1/8
2: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP>
    inet 192.168.1.100/24

[2] Gateway Ping:
Gateway (192.168.1.1): OK

[3] Internet Check:
Internet (8.8.8.8): OK

[4] DNS Resolution:
DNS: OK

[5] Listening Ports:
tcp   LISTEN 0  128  0.0.0.0:22  0.0.0.0:*   users:(("sshd",pid=789,fd=3))
tcp   LISTEN 0  5    127.0.0.1:631 0.0.0.0:*   users:(("cups",pid=456,fd=5))
```

**Explanation:**
- Script systematically tests each layer of network
- Each test shows OK or FAILED clearly
- Useful for quick network troubleshooting

---

## SSH Configuration File

### Create SSH Config

```bash
nano ~/.ssh/config
```

**Add configurations:**

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

**Now connect easily:**

```bash
ssh myserver      # instead of ssh alice@192.168.1.100
ssh webserver    # instead of ssh admin -p 2222 example.com
```

---

## Advanced Network Commands

### Check Open Ports

```bash
# Show all listening ports
ss -tulpn

# Show specific port
ss -tulpn | grep :22
```

### Network Statistics

```bash
# View network interface statistics
ip -s link

# View detailed interface info
ethtool eth0
```

### Bandwidth Monitoring

```bash
# Install iftop
sudo dnf install iftop

# Run iftop
sudo iftop -i eth0
```

---

## Instructor Notes

### Teaching Tips

1. **Live SSH demo**: Connect to a VM or remote server during class
2. **Key generation walkthrough**: Show ssh-keygen and ssh-copy-id in real-time
3. **Port forwarding demo**: Demonstrate local port forwarding
4. **Network troubleshooting**: Walk through diagnostic steps
5. **Security discussion**: Why SSH > Telnet, why keys > passwords

### Common Student Issues

| Issue | Solution |
|-------|----------|
| Connection refused | Check if SSH server is running: `sudo systemctl status ssh` |
| Permission denied | Verify public key is in `~/.ssh/authorized_keys` on remote |
| Host key verification failed | Remove old key: `ssh-keygen -R hostname` |
| rsync not found | Install: `sudo dnf install rsync` |
| scp permission denied | Check write permissions on destination |

### Extension Activities

- Set up passwordless SSH between lab machines
- Create an SSH config file for multiple servers
- Practice rsync for backup automation
- Set up local port forwarding for a web server
- Diagnose and fix network problems
- Research SSH jump hosts and proxy jumping
- Create a network monitoring script
