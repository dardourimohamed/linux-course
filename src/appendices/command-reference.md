# Command Reference

Comprehensive reference for commands covered in this course.

## File System Commands

### `pwd` - Print Working Directory

**Syntax:** `pwd [options]`

**Common Options:**
- `-P` - Physical path (resolve symlinks)

**Example:**
```bash
$ pwd
/home/student
```

---

### `ls` - List Directory Contents

**Syntax:** `ls [options] [directory]`

**Common Options:**
- `-l` - Long format (permissions, owner, size, date)
- `-a` - Include hidden files (starting with .)
- `-h` - Human-readable sizes
- `-R` - Recursive listing
- `-t` - Sort by modification time

**Examples:**
```bash
$ ls -lh
total 16K
drwxr-xr-x 2 student student 4.0K Feb  5 12:00 documents
-rw-r--r-- 1 student student  123 Feb  5 12:00 notes.txt

$ ls -la
total 32K
drwxr-xr-x 5 student student 4.0K Feb  5 12:00 .
drwxr-xr-x 3 root     root     4.0K Feb  4 00:00 ..
-rw-r--r-- 1 student student  123 Feb  5 12:00 notes.txt
drwx------ 2 student student 4.0K Feb  5 12:00 .ssh
```

---

### `cd` - Change Directory

**Syntax:** `cd [directory]`

**Special Paths:**
- `~` - Home directory
- `-` - Previous directory
- `..` - Parent directory
- `.` - Current directory

**Examples:**
```bash
$ cd /var/log
$ pwd
/var/log

$ cd ~
$ pwd
/home/student

$ cd -
/var/log
```

---

### `mkdir` - Make Directory

**Syntax:** `mkdir [options] <directory>`

**Common Options:**
- `-p` - Create parent directories as needed
- `-v` - Verbose (print each directory)

**Examples:**
```bash
$ mkdir project
$ mkdir -p parent/child/grandchild
$ ls
parent  project
```

---

### `touch` - Create Empty File / Update Timestamp

**Syntax:** `touch [options] <file>`

**Common Options:**
- `-c` - Don't create if doesn't exist
- `-d` - Use specified date/time

**Examples:**
```bash
$ touch newfile.txt
$ ls
newfile.txt
```

---

### `cp` - Copy Files/Directories

**Syntax:** `cp [options] <source> <destination>`

**Common Options:**
- `-r` - Recursive (for directories)
- `-i` - Interactive (prompt before overwrite)
- `-v` - Verbose
- `-p` - Preserve attributes

**Examples:**
```bash
$ cp file.txt backup.txt
$ cp -r src/ dst/
```

---

### `mv` - Move/Rename Files

**Syntax:** `mv [options] <source> <destination>`

**Common Options:**
- `-i` - Interactive (prompt before overwrite)
- `-v` - Verbose

**Examples:**
```bash
$ mv oldname.txt newname.txt
$ mv file.txt /tmp/
```

---

### `rm` - Remove Files/Directories

**Syntax:** `rm [options] <file>`

**Common Options:**
- `-r` - Recursive (for directories)
- `-i` - Interactive (prompt before removal)
- `-f` - Force (ignore nonexistent files)

**Warning:** `rm` is permanent. Use with caution!

**Examples:**
```bash
$ rm file.txt
$ rm -r directory/
```

## Text Processing Commands

### `cat` - Concatenate and Display Files

**Syntax:** `cat [options] <file>`

**Common Options:**
- `-n` - Number lines
- `-b` - Number non-blank lines

**Example:**
```bash
$ cat -n file.txt
     1	First line
     2	Second line
```

---

### `less` - Page Through Files

**Syntax:** `less <file>`

**Navigation:**
- `Space` / `f` - Forward one page
- `b` - Backward one page
- `g` - Go to first line
- `G` - Go to last line
- `/pattern` - Search forward
- `q` - Quit

---

### `head` / `tail` - Display File Start/End

**Syntax:**
```bash
head [options] <file>
tail [options] <file>
```

**Common Options:**
- `-n N` - Show N lines (default 10)
- `-f` - Follow file (tail only, live updates)

**Examples:**
```bash
$ head -n 5 file.txt    # First 5 lines
$ tail -f /var/log/syslog  # Follow log file
```

---

### `grep` - Print Matching Lines

**Syntax:** `grep [options] <pattern> <file>`

