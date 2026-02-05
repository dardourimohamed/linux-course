# Chapter 9: Processes & Services

## Learning Objectives

By the end of this chapter, you will be able to:

- [x] Understand what processes are and how Linux manages them
- [x] View running processes using `ps`, `top`, and `htop`
- [x] Monitor system resources (CPU, memory, disk)
- [x] Control processes (start, stop, kill)
- [x] Manage systemd services
- [x] View and interpret system logs

## Prerequisites

- Completed Chapter 8: Package Management
- Comfortable with basic CLI commands
- Understanding of file paths

---

## What is a Process?

A **process** is a running instance of a program. Every command you run creates a process.

```
┌─────────────────────────────────────────────────────────┐
│                    Your Linux System                    │
├─────────────────────────────────────────────────────────┤
│  Process ID (PID) │ Command          │ Status │ User   │
├───────────────────┼──────────────────┼────────┼────────┤
│  1                │ systemd          │ Running│ root   │
│  423              │ NetworkManager   │ Running│ root   │
│  512              │ gnome-shell      │ Running│ user   │
│  789              │ firefox          │ Running│ user   │
│  1024             │ vim              │ Sleeping│ user  │
│  2048             │ bash             │ Running│ user   │
└─────────────────────────────────────────────────────────┘
```

### Key Process Concepts

| Concept | Description |
|---------|-------------|
| **PID** | Process ID - unique number identifying each process |
| **PPID** | Parent Process ID - the process that started this one |
| **User** | Owner of the process (root or your user) |
| **State** | Running, sleeping, stopped, zombie |
| **Priority** | How important the process is (nice value) |

---

## Viewing Processes

### ps - Process Snapshot

`ps` shows a snapshot of current processes.

```bash
# Simple listing (your processes only)
ps

# Detailed listing of all processes
ps aux

# Tree view (parent-child relationships)
ps auxf

# Process tree with ASCII art
pstree
```

### Understanding `ps aux` Output

```bash
$ ps aux | head -n 10
USER       PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
root         1  0.0  0.1 168336 11200 ?        Ss   Feb07   0:02 /sbin/init
root       423  0.0  0.2 190872 21456 ?        Ss   Feb07   0:00 /usr/sbin/NetworkManager
user       789  2.5  5.2 3245680 423456 ?      Sl   Feb07  45:23 /usr/lib/firefox/firefox
```

| Column | Meaning |
|--------|---------|
| **USER** | Process owner |
| **PID** | Process ID |
| **%CPU** | CPU usage percentage |
| **%MEM** | Memory usage percentage |
| **VSZ** | Virtual memory size (KB) |
| **RSS** | Resident Set Size - actual RAM used |
| **TTY** | Terminal type (?: no terminal) |
| **STAT** | Process state (S=sleeping, R=running) |
| **START** | When process started |
| **TIME** | Total CPU time used |
| **COMMAND** | Command that started the process |

### Process States (STAT column)

| Code | State | Description |
|------|-------|-------------|
| **R** | Running | Currently running or runnable |
| **S** | Sleeping | Waiting for something (I/O, etc.) |
| **D** | Uninterruptible | Waiting for I/O (cannot be interrupted) |
| **Z** | Zombie | Completed but not yet cleaned up by parent |
| **T** | Stopped | Paused (usually by SIGSTOP) |
| **s** | Session leader | |
| **+** | Foreground process | In process group |

---

## Monitoring with `top` and `htop`

### top - Interactive Process Viewer

`top` shows processes in real-time, sorted by resource usage.

```bash
top
```

**Key shortcuts in top:**

| Key | Action |
|-----|--------|
| `q` | Quit |
| `k` | Kill a process (enter PID) |
| `r` | Renice (change priority) |
| `M` | Sort by memory |
| `P` | Sort by CPU (default) |
| `1` | Show per-CPU stats |
| `u` | Filter by user |
| `h` | Help |

### htop - Enhanced Process Viewer

`htop` is more user-friendly than `top` (needs installation).

```bash
# Install
sudo dnf install htop          # Fedora
sudo apt install htop          # Debian

# Run
htop
```

**htop advantages:**
- Color-coded output
- Mouse support
- Visual meter for CPU, memory, swap
- F-keys for common actions
- Scrollable process list

```bash
# htop keyboard shortcuts
F1      Help
F2      Setup
F3      Search
F4      Filter
F5      Tree view
F9      Kill process
F10     Quit
```

---

## Controlling Processes

### Killing Processes

Sometimes a program freezes or hangs. You need to terminate it.

```bash
# Find the process PID
ps aux | grep firefox
user      789  2.5  5.2 ... /usr/lib/firefox/firefox

# Kill it by PID
kill 789

# Or kill by name
pkill firefox
killall firefox
```

