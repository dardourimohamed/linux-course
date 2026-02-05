# Chapter 7: Permissions & Users

## Learning Objectives

By the end of this chapter, you will be able to:

- [x] Understand Linux's permission model (read, write, execute)
- [x] Interpret `ls -l` output and permission strings
- [x] Change permissions using symbolic and numeric modes with `chmod`
- [x] Change file ownership with `chown` and `chgrp`
- [x] Use `sudo` to execute commands with elevated privileges
- [x] Manage users and groups with `useradd`, `usermod`, and `passwd`

## Prerequisites

- Completed Chapter 6: Text Processing
- Comfortable with basic file operations

---

## Understanding Linux Permissions

Linux is a multi-user system with built-in security. Every file and directory has permissions that control who can read, write, or execute it.

### The Three Permission Types

| Permission | Symbol | Description | For Files | For Directories |
|------------|--------|-------------|-----------|-----------------|
| **Read** | `r` | View contents | Can read file | Can list directory |
| **Write** | `w` | Modify contents | Can change file | Can add/remove files |
| **Execute** | `x` | Run as program | Can execute file | Can enter directory |

### The Three User Classes

| Class | Symbol | Description |
|-------|--------|-------------|
| **User (Owner)** | `u` | The file's owner |
| **Group** | `g` | Users in the file's group |
| **Others** | `o` | Everyone else |

```
Permissions:  rwx  rwx  rwx
              ^    ^    ^
              |    |    |
           User  Group Others
```

---

## Reading Permissions with ls -l

The `ls -l` command displays detailed file information including permissions.

### Format Breakdown

```bash
$ ls -l file.txt
-rw-r--r-- 1 student student 1234 Jan 15 10:30 file.txt
^----------^ ^  ^       ^      ^    ^
 permissions   | owner   group size date name
              links
```

### Permission String Structure

```
-  rw-  r--  r--
|  ^   ^   ^
|  |   |   |
|  |   |   +-- Others permissions
|  |   +------ Group permissions
|  +---------- User (owner) permissions
+-------------- File type
```

### File Type Characters

| Character | Type |
|-----------|------|
| `-` | Regular file |
| `d` | Directory |
| `l` | Symbolic link |
| `b` | Block device |
| `c` | Character device |
| `s` | Socket |
| `p` | Named pipe |

### Examples

```bash
drwxr-xr-x  # Directory: user=rwx, group=rx, others=rx
-rw-r--r--  # Regular file: user=rw, group=r, others=r
-rwxr-xr-x  # Executable: user=rwx, group=rx, others=rx
lrwxrwxrwx  # Symbolic link (all permissions shown)
-r--------  # Private file: only user can read
```

---

## Changing Permissions with chmod

`chmod` (change mode) modifies file permissions. You can use **symbolic** or **numeric** modes.

### Symbolic Mode (User-Friendly)

Add, remove, or set specific permissions.

```bash
# Syntax: chmod [who][operator][permissions] file

# Add execute permission for user
chmod u+x script.sh

# Remove write permission for group
chmod g-w file.txt

# Add read and write for others
chmod o+rw document.txt

# Set specific permissions
chmod u=rwx,g=rx,o=r script.sh

# Multiple changes at once
chmod u+x,g-w,o-r file.txt

# Apply to all (user, group, others)
chmod a+x script.sh          # Same as: chmod +x script.sh
```

**Operators:**

| Operator | Action |
|----------|--------|
| `+` | Add permission |
| `-` | Remove permission |
| `=` | Set exact permission |

### Numeric Mode (Power User)

Each permission has a numeric value:

| Permission | Value |
|------------|-------|
| `r` (read) | 4 |
| `w` (write) | 2 |
| `x` (execute) | 1 |

Add them up for combined permissions:

| Value | Permissions | Meaning |
|-------|-------------|---------|
| 0 | `---` | No permissions |
| 1 | `--x` | Execute only |
| 2 | `-w-` | Write only |
| 3 | `-wx` | Write + execute |
| 4 | `r--` | Read only |
| 5 | `r-x` | Read + execute |
| 6 | `rw-` | Read + write |
| 7 | `rwx` | All permissions |

**Common permission patterns:**

```bash
chmod 644 file.txt    # rw-r--r-- (standard file)
chmod 755 script.sh   # rwxr-xr-x (executable)
chmod 600 secret.txt  # rw------- (private file)
chmod 777 shared.txt  # rwxrwxrwx (all permissions - use carefully!)
```

### Recursive Permission Changes

Apply permissions to directories and all contents:

