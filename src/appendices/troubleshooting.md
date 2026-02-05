# Troubleshooting Guide

Solutions to common problems you may encounter while learning Linux.

## Permission Errors

### "Permission denied"

**Symptom:**
```bash
$ touch /etc/test.txt
touch: cannot touch '/etc/test.txt': Permission denied
```

**Solutions:**
1. Use `sudo` for system operations:
   ```bash
   sudo touch /etc/test.txt
   ```

2. Check if you need to own the file:
   ```bash
   sudo chown $USER /etc/test.txt
   ```

3. Check file permissions:
   ```bash
   ls -l /etc/test.txt
   ```

---

## Command Not Found

### "command not found"

**Symptom:**
```bash
$ git
git: command not found
```

**Solutions:**
1. Check spelling:
   ```bash
   # Common typos
   cd /hme/user      # Wrong
   cd /home/user     # Correct
   ```

2. Install missing package:

   **Fedora:**
   ```bash
   sudo dnf install git
   ```

   **Debian:**
   ```bash
   sudo apt install git
   ```

3. Check PATH:
   ```bash
   echo $PATH
   which git
   ```

---

## File System Issues

### "No such file or directory"

**Symptom:**
```bash
$ cd documents
bash: cd: documents: No such file or directory
```

**Solutions:**
1. List current directory contents:
   ```bash
   ls -la
   ```

2. Check your location:
   ```bash
   pwd
   ```

3. Use tab completion to avoid typos:
   ```bash
   cd doc<Tab>    # Completes to documents/
   ```

4. Search for the file:
   ```bash
   find ~ -name "documents" -type d
   ```

---

### "Directory not empty"

**Symptom:**
```bash
$ rm directory/
rm: cannot remove 'directory/': Is a directory
```

**Solutions:**
1. Use recursive remove:
   ```bash
   rm -r directory/
   ```

2. Remove with confirmation:
   ```bash
   rm -ri directory/
   ```

---

## Package Management Issues

### "Could not get lock /var/lib/dpkg/lock"

**Symptom (Debian):**
```
E: Could not get lock /var/lib/dpkg/lock-frontend
```

**Solutions:**
1. Wait for another package manager to finish
2. Check for running processes:
   ```bash
   ps aux | grep apt
   ```

3. Remove lock (if stuck):
   ```bash
   sudo rm /var/lib/dpkg/lock-frontend
   sudo rm /var/lib/dpkg/lock
   ```

---

### "Cannot update repository metadata"

**Symptom (Fedora):**
```
Error: Cannot update repository metadata
```

**Solutions:**
1. Clean metadata cache:
   ```bash
   sudo dnf clean all
   sudo dnf makecache
   ```

2. Check network connection:
   ```bash
   ping -c 3 fedoraproject.org
   ```

3. Refresh repositories:
   ```bash
   sudo dnf refresh
   ```

---

## Process Issues

### Process won't stop

**Symptom:** Application freezes and won't close.

**Solutions:**
1. Find the process ID:
   ```bash
   ps aux | grep application_name
   ```

2. Try graceful termination:
   ```bash
   kill <PID>
   ```

3. Force kill if needed:
   ```bash
   kill -9 <PID>
   ```

4. Kill by name:
   ```bash
   pkill application_name
   killall application_name
   ```

---

### Service won't start

**Symptom:**
```bash
$ sudo systemctl start nginx
Job for nginx.service failed.
```

**Solutions:**
1. Check service status:
   ```bash
   systemctl status nginx
   ```

2. View service logs:
   ```bash
   journalctl -u nginx -n 50
   ```

3. Check configuration:
   ```bash
   sudo nginx -t    # Test nginx config
   ```

4. Check for port conflicts:
   ```bash
   sudo ss -tulpn | grep :80
   ```

---

## Network Issues

### Cannot connect to WiFi

**Solutions:**
1. Check network interface:
   ```bash
   ip link show
   ```

2. Bring interface up:
   ```bash
   sudo ip link set wlan0 up
   ```

3. Use NetworkManager (GUI or CLI):
   ```bash
   nmcli dev wifi list
   nmcli dev wifi connect "SSID" password "password"
   ```

