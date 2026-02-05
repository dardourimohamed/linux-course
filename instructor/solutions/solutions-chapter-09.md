# Chapter 9 Solutions: Processes & Services

## Exercise Solutions

### Exercise 1: Process Exploration

**Task:** Explore running processes with ps.

**Solution:**

```bash
# Count total processes
ps aux | wc -l
```

**Expected Output:**
```
245
```

```bash
# Find your shell process
ps aux | grep bash | grep -v grep
```

**Expected Output:**
```
user  2048  0.0  0.1  12548  9524 pts/0  Ss   10:15   0:00 -bash
```

```bash
# Find the systemd process (PID 1)
ps aux | grep systemd | grep -v grep
```

**Expected Output:**
```
root       1  0.0  0.1 168336 11200 ?      Ss   Feb07   0:02 /sbin/init
```

```bash
# View process tree
pstree | head -20
```

**Expected Output:**
```
systemd─┬─NetworkManager───2*[{NetworkManager}]
        ├─ModemManager───2*[{ModemManager}]
        ├─accounts-daemon───2*[{accounts-daemon}]
        ├─bash───pstree
        ├─colord───2*[{colord}]
        └─systemd-journald
```

**Explanation:**
- `ps aux`: Show all processes for all users
- `grep -v grep`: Exclude the grep process itself from results
- PID 1 is always systemd (init process)
- pstree shows parent-child relationships

---

### Exercise 2: Monitor with top/htop

**Task:** Use htop to monitor system resources.

**Solution:**

```bash
# Install htop if not present
which htop || sudo dnf install htop
```

```bash
# Start htop
htop
```

**In htop:**
- Press `P` to sort by CPU (default)
- Press `M` to sort by memory
- Press `T` to sort by time
- Use arrow keys to navigate
- Press `F9` to kill a process
- Press `F10` to quit

**Expected htop display (text representation):**
```
CPU  ████████████████████░░░░░░░ 45%
Mem  ████████░░░░░░░░░░░░░░░░░░░ 2.5G/15G
Swp  ░░░░░░░░░░░░░░░░░░░░░░░░░░░ 0/4G

  PID USER      PRI  NI  VIRT   RES   SHR S CPU% MEM%   TIME+  Command
  789 user       20   0 3.2G   423M   45M S  5.0  2.7  0:45.23 firefox
 1024 user       20   0  1.5G  120M   20M S  2.0  0.8  0:12.34 code
    1 root       20   0  164M   11M   3M S  0.0  0.1  0:02.34 systemd
```

**Monitor for 30 seconds and note:**
- Top CPU-consuming processes
- Top memory-consuming processes
- Overall CPU usage percentage
- Overall memory usage

**Explanation:**
- htop shows real-time system statistics
- Color-coded bars for CPU, memory, swap
- Sortable by different criteria
- Interactive process management

---

### Exercise 3: Service Management

**Task:** Check and manage SSH service.

**Solution:**

```bash
# Check if SSH service is running
sudo systemctl status ssh
```

**Expected Output:**
```
● ssh.service - OpenSSH server daemon
     Loaded: loaded (/usr/lib/systemd/system/ssh.service; enabled; preset: enabled)
     Active: active (running) since Wed 2025-02-07 10:15:23 CET; 2h ago
       Docs: man:sshd(8)
             man:sshd_config(5)
   Main PID: 789 (sshd)
      Tasks: 1 (limit: 38212)
     Memory: 4.2M (peak: 8.9M)
        CPU: 45ms
     CGroup: /system.slice/ssh.service
             └─789 "sshd: /usr/sbin/sshd -D [listener] 0 of 10-100 startups"

Feb 07 10:15:23 hostname systemd[1]: Starting OpenSSH server daemon...
Feb 07 10:15:23 hostname sshd[789]: Server listening on 0.0.0.0 port 22.
```

```bash
# Check if enabled at boot
systemctl is-enabled ssh
```

**Expected Output:**
```
enabled
```

```bash
# View recent SSH logs with journalctl
sudo journalctl -u ssh -n 10
```

**Expected Output:**
```
-- Logs begin at Wed 2025-02-01 00:00:00 CET, end at Wed 2025-02-07 12:34:56 CET. --
Feb 07 10:15:20 hostname systemd[1]: Starting OpenSSH server daemon...
Feb 07 10:15:20 hostname sshd[789]: Server listening on 0.0.0.0 port 22.
Feb 07 10:20:45 hostname sshd[1234]: Accepted password for user from 192.168.1.100 port 22 ssh2
Feb 07 12:30:15 hostname sshd[2345]: Accepted password for user from 192.168.1.100 port 22 ssh2
```

```bash
# If SSH is not running, start it:
# sudo systemctl start ssh

# If SSH is not enabled, enable it:
# sudo systemctl enable ssh
```

**Explanation:**
- `systemctl status`: Show detailed service status
- `systemctl is-enabled`: Check if service starts at boot
- `journalctl -u`: View logs for specific service

---

### Exercise 4: Process Control

**Task:** Start, control, and kill a background process.

**Solution:**

```bash
# Start a long-running process in background
sleep 300 &
```

**Expected Output:**
```
[1] 5678
```

```bash
# Find its PID
ps -p 5678
```

**Expected Output:**
```
PID TTY          TIME CMD
5678 pts/0    00:00:00 sleep
```

```bash
# View background jobs
jobs
```

**Expected Output:**
```
[1]+  Running                 sleep 300 &
```

```bash
# Bring to foreground
fg %1
```