```bash
chmod -R 755 ~/public_html        # Make website readable
chmod -R u+x ~/scripts            # Add execute for user recursively

# Make all scripts executable
find . -name "*.sh" -exec chmod +x {} \;
```

---

## Changing Ownership with chown

`chown` (change owner) changes the user and/or group ownership of files.

### Basic Usage

```bash
# Change owner
sudo chown john file.txt

# Change group
sudo chown :developers file.txt

# Change both owner and group
sudo chown john:developers file.txt

# Recursive (directories)
sudo chown -R john:developers /path/to/directory

# Change owner only, keep group
sudo chown john: file.txt
```

### Common Use Cases

```bash
# Take ownership of a file
sudo chown $USER:$USER myfile.txt

# Fix ownership after sudo operations
sudo chown -R $USER:$USER ~/project

# Set up shared directory
sudo mkdir /shared
sudo chown john:team /shared
sudo chmod 770 /shared
```

### Changing Group with chgrp

`chgrp` is an alternative for changing group ownership only:

```bash
sudo chgrp team file.txt
sudo chgrp -R team /shared/project
```

---

## Special Permissions

Linux has three special permission bits:

### Set User ID (SUID)

When executed, the file runs with the permissions of the file's owner (not the user running it).

```bash
chmod u+s file            # Add SUID
chmod 4755 file           # SUID with rwxr-xr-x

# Example: passwd command needs to modify /etc/shadow
$ ls -l /usr/bin/passwd
-rwsr-xr-x 1 root root ...   # Note the 's' instead of 'x'
```

### Set Group ID (SGID)

Files created in the directory inherit the directory's group.

```bash
chmod g+s directory        # Add SGID
chmod 2755 directory       # SGID with rwxr-xr-x

# Useful for shared project directories
```

### Sticky Bit

Only the file owner can delete files in this directory (even if others have write permission).

```bash
chmod +t directory         # Add sticky bit
chmod 1755 directory       # Sticky bit with rwxr-xr-x

# Example: /tmp directory
$ ls -ld /tmp
drwxrwxrwt 10 root root ...   # Note the 't'
```

### Summary Table

| Bit | Prefix | Purpose | Example |
|-----|--------|---------|---------|
| SUID | 4 | Execute as owner | `4755` |
| SGID | 2 | Inherit group | `2755` |
| Sticky | 1 | Owner-only delete | `1755` |

---

## Sudo: Elevated Privileges

`sudo` (superuser do) allows you to execute commands with root (administrator) privileges.

### Basic Usage

```bash
sudo command                # Run single command as root
sudo -i                    # Start root shell
sudo -s                    # Start shell with current environment
sudo -u john command       # Run as specific user
```

### Why Use Sudo?

- **Security**: Don't stay logged in as root
- **Accountability**: Commands are logged
- **Least Privilege**: Only elevate when needed

### Common Sudo Commands

```bash
# Update system
sudo dnf update            # Fedora
sudo apt update            # Debian/Ubuntu

# Install software
sudo dnf install package
sudo apt install package

# System administration
sudo systemctl start nginx
sudo useradd newuser
sudoedit /etc/fstab        # Edit file with sudo
```

### Sudoers Configuration

The `/etc/sudoers` file controls who can use sudo and what they can do.

```bash
# View sudo privileges
sudo -l                    # List your sudo permissions

# Edit sudoers (use visudo!)
sudo visudo               # NEVER edit /etc/sudoers directly
```

**Example sudoers entries:**

```bash
# Allow user to run all commands
student ALL=(ALL:ALL) ALL

# Allow specific commands
student ALL=(ALL) /usr/bin/systemctl, /usr/bin/useradd

# No password required for specific commands
student ALL=(ALL) NOPASSWD: /usr/bin/systemctl status
```

---

## User and Group Management

Linux has multiple commands for managing users and groups.

### User Management

#### useradd - Create User

```bash
# Basic user creation
sudo useradd username

# Create with home directory
sudo useradd -m username

# Set specific shell
sudo useradd -s /bin/zsh username

# Add to groups during creation
sudo useradd -G wheel,docker username

# Create with specific home
sudo useradd -d /custom/home username

# View user details
id username
finger username            # (if installed)
```

#### usermod - Modify User

```bash
# Add user to group
sudo usermod -aG docker username

# Change login name
sudo usermod -l newname oldname

# Change home directory
sudo usermod -d /new/home -m username

# Lock/unlock account
sudo usermod -L username   # Lock
sudo usermod -U username   # Unlock

# Set expiry
sudo usermod -e 2025-12-31 username
```

#### passwd - Change Password

```bash
# Change your password
passwd

# Change another user's password (requires sudo)
sudo passwd username

# Force password change on next login
sudo chage -d 0 username

# View password expiry info
chage -l username
```