### Kill Signals

```bash
kill -<signal> <PID>
```

| Signal | Number | Description |
|--------|--------|-------------|
| **SIGTERM** | 15 | Terminate politely (asks to close) |
| **SIGKILL** | 9 | Kill immediately (cannot be ignored) |
| **SIGHUP** | 1 | Hang up (reload config) |
| **SIGINT** | 2 | Interrupt (Ctrl+C) |

```bash
# Try politely first
kill -15 789        # Same as: kill 789

# Force kill if stuck
kill -9 789         # Same as: kill -KILL 789
```

### pkill vs killall

```bash
# pkill - match by pattern
pkill firef         # Kills firefox, firefox-bin, etc.
pkill -u student    # Kill all processes for user 'student'

# killall - exact name match
killall firefox     # Kill all processes named 'firefox'
```

**Warning:** `killall` on some Unix systems kills ALL processes. Be careful!

---

## Background and Foreground Processes

### Running in Background

```bash
# Run command in background
sleep 60 &

# Background job is assigned a job number
[1] 12345

# Bring back to foreground
fg %1

# Send to background (Ctrl+Z, then bg)
# 1. Ctrl+Z - suspend current job
# 2. bg - resume in background
```

### Listing Jobs

```bash
jobs
[1]   Running                 sleep 60 &
[2]-  Running                 python3 script.py &
[3]+  Stopped                 vim file.txt
```

---

## systemd and Service Management

**systemd** is the init system and service manager in modern Linux. It manages system services (daemons) that run in the background.

### What are Services?

Services (daemons) are background processes that:
- Start at boot
- Run continuously
- Provide system functionality
- Examples: web servers, database servers, network managers

### systemctl - Control systemd Services

```bash
# Service status
sudo systemctl status ssh

# Start a service
sudo systemctl start nginx

# Stop a service
sudo systemctl stop nginx

# Restart a service
sudo systemctl restart nginx

# Reload (re-read config without restart)
sudo systemctl reload nginx

# Enable at boot
sudo systemctl enable nginx

# Disable at boot
sudo systemctl disable nginx

# Check if enabled
systemctl is-enabled nginx
```

### Viewing Service Status

```bash
$ sudo systemctl status ssh
● ssh.service - OpenSSH server daemon
     Loaded: loaded (/usr/lib/systemd/system/ssh.service; enabled; preset: enabled)
     Active: active (running) since Wed 2025-02-07 10:15:23 CET; 2h 34min ago
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

### Listing All Services

```bash
# List all services
systemctl list-units --type=service

# List all services (including inactive)
systemctl list-units --type=service --all

# List failed services
systemctl --failed

# List enabled services
systemctl list-unit-files --state=enabled
```

### Common Services

| Service | Purpose |
|---------|---------|
| `ssh` or `sshd` | SSH server for remote access |
| `NetworkManager` | Network connectivity |
| `firewalld` | Firewall management |
| `cups` | Printing service |
| `cron` or `systemd-cron` | Scheduled tasks |
| `nginx` | Web server |
| `docker` | Container management |

---

## System Logs

### journalctl - Query systemd Journal

systemd logs are stored in the **journal**, accessed with `journalctl`.

```bash
# Show all logs
sudo journalctl

# Follow logs (like tail -f)
sudo journalctl -f

# Show last 100 entries
sudo journalctl -n 100

# Show logs for specific service
sudo journalctl -u nginx
sudo journalctl -u ssh -f    # Follow ssh logs

# Show logs since boot
sudo journalctl -b

# Show logs since specific time
sudo journalctl --since "1 hour ago"
sudo journalctl --since "today"
sudo journalctl --since "2025-02-07" --until "2025-02-08"

# Show error logs only
sudo journalctl -p err

# Show kernel messages
sudo journalctl -k
```

### Log Priorities

```bash
# Filter by priority
journalctl -p 0    # emerg
journalctl -p 1    # alert
journalctl -p 2    # crit
journalctl -p 3    # err
journalctl -p 4    # warning
journalctl -p 5    # notice
journalctl -p 6    # info
journalctl -p 7    # debug
```

### Traditional Log Files

Some logs are still stored as text files in `/var/log/`:

```bash
# System logs
sudo tail /var/log/syslog        # Debian
sudo tail /var/log/messages      # Fedora

# Authentication logs
sudo tail /var/log/auth.log

# Kernel messages
sudo dmesg | tail

# Application logs
tail /var/log/nginx/access.log
tail /var/log/nginx/error.log
```

---

## Resource Monitoring

### System Resource Commands

```bash
# CPU and processes
top
htop

# Memory usage
free -h

# Disk usage
df -h

# Disk usage by directory
du -sh ~/Documents

# I/O monitoring
iotop        # needs installation

