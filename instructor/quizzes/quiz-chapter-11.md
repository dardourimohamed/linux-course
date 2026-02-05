# Chapter 11 Quiz: Networking Basics

## Quiz Questions

### Multiple Choice

**1. What is the default port for SSH?**
- A) 21
- B) 22
- C) 23
- D) 80

**Answer:** B

**2. Which command tests network connectivity to another host?**
- A) ping
- B) traceroute
- C) nslookup
- D) ssh

**Answer:** A

**3. What does SSH provide that Telnet does not?**
- A) Faster connection
- B) Encryption and security
- C) Color support
- D) File transfer

**Answer:** B

**4. Which command copies files securely over the network?**
- A) scp
- B) copy
- C) rsync
- D) Both A and C

**Answer:** D

**5. What is the purpose of an SSH key pair?**
- A) To encrypt your home directory
- B) To authenticate without passwords
- C) To speed up network connections
- D) To compress data during transfer

**Answer:** B

### True/False

**6. True or False: The ip command is the modern replacement for ifconfig.**

**Answer:** True

**7. True or False: DNS translates domain names to IP addresses.**

**Answer:** True

**8. True or False: You can use ssh to execute commands on a remote system without logging in interactively.**

**Answer:** True

**9. True or False: rsync is always faster than scp for file transfers.**

**Answer:** False (it can be more efficient due to delta transfer, but not always faster)

**10. True or False: The gateway is the route to other networks.**

**Answer:** True

### Short Answer

**11. What are the five essential networking commands every Linux user should know?**

**Answer:**
1. **ping** - Test connectivity to another host
2. **ip addr** - View network interfaces and IP addresses
3. **ssh** - Secure remote login and command execution
4. **scp/rsync** - Secure file transfer
5. **nslookup/dig** - DNS queries and troubleshooting
(Alternates: traceroute, netstat/ss, curl, wget)

**12. Explain the difference between password authentication and key-based authentication in SSH.**

**Answer:**
**Password Authentication:**
- Server prompts for password each connection
- Password transmitted (encrypted)
- Vulnerable to brute-force attacks
- Requires typing password each time
- Less convenient for automation

**Key-Based Authentication:**
- Uses cryptographic key pair (private + public)
- Private key never leaves your machine
- Public key stored on server in ~/.ssh/authorized_keys
- No password needed (more secure + convenient)
- Resistant to brute-force attacks
- Better for automation and scripts
- Requires one-time setup with ssh-copy-id

**13. What is a router/gateway and why is it important for network connectivity?**

**Answer:**
A **router/gateway** is a network device that connects your local network to other networks (including the internet). It's important because:

1. **Route to internet**: All traffic to external networks passes through the gateway
2. **DHCP server**: Assigns IP addresses to devices on your network
3. **NAT**: Allows multiple devices to share one public IP address
4. **Firewall**: Filters traffic for security
5. **Local network communication**: Routes traffic between devices on your LAN

**In networking**: Your computer sends traffic to the gateway (usually 192.168.1.1 or similar), which forwards it to the internet and routes responses back to you.

### Discussion Question

**14. SSH is often called the "swiss army knife" of system administration. Explain why this description is accurate by discussing at least five different things you can do with SSH beyond just logging into a remote server. How has SSH revolutionized remote system management?**

**Sample Answer:**

**SSH Capabilities Beyond Remote Login:**

1. **Execute Remote Commands**: Run commands without full interactive session
   ```bash
   ssh user@host "sudo systemctl restart nginx"
   ```
   Perfect for automation and scripting.

2. **File Transfer with SCP**: Copy files securely
   ```bash
   scp localfile.txt user@host:/remote/path/
   ```

3. **File Sync with rsync**: Efficiently sync directories
   ```bash
   rsync -avz localdir/ user@host:remotedir/
   ```

4. **Port Forwarding/Tunneling**: Access remote services locally
   ```bash
   ssh -L 8080:localhost:80 user@host
   ```
   Access remote web server on localhost:8080

5. **SSH Keys for Authentication**: Passwordless, secure login
   ```bash
   ssh-keygen -t ed25519
   ssh-copy-id user@host
   ```

6. **SSH Config for Simplification**: Define hosts and options
   ```bash
   Host myserver
       HostName 192.168.1.100
       User admin
   ```

7. **Mount Remote Filesystems**: SSHFS makes remote directories local
   ```bash
   sshfs user@host:/remote/path ~/local/mount
   ```

8. **Git over SSH**: Push/pull code repositories securely

9. **Tunneling Through Firewalls**: Bypass restrictive network policies

10. **Remote Port Forwarding**: Make local services accessible remotely
    ```bash
    ssh -R 8080:localhost:80 user@host
    ```

**How SSH Revolutionized System Administration:**

**Before SSH:**
- Telnet: Everything sent in plain text (passwords visible!)
- Physical access required for servers
- Rlogin/rsh: Insecure, limited functionality
- Each tool had its own protocol and security model

**After SSH:**
- **Encryption**: All traffic encrypted, including passwords
- **Unified**: Single tool for remote access, file transfer, port forwarding
- **Scriptable**: Perfect for automation (cron jobs, deployment scripts)
- **Cross-platform**: Works everywhere (Linux, Mac, Windows)
- **Key-based auth**: Secure, convenient authentication
- **Port forwarding**: Replace VPNs for many use cases
- **Ubiquitous**: Standard for all server management

**Impact**: SSH made secure remote administration accessible and practical. Without SSH, modern cloud computing and DevOps would not exist as we know them. It's the foundation of remote Linux system administration.

## Answer Key

| Question | Answer | Points |
|----------|--------|--------|
| 1 | B | 1 |
| 2 | A | 1 |
| 3 | B | 1 |
| 4 | D | 1 |
| 5 | B | 1 |
| 6 | True | 1 |
| 7 | True | 1 |
| 8 | True | 1 |
| 9 | False | 1 |
| 10 | True | 1 |
| 11 | Five commands listed | 4 |
| 12 | Both methods compared | 3 |
| 13 | Gateway explained | 3 |
| 14 | Five SSH uses + impact | 5 |

**Total Points:** 27

**Passing Score:** 18/27 (67%)

---

## Instructor Notes

### Common Misconceptions to Address

1. **"SSH is only for logging in"** - Emphasize the many other capabilities (file transfer, port forwarding, etc.)

2. **"Telnet is good enough for local networks"** - Still sends everything in plain text. Always use SSH.

3. **"Password auth is fine if the password is strong"** - Key-based auth is both more secure AND more convenient.

4. **"IP addresses are permanent"** - Explain dynamic IP assignment via DHCP.

### Teaching Tips

- **Live SSH demo**: Connect to a VM or remote server during class
- **Key generation demo**: Show ssh-keygen and ssh-copy-id in real-time
- **Port forwarding demo**: Demonstrate local port forwarding with a web server
- **Network troubleshooting**: Walk through diagnosing connectivity problems step by step
- **Security discussion**: Why plain text protocols (Telnet, HTTP) are dangerous

### Extension Activities

- Set up passwordless SSH between lab machines
- Create an SSH config file for multiple servers
- Practice rsync for backup automation
- Set up a simple port forwarding tunnel
- Diagnose and fix network problems in a simulated environment
- Research SSH jump hosts and proxy jumping