#### userdel - Delete User

```bash
# Delete user (keep home)
sudo userdel username

# Delete user and home directory
sudo userdel -r username
```

### Group Management

#### groupadd - Create Group

```bash
sudo groupadd developers
sudo groupadd -g 1001 developers    # Specify GID
```

#### groupmod - Modify Group

```bash
sudo groupmod -n newname oldname    # Rename group
sudo groupmod -g 2001 developers    # Change GID
```

#### groupdel - Delete Group

```bash
sudo groupdel developers
```

### Group Membership Commands

```bash
# View user's groups
groups
groups username

# View all groups
cat /etc/group
getent group

# Add user to group
sudo usermod -aG group username     # -aG is important (append)
```

> **Important**: Always use `-aG` (append) to avoid removing the user from existing groups!

---

## Practical Examples

### Example 1: Set Up a Project Directory

```bash
# Create project directory
mkdir -p ~/projects/myapp
cd ~/projects/myapp

# Create script
cat > run.sh << 'EOF'
#!/bin/bash
echo "Running my application..."
EOF

# Make executable
chmod +x run.sh

# Verify
ls -l run.sh
# Output: -rwxr-xr-x 1 student student ... run.sh
```

### Example 2: Secure Private Files

```bash
# Create private directory
mkdir -p ~/private

# Restrict permissions
chmod 700 ~/private

# Create secret file
echo "sensitive data" > ~/private/secret.txt

# Restrict file permissions
chmod 600 ~/private/secret.txt

# Verify
ls -la ~/private/
# drwx------  private/
# -rw-------  secret.txt
```

### Example 3: Shared Team Directory

```bash
# Create shared directory (as root)
sudo mkdir /shared
sudo chown -R $USER:team /shared
sudo chmod 2770 /shared         # SGID + rwxrwx---

# Team members can create files
# Files automatically belong to 'team' group
# Only team members can access
```

### Example 4: Web Server Setup

```bash
# Typical web directory permissions
sudo mkdir -p /var/www/html/mysite

# Ownership: www-data user (web server runs as this)
sudo chown -R www-data:www-data /var/www/html/mysite

# Permissions: directories 755, files 644
sudo find /var/www/html/mysite -type d -exec chmod 755 {} \;
sudo find /var/www/html/mysite -type f -exec chmod 644 {} \;

# Verify
ls -la /var/www/html/mysite/
```

### Example 5: Fix Permission Issues

```bash
# Files copied from Windows often have wrong permissions
# Fix executable scripts
find . -name "*.sh" -exec chmod +x {} \;

# Fix home directory permissions
chmod 700 ~/.ssh
chmod 600 ~/.ssh/id_rsa
chmod 644 ~/.ssh/id_rsa.pub

# Fix ownership after sudo operations
sudo chown -R $USER:$USER ~/project
```

---

## Default Permissions: umask

`umask` determines default permissions for new files.

### How umask Works

`umask` subtracts from default permissions (666 for files, 777 for directories).

```bash
# View current umask
umask
# Output: 0002 (common default)

# Calculate:
# Files: 666 - 022 = 644 (rw-r--r--)
# Directories: 777 - 022 = 755 (rwxr-xr-x)

# Set umask temporarily
umask 077                   # New files: 600 (rw-------)

# Set permanently (add to ~/.bashrc)
echo "umask 027" >> ~/.bashrc
```

### Common umask Values

| umask | File Permissions | Directory Permissions | Use Case |
|-------|------------------|----------------------|----------|
| 022 | 644 (rw-r--r--) | 755 (rwxr-xr-x) | Default (shared read) |
| 027 | 640 (rw-r-----) | 750 (rwxr-x---) | Group-private |
| 077 | 600 (rw-------) | 700 (rwx------) | Private |

---

## Summary

In this chapter, you learned:

- **Permission Model**: Read (r), Write (w), Execute (x) for User, Group, Others
- **Reading Permissions**: Interpret `ls -l` output
- **chmod**: Change permissions with symbolic (`u+x`) or numeric (`755`) modes
- **chown/chgrp**: Change file ownership
- **Special Permissions**: SUID (4), SGID (2), Sticky bit (1)
- **sudo**: Execute commands with elevated privileges
- **User Management**: `useradd`, `usermod`, `passwd`, `userdel`
- **Group Management**: `groupadd`, `groupmod`, `groupdel`
- **umask**: Control default permissions for new files

### Quick Reference