4. Restart network service:
   ```bash
   sudo systemctl restart NetworkManager
   ```

---

### SSH connection refused

**Symptom:**
```bash
$ ssh user@host
ssh: connect to host port 22: Connection refused
```

**Solutions:**
1. Check if SSH server is running:
   ```bash
   systemctl status sshd       # Fedora
   systemctl status ssh        # Debian
   ```

2. Start SSH server:
   ```bash
   sudo systemctl start sshd
   sudo systemctl enable sshd
   ```

3. Check firewall:
   ```bash
   sudo firewall-cmd --list-all    # Fedora
   sudo ufw status                 # Debian
   ```

4. Check if port 22 is open:
   ```bash
   sudo ss -tulpn | grep :22
   ```

---

## Disk Space Issues

### "No space left on device"

**Symptom:**
```bash
$ touch file.txt
touch: cannot touch 'file.txt': No space left on device
```

**Solutions:**
1. Check disk usage:
   ```bash
   df -h
   ```

2. Find large files:
   ```bash
   du -h ~ | sort -hr | head -20
   ```

3. Clean package cache:

   **Fedora:**
   ```bash
   sudo dnf clean all
   ```

   **Debian:**
   ```bash
   sudo apt clean
   sudo apt autoremove
   ```

4. Empty trash:
   ```bash
   rm -rf ~/.local/share/Trash/*
   ```

5. Clean old logs:
   ```bash
   sudo journalctl --vacuum-time=7d
   ```

---

## Git Issues

### "fatal: not a git repository"

**Symptom:**
```bash
$ git status
fatal: not a git repository (or any of the parent directories)
```

**Solutions:**
1. Initialize repository:
   ```bash
   git init
   ```

2. Clone existing repository:
   ```bash
   git clone <url>
   ```

---

### Merge conflicts

**Symptom:**
```bash
$ git merge feature
Auto-merging file.txt
CONFLICT (content): Merge conflict in file.txt
```

**Solutions:**
1. Edit the file and resolve conflicts manually
2. Mark as resolved:
   ```bash
   git add file.txt
   git commit
   ```

3. Abort merge if needed:
   ```bash
   git merge --abort
   ```

---

## Docker Issues

### "Permission denied while trying to connect to the Docker daemon"

**Symptom:**
```bash
$ docker ps
permission denied while trying to connect to the Docker daemon socket
```

**Solutions:**
1. Use sudo:
   ```bash
   sudo docker ps
   ```

2. Add user to docker group:
   ```bash
   sudo usermod -aG docker $USER
   ```

3. Log out and back in for changes to take effect.

---

### Container exits immediately

**Symptom:**
```bash
$ docker run nginx
$ docker ps -a
CONTAINER ID   STATUS
abc123         Exited (0) 5 seconds ago
```

**Solutions:**
1. Container may have no foreground process
2. Use `-d` for detached mode:
   ```bash
   docker run -d nginx
   ```

3. Check logs:
   ```bash
   docker logs <container_id>
   ```

---

## Getting Help

### Built-in Help

**Man pages (manual):**
```bash
man <command>      # e.g., man ls
man -k <keyword>   # Search man pages
```

**Help flag:**
```bash
<command> --help   # e.g., ls --help
```

**Info pages:**
```bash
info <command>
```

### Online Resources

- **Fedora Documentation:** https://docs.fedoraproject.org/
- **Debian Documentation:** https://www.debian.org/doc/
- **Arch Wiki (excellent general reference):** https://wiki.archlinux.org/
- **Linux Command Library:** https://linux.die.net/
- **Stack Overflow:** https://stackoverflow.com/questions/tagged/linux

### Community Forums

- **Fedora Discussion:** https://discussion.fedoraproject.org/
- **Debian Forums:** https://forums.debian.net/
- **Reddit r/linux4noobs:** https://reddit.com/r/linux4noobs

## Debugging Commands

| Command | Purpose |
|---------|---------|
| `journalctl -xb` | View boot messages |
| `dmesg` | Kernel ring buffer |
| `systemctl --failed` | List failed services |
| `strace <command>` | Trace system calls |
| `ltrace <command>` | Trace library calls |
| `tail -f /var/log/syslog` | Follow system log |