# Network monitoring
ss -tulpn
netstat -tulpn
```

### Understanding `free -h`

```bash
$ free -h
               total        used        free      shared  buff/cache   available
Mem:           15Gi       4.5Gi       6.2Gi       512Mi       4.8Gi       9.8Gi
Swap:          4.0Gi          0B       4.0Gi
```

| Column | Description |
|--------|-------------|
| **total** | Total RAM |
| **used** | Used by applications |
| **free** | Completely free |
| **shared** | Shared between processes (tmpfs) |
| **buff/cache** | Cached files (can be freed) |
| **available** | Available for new apps |
| **Swap** | Disk space used as memory overflow |

### Understanding `df -h`

```bash
$ df -h
Filesystem      Size  Used Avail Use% Mounted on
/dev/sda2        50G   22G   26G  46% /
/dev/sda1       512M  6.1M  506M   2% /boot/efi
/dev/sda3       200G  120G   70G  63% /home
```

---

## Practical Examples

### Example 1: Kill a Frozen Application

```bash
# Firefox is frozen
$ ps aux | grep firefox
user      789  95.0  5.2 3245680 423456 ?      Rl  Feb07 245:23 firefox

# Kill it
kill -9 789

# Or by name
pkill -9 firefox
```

### Example 2: Start a Web Server

```bash
# Install nginx
sudo dnf install nginx          # Fedora
sudo apt install nginx          # Debian

# Start the service
sudo systemctl start nginx

# Check status
sudo systemctl status nginx

# Enable at boot
sudo systemctl enable nginx

# Verify it's running
sudo systemctl is-active nginx
active
```

### Example 3: Monitor System Resources

```bash
# Launch htop for monitoring
htop

# Or use multiple terminals
# Terminal 1: CPU
watch -n 1 'ps aux | sort -rk 3 | head -n 10'

# Terminal 2: Memory
watch -n 1 free -h

# Terminal 3: Disk
watch -n 1 df -h
```

### Example 4: Debug Service Failures

```bash
# Service won't start
$ sudo systemctl start myservice
Job for myservice failed.

# Check the status for error details
$ sudo systemctl status myservice
● myservice.service - My Service
     Loaded: loaded (/usr/lib/systemd/system/myservice.service; enabled)
     Active: failed (Result: exit-code) since Wed 2025-02-07 10:15:23 CET

# View the logs
$ sudo journalctl -u myservice -n 50
-- Logs begin at Wed 2025-02-01 00:00:00 CET, end at Wed 2025-02-07 10:15:30 CET. --
Feb 07 10:15:23 hostname myservice[1234]: Error: Configuration file not found
Feb 07 10:15:23 hostname systemd[1]: myservice.service: Main process exited, code=exited, status=1/FAILURE
```

### Example 5: Find Resource-Hungry Processes

```bash
# Top 10 CPU users
ps aux | sort -rk 3 | head -n 10

# Top 10 memory users
ps aux | sort -rk 4 | head -n 10

# Or use htop and press M to sort by memory
```

---

## Troubleshooting

### Zombie Processes

Zombie processes are dead but waiting for parent to clean up.

```bash
# Find zombies
ps aux | grep Z

# Usually harmless, parent will clean up
# If persistent, kill the parent process
```

### High CPU Usage

```bash
# Find the culprit
top    # or htop

# Check if it's legitimate
ps -p 1234 -f

# If not needed, kill it
kill 1234
```

### Out of Memory (OOM)

When RAM is full, Linux uses swap or kills processes.

```bash
# Check memory usage
free -h

# Find memory hogs
ps aux | sort -rk 4 | head -n 10

# Check OOM killer logs
sudo journalctl -k | grep -i "out of memory"
```

### Service Won't Start

```bash
# Check status
sudo systemctl status service-name

# Check logs
sudo journalctl -u service-name -n 50

# Check config syntax
sudo systemd-analyze verify service-file

# Reload systemd
sudo systemctl daemon-reload
```

---

## Summary

In this chapter, you learned:

- **Processes**: Running programs with unique PIDs
- **Viewing Processes**: `ps`, `top`, `htop`, `pstree`
- **Killing Processes**: `kill`, `pkill`, `killall`
- **Background Jobs**: `&`, `fg`, `bg`, `jobs`
- **systemd Services**: `systemctl` to manage system services
- **System Logs**: `journalctl` for viewing logs
- **Resource Monitoring**: `free`, `df`, `du`, `top`

---

## Chapter Quiz

Test your understanding of processes and services:

{{#quiz ../../quizzes/chapter-09.toml}}

---

## Exercises

### Exercise 1: Process Exploration

1. Run `ps aux` and count the processes
2. Find your shell process
3. Find the systemd process (PID 1)
4. Use `pstree` to see the process hierarchy

### Exercise 2: Monitor with top/htop

1. Start `htop` (install if needed)
2. Sort by CPU (press P)
3. Sort by memory (press M)
4. Watch for 30 seconds and note the top processes

### Exercise 3: Service Management

1. Check if SSH service is running
2. View its status
3. Check if it's enabled at boot
4. View recent SSH logs with journalctl

### Exercise 4: Process Control

1. Start a long-running process in background: `sleep 300 &`
2. Find its PID
3. Bring it to foreground and then suspend with Ctrl+Z
4. Resume it in background
5. Kill the process

### Exercise 5: Logs and Troubleshooting

1. View the last 20 system log entries
2. Find any error or warning messages
3. Check logs from the last boot
4. View logs from a specific service (e.g., NetworkManager)

---

## Expected Output

### Exercise 1 Solution

```bash
$ ps aux | wc -l
245