**Common Options:**
- `-i` - Case insensitive
- `-r` - Recursive directory search
- `-v` - Invert match (show non-matching)
- `-n` - Show line numbers
- `-c` - Count matches

**Examples:**
```bash
$ grep "error" log.txt
ERROR: Connection failed

$ grep -r "TODO" src/
src/main.c: // TODO: Fix this

$ grep -n "main" file.txt
42: int main() {
```

---

### `find` - Search for Files

**Syntax:** `find [path] [options]`

**Common Tests:**
- `-name <pattern>` - Name matches pattern
- `-type <type>` - File type (f=file, d=directory)
- `-size <n>` - File size
- `-mtime <n>` - Modified n days ago

**Examples:**
```bash
$ find . -name "*.txt"
./notes.txt
./todo.txt

$ find /var/log -type f -name "*.log"
/var/log/system.log

$ find . -type f -mtime -7    # Modified in last 7 days
```

## Permission Commands

### `chmod` - Change Mode (Permissions)

**Syntax:** `chmod [options] <mode> <file>`

**Symbolic Mode:**
```bash
chmod u+x file    # Add execute for user
chmod g-w file    # Remove write for group
chmod o=r file    # Set read-only for others
chmod a+rw file   # Add read-write for all
```

**Numeric Mode:**
```bash
chmod 755 file    # rwxr-xr-x
chmod 644 file    # rw-r--r--
chmod 777 file    # rwxrwxrwx (not recommended)
```

**Permission Values:**
- `4` = Read (r)
- `2` = Write (w)
- `1` = Execute (x)

---

### `chown` - Change Owner

**Syntax:** `chown [options] <user>[:<group>] <file>`

**Examples:**
```bash
$ sudo chown john file.txt
$ sudo chown john:developers file.txt
$ sudo chown -R john:john directory/    # Recursive
```

---

### `sudo` - Execute as Superuser

**Syntax:** `sudo [options] <command>`

**Examples:**
```bash
$ sudo apt install nginx
$ sudo systemctl restart ssh
$ sudo -u postgres psql    # Run as postgres user
```

## Process Commands

### `ps` - Process Status

**Syntax:** `ps [options]`

**Common Options:**
- `aux` - Show all processes for all users
- `-u <user>` - Show processes for user
- `-p <pid>` - Show specific PID

**Example:**
```bash
$ ps aux
USER   PID  %CPU  %MEM  COMMAND
root     1   0.0   0.1  systemd
root   123   0.5   1.2  nginx
```

---

### `top` / `htop` - Process Monitor

**Interactive process viewers.**

**top keys:**
- `q` - Quit
- `k` - Kill process
- `M` - Sort by memory
- `P` - Sort by CPU

---

### `kill` - Send Signal to Process

**Syntax:** `kill [options] <pid>`

**Common Signals:**
- `SIGTERM` (15) - Terminate gracefully
- `SIGKILL` (9) - Force kill
- `SIGHUP` (1) - Reload configuration

**Examples:**
```bash
$ kill 1234              # Graceful terminate
$ kill -9 1234           # Force kill
$ kill -HUP 1234         # Reload config
```

## Package Management

### Fedora (DNF)

| Command | Description |
|---------|-------------|
| `sudo dnf search <pkg>` | Search for package |
| `sudo dnf install <pkg>` | Install package |
| `sudo dnf remove <pkg>` | Remove package |
| `sudo dnf update` | Update all packages |
| `sudo dnf upgrade` | Upgrade distribution |
| `sudo dnf list installed` | List installed packages |
| `sudo dnf info <pkg>` | Show package info |
| `sudo dnf repolist` | List repositories |

### Debian (APT)

| Command | Description |
|---------|-------------|
| `sudo apt update` | Update package lists |
| `sudo apt upgrade` | Upgrade packages |
| `sudo apt install <pkg>` | Install package |
| `sudo apt remove <pkg>` | Remove package |
| `sudo apt purge <pkg>` | Remove with config |
| `sudo apt search <pkg>` | Search packages |
| `sudo apt show <pkg>` | Show package info |
| `sudo apt autoremove` | Remove unused packages |

## Systemd Commands

### `systemctl` - Control systemd System

**Syntax:** `systemctl [command] [unit]`

**Commands:**
```bash
sudo systemctl start <service>     # Start service
sudo systemctl stop <service>      # Stop service
sudo systemctl restart <service>   # Restart service
sudo systemctl status <service>    # Show status
sudo systemctl enable <service>    # Enable at boot
sudo systemctl disable <service>   # Disable at boot
sudo systemctl reload <service>    # Reload config
systemctl list-units --type=service    # List services
systemctl --failed                   # List failed units
```