**Expected Output:**
```
sleep 300
```

```bash
# Suspend with Ctrl+Z
# (Press Ctrl+Z)
```

**Expected Output:**
```
^Z
[1]+  Stopped                 sleep 300
```

```bash
# Resume in background
bg
```

**Expected Output:**
```
[1]+ sleep 300 &
```

```bash
# Kill the process
kill 5678
```

**Expected Output:**
```
[1]+  Terminated              sleep 300
```

**Explanation:**
- `&`: Run command in background
- `jobs`: List background jobs
- `fg %N`: Bring job N to foreground
- `bg`: Resume suspended job in background
- `Ctrl+Z`: Suspend current foreground job
- `kill PID`: Terminate process by PID

---

### Exercise 5: Logs and Troubleshooting

**Task:** View and analyze system logs.

**Solution:**

```bash
# View last 20 system log entries
sudo journalctl -n 20
```

**Expected Output:**
```
-- Logs begin at Wed 2025-02-01 00:00:00 CET, end at Wed 2025-02-07 12:34:56 CET. --
Feb 07 12:30:01 hostname CRON[4567]: (root) CMD (cd / && run-parts --report /etc/cron.hourly)
Feb 07 12:34:12 hostname systemd[1]: Started User Manager for UID 1000
Feb 07 12:34:15 hostname gnome-shell[890]: Activating HUD
Feb 07 12:34:30 hostname kernel: usb 1-2: USB disconnect, device number 5
Feb 07 12:34:45 hostname systemd[1]: Starting Daily apt download activities...
```

```bash
# Find error or warning messages
sudo journalctl -p err -n 10
```

**Expected Output:**
```
-- Logs begin at Wed 2025-02-01 00:00:00 CET. --
Feb 07 10:15:23 hostname kernel: Out of memory: Killed process 1234
Feb 07 11:20:45 hostname sshd[2345]: error: Could not load host key
Feb 07 12:00:15 hostname systemd[1]: Failed to start Some Service
```

```bash
# Check logs from last boot
sudo journalctl -b -n 10
```

```bash
# View NetworkManager logs
sudo journalctl -u NetworkManager -n 10 --no-pager
```

**Expected Output:**
```
-- Logs begin at Wed 2025-02-07 10:00:00 CET. --
Feb 07 10:15:20 hostname NetworkManager[789]: <info>  [1707284920.234] manager: startup complete
Feb 07 10:15:25 hostname NetworkManager[789]: <info>  [1707284925.456] device (wlp3s0): state change: activated -> disconnected
Feb 07 10:15:30 hostname NetworkManager[789]: <warn>  [1707284930.678] device (wlp3s0): link down
```

**Explanation:**
- `journalctl -n`: Show last N entries
- `journalctl -p err`: Show only error priority and above
- `journalctl -b`: Show logs from current boot only
- `journalctl -u service`: Show logs for specific service

---

## Process Monitoring Examples

### Find Top CPU Consumers

```bash
ps aux | sort -rk 3 | head -10
```

**Expected Output:**
```
USER       PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
user      789 15.3  5.2 3245680 423456 ?      Sl   Feb07 245:23 firefox
root       45  8.2  0.3  67890  23456 ?        S    Feb07  12:34  /usr/bin/X
user     1024  5.1  2.8 1245678 223456 ?      Sl   Feb07  67:89 code
```

### Find Top Memory Consumers

```bash
ps aux | sort -rk 4 | head -10
```

### Find Zombie Processes

```bash
ps aux | grep Z
```

### Monitor Specific Process

```bash
# Watch Firefox CPU usage in real-time
watch -n 1 'ps aux | grep firefox | grep -v grep'
```

---

## Service Management Examples

### Start a Web Server

```bash
# Install nginx
sudo dnf install nginx

# Start the service
sudo systemctl start nginx

# Check status
sudo systemctl status nginx

# Enable at boot
sudo systemctl enable nginx

# Verify it's running
sudo systemctl is-active nginx
# Output: active
```

### Restart a Service

```bash
# Restart nginx after config change
sudo systemctl restart nginx

# Or reload (no downtime) if service supports it
sudo systemctl reload nginx
```

### Stop and Disable a Service

```bash
# Stop the service
sudo systemctl stop nginx

# Disable from starting at boot
sudo systemctl disable nginx

# Verify
systemctl is-enabled nginx
# Output: disabled
```

---

## Instructor Notes

### Teaching Tips

1. **Live monitoring demo**: Run htop during class to show real-time process activity
2. **Service demonstration**: Start/stop/restart actual services (nginx, ssh)
3. **Kill signals demo**: Show difference between kill -15 (SIGTERM) and kill -9 (SIGKILL)
4. **Log exploration**: Use journalctl to show how logs reveal system events
5. **Process tree visualization**: Use pstree to show parent-child relationships

### Common Student Issues

| Issue | Solution |
|-------|----------|
| Can't find process PID | Use `ps aux | grep name` or `pgrep name` |
| Service won't start | Check logs: `sudo journalctl -u service -xe` |
| Can't kill process | Try `kill -9 PID` (force) or check if parent is stuck |
| htop not installed | `sudo dnf install htop` or `sudo apt install htop` |
| journalctl too much output | Use `-n` for last N entries or `--since "1 hour ago"` |

### Extension Activities

- Create a process monitoring dashboard script
- Set up custom service units in systemd
- Explore cgroups for resource limiting
- Research process priority with nice and renice
- Create a log analyzer script with journalctl
- Compare systemd with SysVinit scripts
- Monitor system resources and create alerts