$ ps aux | grep bash
user      2048  0.0  0.1  12548  9524 pts/0  Ss   10:15   0:00 -bash

$ ps aux | grep systemd
root         1  0.0  0.1 168336 11200 ?      Ss   Feb07   0:02 /sbin/init

$ pstree | head -n 15
systemd─┬─NetworkManager───2*[{NetworkManager}]
        ├─ModemManager───2*[{ModemManager}]
        ├─accounts-daemon───2*[{accounts-daemon}]
        ├─bash───pstree
        └─systemd─┬─(sd-pam)
                 └─systemd-logind
```

### Exercise 2 Solution

```bash
# Install htop first
$ sudo dnf install htop
[...]
Complete!

$ htop
# (Interactive output with colored bars)
# CPU: [||||||||||||||||||||||||||||||||||||||||||||||] 45%
# Mem: [||||||||||||||||||                            ] 2.5G/15G
# Swap: [                                             ] 0/4G
#
#   PID USER      PRI  NI  VIRT   RES   SHR S CPU% MEM%   TIME+  Command
#   789 user       20   0 3.2G   423M   45M S  5.0  2.7  0:45.23 firefox
#  1024 user       20   0  1.5G  120M   20M S  2.0  0.8  0:12.34 code
#     1 root       20   0  164M   11M   3M S  0.0  0.1  0:02.34 systemd
```

### Exercise 3 Solution

```bash
$ sudo systemctl status ssh
● ssh.service - OpenSSH server daemon
     Loaded: loaded (/usr/lib/systemd/system/ssh.service; enabled; preset: enabled)
     Active: active (running) since Wed 2025-02-07 10:15:23 CET; 2h ago

$ systemctl is-enabled ssh
enabled

$ sudo journalctl -u ssh -n 10
-- Logs begin at Wed 2025-02-01 00:00:00 CET --
Feb 07 10:15:23 hostname systemd[1]: Starting OpenSSH server daemon...
Feb 07 10:15:23 hostname sshd[789]: Server listening on 0.0.0.0 port 22.
Feb 07 10:20:45 hostname sshd[1234]: Accepted password for user from 192.168.1.100
```

### Exercise 4 Solution

```bash
$ sleep 300 &
[1] 5678

$ ps -p 5678
  PID TTY          TIME CMD
 5678 pts/0    00:00:00 sleep

$ jobs
[1]+  Running                 sleep 300 &

$ fg %1
sleep 300
^Z
[1]+  Stopped                 sleep 300

$ bg
[1]+ sleep 300 &

$ kill 5678
[1]+  Terminated              sleep 300
```

### Exercise 5 Solution

```bash
$ sudo journalctl -n 20
-- Logs begin at Wed 2025-02-01 00:00:00 CET, end at Wed 2025-02-07 12:34:56 CET. --
Feb 07 12:30:01 hostname CRON[4567]: (root) CMD (...)
Feb 07 12:34:12 hostname systemd[1]: Started User Manager for UID 1000
Feb 07 12:34:15 hostname gnome-shell[890]: Activating HUD

$ sudo journalctl -p err -n 10
-- Logs begin at Wed 2025-02-01 00:00:00 CET. --
Feb 07 10:15:23 hostname kernel: Out of memory: Killed process 1234
Feb 07 11:20:45 hostname sshd[2345]: error: Could not load host key

$ sudo journalctl -b -u NetworkManager -n 10
-- Logs begin at Wed 2025-02-07 10:00:00 CET. --
Feb 07 10:15:20 hostname NetworkManager[789]: <info>  [1707284920.234] manager: startup complete
Feb 07 10:15:25 hostname NetworkManager[789]: <info>  [1707284925.456] device (wlp3s0): state change: activated -> disconnected
```

---

## Next Chapter

In Chapter 10, you'll learn **Shell Scripting** - writing bash scripts to automate tasks, using variables, loops, conditionals, and functions to make your life easier.
