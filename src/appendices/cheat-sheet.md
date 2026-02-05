# Cheat Sheet

Quick reference for the most essential Linux commands.

## Navigation

| Command | Description |
|---------|-------------|
| `pwd` | Print working directory |
| `ls` | List directory contents |
| `cd <path>` | Change directory |
| `cd ~` | Go to home directory |
| `cd -` | Go to previous directory |

## File Operations

| Command | Description |
|---------|-------------|
| `touch <file>` | Create empty file |
| `mkdir <dir>` | Create directory |
| `cp <src> <dst>` | Copy file/directory |
| `mv <src> <dst>` | Move/rename file |
| `rm <file>` | Remove file |
| `rm -r <dir>` | Remove directory |

## Viewing Files

| Command | Description |
|---------|-------------|
| `cat <file>` | Display file contents |
| `less <file>` | View file with pagination |
| `head <file>` | Show first 10 lines |
| `tail <file>` | Show last 10 lines |
| `tail -f <file>` | Follow file (live updates) |

## Search

| Command | Description |
|---------|-------------|
| `grep <pattern> <file>` | Search in file |
| `grep -r <pattern> <dir>` | Search recursively |
| `find <dir> -name <pattern>` | Find files |
| `locate <name>` | Quick file search |

## Permissions

| Command | Description |
|---------|-------------|
| `chmod <perms> <file>` | Change permissions |
| `chown <user> <file>` | Change owner |
| `sudo <command>` | Run as superuser |
| `chmod +x <script>` | Make executable |

## Package Management

### Fedora (DNF)

| Command | Description |
|---------|-------------|
| `dnf search <pkg>` | Search package |
| `dnf install <pkg>` | Install package |
| `dnf remove <pkg>` | Remove package |
| `dnf update` | Update packages |

### Debian (APT)

| Command | Description |
|---------|-------------|
| `apt search <pkg>` | Search package |
| `apt install <pkg>` | Install package |
| `apt remove <pkg>` | Remove package |
| `apt update && apt upgrade` | Update system |

## Processes

| Command | Description |
|---------|-------------|
| `ps aux` | Show all processes |
| `top` | Interactive process viewer |
| `kill <pid>` | Terminate process |
| `systemctl status <svc>` | Service status |
| `journalctl -u <svc>` | Service logs |

## Networking

| Command | Description |
|---------|-------------|
| `ip addr` | Show IP addresses |
| `ping <host>` | Test connectivity |
| `ssh user@host` | Remote login |
| `scp <src> <dst>` | Secure copy |

## Git

| Command | Description |
|---------|-------------|
| `git init` | Initialize repo |
| `git status` | Show status |
| `git add <file>` | Stage changes |
| `git commit -m "msg"` | Commit |
| `git log` | Show history |

## Docker

| Command | Description |
|---------|-------------|
| `docker pull <img>` | Pull image |
| `docker run <img>` | Run container |
| `docker ps` | List containers |
| `docker stop <id>` | Stop container |

## Keyboard Shortcuts

| Shortcut | Description |
|----------|-------------|
| `Tab` | Auto-complete |
| `Ctrl+C` | Cancel command |
| `Ctrl+L` | Clear screen |
| `Ctrl+A` | Start of line |
| `Ctrl+E` | End of line |
| `Ctrl+U` | Clear to start |
| `Ctrl+K` | Clear to end |
| `Ctrl+R` | Search history |
| `↑/↓` | Browse history |
