# Chapter 9 Quiz: Processes & Services

## Quiz Questions

### Multiple Choice

**1. What is a PID?**
- A) Personal Identifier for users
- B) Process ID - unique number for each running process
- C) Program Installation Directory
- D) Parent Interface Daemon

**Answer:** B

**2. Which command shows a snapshot of running processes?**
- A) top
- B) htop
- C) ps
- D) kill

**Answer:** C

**3. What does the systemd init system manage?**
- A) Only web servers
- B) System services (daemons) that run in the background
- C) Only user applications
- D) File system permissions

**Answer:** B

**4. Which signal politely asks a process to terminate?**
- A) SIGKILL (9)
- B) SIGTERM (15)
- C) SIGHUP (1)
- D) SIGINT (2)

**Answer:** B

**5. How do you view logs from a specific service with journalctl?**
- A) journalctl service-name
- B) journalctl -u service-name
- C) journalctl --service service-name
- D) journalctl view service-name

**Answer:** B

### True/False

**6. True or False: The init process (systemd) always has PID 1.**

**Answer:** True

**7. True or False: Zombie processes are normal and will be cleaned up by the parent.**

**Answer:** True

**8. True or False: htop is more user-friendly than top.**

**Answer:** True

**9. True or False: The kill command can only terminate processes.**

**Answer:** False (it can send various signals)

**10. True or False: Systemd services can be configured to start automatically at boot.**

**Answer:** True

### Short Answer

**11. What are the five key process states shown in the STAT column of ps output?**

**Answer:**
1. **R (Running)**: Currently running or runnable
2. **S (Sleeping)**: Waiting for something (I/O, input)
3. **D (Uninterruptible)**: Waiting for I/O, cannot be interrupted
4. **Z (Zombie)**: Completed but not yet cleaned up by parent
5. **T (Stopped)**: Paused (usually by SIGSTOP or Ctrl+Z)

**12. What are the main systemctl commands for managing services?**

**Answer:**
- `systemctl status service` - Check if service is running
- `systemctl start service` - Start a stopped service
- `systemctl stop service` - Stop a running service
- `systemctl restart service` - Restart a service
- `systemctl reload service` - Reload configuration without restart
- `systemctl enable service` - Start service at boot
- `systemctl disable service` - Don't start service at boot
- `systemctl is-active service` - Check if service is running
- `systemctl is-enabled service` - Check if enabled at boot

**13. Explain the difference between kill, pkill, and killall.**

**Answer:**
- **`kill PID`**: Terminates a specific process by its PID. You need to know the exact process ID. Most precise.

- **`pkill pattern`**: Kills processes by name pattern. Kills all processes matching the pattern name. Convenient when you don't know the PID.

- **`killall name`**: Kills all processes with the exact name specified. Requires exact name match. Be careful - on some Unix systems, killall kills ALL processes!

### Discussion Question

**14. Why is understanding process management critical for a Linux system administrator? Discuss scenarios where improper process management could cause system problems, and explain how tools like top, htop, systemctl, and journalctl help diagnose and resolve issues.**

**Sample Answer:**

**Why Process Management Matters:**

1. **Resource Management**: Identify and terminate processes consuming excessive CPU or RAM that slow down the system.

2. **Service Reliability**: Ensure critical services (web server, database) are running and restart them if they fail.

3. **Troubleshooting**: When something's wrong, process status and logs reveal what's happening.

4. **Security**: Identify suspicious processes that could indicate malware or intrusion.

5. **Performance**: Monitor system health and identify bottlenecks before they become problems.

**Problem Scenarios:**

**High CPU Usage Causing Slowdown:**
- Problem: Firefox using 95% CPU, system unresponsive
- Diagnosis: `top` or `htop` shows CPU usage
- Resolution: `kill -TERM PID` to restart Firefox gracefully

**Web Server Down:**
- Problem: Website not loading
- Diagnosis: `systemctl status nginx` shows service failed
- Resolution: `journalctl -u nginx -n 50` shows error, fix config, `systemctl restart nginx`

**Zombie Processes Accumulating:**
- Problem: Too many zombie processes
- Diagnosis: `ps aux | grep Z` shows zombies
- Resolution: Kill or restart parent process to clean up zombies

**Service Won't Start:**
- Problem: Service fails to start after configuration change
- Diagnosis: `journalctl -u service -xe` shows detailed error
- Resolution: Fix configuration error found in logs

**Memory Leak:**
- Problem: Available memory decreasing over time
- Diagnosis: `ps aux | sort -rk 4 | head -n 10` shows memory hogs
- Resolution: Restart leaking process or investigate for bug fix

**Tools for Diagnosis:**
- **top/htop**: Real-time view of all processes and resource usage
- **ps**: Snapshot of process state
- **systemctl**: Service status and management
- **journalctl**: Detailed logs for troubleshooting
- **kill**: Terminate hung or problematic processes

**Conclusion**: Process management is essential for maintaining healthy, secure, and performant Linux systems. Without these skills, administrators would be flying blind when problems occur.

## Answer Key

| Question | Answer | Points |
|----------|--------|--------|
| 1 | B | 1 |
| 2 | C | 1 |
| 3 | B | 1 |
| 4 | B | 1 |
| 5 | B | 1 |
| 6 | True | 1 |
| 7 | True | 1 |
| 8 | True | 1 |
| 9 | False | 1 |
| 10 | True | 1 |
| 11 | Five states explained | 4 |
| 12 | Main systemctl commands | 3 |
| 13 | Three kill commands explained | 3 |
| 14 | Scenarios and tools discussed | 5 |

**Total Points:** 27

**Passing Score:** 18/27 (67%)

---

## Instructor Notes

### Common Misconceptions to Address

1. **"Kill always destroys data"** - SIGTERM (kill without -9) gives the process a chance to clean up.

2. **"High memory usage is always bad"** - Some applications (databases) legitimately use lots of memory.

3. **"Zombie processes are eating my system"** - Zombies are usually harmless and normal.

4. **"I need to restart the whole system to fix a service"** - Usually just restarting the service is enough.

### Teaching Tips

- **Live monitoring demo**: Run htop during class to show real-time process activity
- **Service demonstration**: Start and stop a real service (like nginx or ssh)
- **Kill signals demo**: Show the difference between kill -15 and kill -9
- **Log exploration**: Use journalctl to show how logs reveal what happened
- **Process tree**: Use pstree to visualize parent-child relationships

### Extension Activities

- Process detective: Find and identify mysterious processes
- Service management lab: Set up and manage multiple services
- Log analysis challenge: Diagnose problems from journalctl output
- Create a monitoring dashboard script using ps, top, and journalctl
- Research cgroups and how they limit process resources
- Compare systemd with other init systems (SysVinit, runit)