**Examples:**
```bash
$ sudo systemctl start nginx
$ sudo systemctl enable nginx
$ systemctl status nginx
```

---

### `journalctl` - Query systemd Journal

**Syntax:** `journalctl [options]`

**Common Options:**
- `-u <unit>` - Show logs for specific unit
- `-f` - Follow logs (live)
- `-b` - Show logs from current boot
- `-r` - Show in reverse order
- `--since "time"` - Show logs since time
- `--until "time"` - Show logs until time

**Examples:**
```bash
$ journalctl -u nginx          # Nginx logs
$ journalctl -u nginx -f       # Follow nginx logs
$ journalctl -b -p err         # Errors from current boot
$ journalctl --since "1 hour ago"
```

## Networking Commands

### `ip` - Network Configuration

**Show IP addresses:**
```bash
$ ip addr show
$ ip a      # Short form
```

**Show routes:**
```bash
$ ip route show
```

---

### `ping` - Test Network Connectivity

**Syntax:** `ping [options] <host>`

**Options:**
- `-c <count>` - Send count packets
- `-i <interval>` - Interval between packets

**Example:**
```bash
$ ping -c 4 google.com
PING google.com (142.250.185.46): 56 data bytes
64 bytes from 142.250.185.46: icmp_seq=0 ttl=54 time=12.3 ms
```

---

### `ssh` - Secure Shell

**Syntax:** `ssh [options] [user@]host [command]`

**Common Options:**
- `-p <port>` - Specify port
- `-i <identity>` - Use identity file
- `-v` - Verbose

**Examples:**
```bash
$ ssh user@server.com
$ ssh -p 2222 user@server.com
$ ssh user@server.com 'ls -la'
```

**SSH Key Management:**
```bash
$ ssh-keygen -t ed25519      # Generate key
$ ssh-copy-id user@host      # Copy public key
```

---

### `scp` - Secure Copy

**Syntax:** `scp [options] <source> <destination>`

**Examples:**
```bash
$ scp file.txt user@host:/path/    # Upload
$ scp user@host:/path/file.txt .   # Download
$ scp -r directory/ user@host:/path/  # Recursive
```

## Git Commands

### Basic Git Commands

| Command | Description |
|---------|-------------|
| `git init` | Initialize new repository |
| `git clone <url>` | Clone existing repository |
| `git status` | Show working tree status |
| `git add <file>` | Stage file for commit |
| `git add .` | Stage all changes |
| `git commit -m "msg"` | Commit staged changes |
| `git log` | Show commit history |
| `git log --oneline` | Concise commit history |
| `git diff` | Show unstaged changes |
| `git diff --staged` | Show staged changes |
| `git branch` | List branches |
| `git branch <name>` | Create branch |
| `git checkout <branch>` | Switch branch |
| `git switch <branch>` | Switch branch (newer) |
| `git merge <branch>` | Merge branch into current |
| `git remote -v` | Show remotes |
| `git push` | Push to remote |
| `git pull` | Pull from remote |
| `git stash` | Stash changes |
| `git stash pop` | Apply stashed changes |

## Docker Commands

### Basic Docker Commands

| Command | Description |
|---------|-------------|
| `docker pull <image>` | Pull image from registry |
| `docker run [options] <image>` | Run container |
| `docker ps` | List running containers |
| `docker ps -a` | List all containers |
| `docker stop <container>` | Stop container |
| `docker start <container>` | Start stopped container |
| `docker rm <container>` | Remove container |
| `docker rmi <image>` | Remove image |
| `docker images` | List images |
| `docker exec -it <container> bash` | Execute in container |
| `docker logs <container>` | Show container logs |
| `docker inspect <object>` | Show low-level info |

### Common Run Options

```bash
docker run -d              # Detached (background)
docker run -it             # Interactive + TTY
docker run -p 80:80        # Port mapping host:container
docker run -v /host:/container  # Volume mount
docker run --name myname   # Name container
docker run --rm           # Remove on exit
```

### Examples

```bash
# Run nginx in background
docker run -d -p 80:80 --name web nginx

# Run Ubuntu interactively
docker run -it ubuntu bash

# Run with volume mount
docker run -v $(pwd):/app -w /app python:3.11 python script.py
```