| Task | Command |
|------|---------|
| View permissions | `ls -l file.txt` |
| Make script executable | `chmod +x script.sh` |
| Set standard file perms | `chmod 644 file.txt` |
| Set standard dir perms | `chmod 755 directory/` |
| Change owner | `sudo chown user file.txt` |
| Add to group | `sudo usermod -aG group user` |
| Run as root | `sudo command` |
| View groups | `groups` or `groups username` |

---

## Chapter Quiz

Test your understanding of Linux permissions and user management:

{{#quiz ../../quizzes/chapter-07.toml}}

---

## Exercises

### Exercise 1: Permission Practice

Create and modify file permissions:

```bash
cd ~/linux-course/session01
touch testfile.txt
ls -l testfile.txt
```

1. Note the default permissions
2. Add execute permission for owner only
3. Remove read permission for group
4. Set permissions to `rwxr-xr--` using numeric mode
5. Verify each step with `ls -l`

### Exercise 2: Create Executable Script

1. Create a script that prints "Hello, Linux!"
2. Make it executable
3. Run it
4. Explain the execute permission's role

### Exercise 3: User Management (Practice Only)

These commands are for practice - don't actually create users on a shared system!

1. Write the command to create user "johndoe"
2. Write the command to set johndoe's password
3. Write the command to add johndoe to the "docker" group
4. Write the command to view johndoe's groups

### Exercise 4: Permission Troubleshooting

Given this scenario:
```bash
$ ls -l script.sh
-rw-r--r-- 1 student student 50 Jan 15 10:00 script.sh

$ ./script.sh
bash: ./script.sh: Permission denied
```

1. Why does this fail?
2. What command fixes it?
3. What would the permissions be after fixing?

### Exercise 5: Set Up Shared Directory

Design commands for a shared team directory:

1. Create `/shared/team` (requires sudo)
2. Set ownership to `alice:developers`
3. Set permissions so:
   - Team members can read/write/execute
   - Others have no access
4. Enable SGID so new files inherit the group

---

## Expected Output

### Exercise 1 Solution

```bash
$ cd ~/linux-course/session01
$ touch testfile.txt
$ ls -l testfile.txt
-rw-r--r-- 1 student student 0 Jan 15 10:00 testfile.txt

$ chmod u+x testfile.txt
$ ls -l testfile.txt
-rwxr--r-- 1 student student 0 Jan 15 10:00 testfile.txt

$ chmod g-r testfile.txt
$ ls -l testfile.txt
-rwx---r-- 1 student student 0 Jan 15 10:00 testfile.txt

$ chmod 754 testfile.txt
$ ls -l testfile.txt
-rwxr-xr-- 1 student student 0 Jan 15 10:00 testfile.txt
```

### Exercise 2 Solution

```bash
$ cat > hello.sh << 'EOF'
#!/bin/bash
echo "Hello, Linux!"
EOF

$ chmod +x hello.sh
$ ls -l hello.sh
-rwxr-xr-x 1 student student 20 Jan 15 10:00 hello.sh

$ ./hello.sh
Hello, Linux!
```

**Explanation**: The execute permission allows the shell to treat the file as a program and run it. Without it, Linux doesn't know the file should be executed.

### Exercise 3 Solution

```bash
# These are the commands (don't run on shared systems)

# 1. Create user with home directory
sudo useradd -m johndoe

# 2. Set password
sudo passwd johndoe

# 3. Add to docker group (always use -aG!)
sudo usermod -aG docker johndoe

# 4. View groups
groups johndoe
# or
id johndoe
```

### Exercise 4 Solution

**Why it fails**: The script doesn't have execute permission (`rw-r--r--`)

**Fix**: `chmod +x script.sh` or `chmod 744 script.sh`

**After fixing**:
```bash
$ chmod +x script.sh
$ ls -l script.sh
-rwxr-xr-x 1 student student 50 Jan 15 10:00 script.sh
```

### Exercise 5 Solution

```bash
# 1. Create directory
sudo mkdir -p /shared/team

# 2. Set ownership
sudo chown alice:developers /shared/team

# 3. Set permissions (rwxrwx--- = 770)
sudo chmod 770 /shared/team

# 4. Enable SGID (2 prefix)
sudo chmod 2770 /shared/team

# Verify
ls -ld /shared/team
# drwxrws--- 2 alice developers 4096 Jan 15 10:00 /shared/team
```

---

## End of Part II: CLI Mastery

Congratulations! You've completed Part II and now have solid command-line skills. You can:

- Navigate the Linux file system confidently
- Process text with powerful tools like grep, sed, and awk
- Manage permissions and users effectively
- Build complex command pipelines

In **Part III: System Administration**, you'll learn about package management, processes, shell scripting, and networking - essential skills for managing a Linux system.
