# Chapter 4: File System & Navigation

## Learning Objectives

By the end of this chapter, you will be able to:

- [x] Understand the Linux file system hierarchy
- [x] Navigate directories using `cd`, `pwd`, and `ls`
- [x] Create and remove directories and files
- [x] Differentiate between absolute and relative paths
- [x] Use tab completion to work more efficiently
- [x] Understand home directories and the `~` shortcut

## Prerequisites

- Completed Part I: Foundations
- Have a working Linux installation with terminal access

---

## The Linux File System Hierarchy

Unlike Windows, which uses drive letters (C:, D:, etc.), Linux uses a single unified directory tree starting at the **root** directory, represented by `/` (forward slash).

```
/ (root)
├── bin/        # Essential user binaries (ls, cp, cat, etc.)
├── boot/       # Boot loader files
├── dev/        # Device files
├── etc/        # Configuration files
├── home/       # User home directories
│   ├── student/
│   └── user/
├── lib/        # System libraries
├── media/      # Removable media mount points
├── mnt/        # Mount point for temporary filesystems
├── opt/        # Optional software packages
├── proc/       # Process and kernel information
├── root/       # Home directory for root user
├── run/        # Runtime data
├── sbin/       # System binaries
├── srv/        # Service data
├── sys/        # Kernel and hardware information
├── tmp/        # Temporary files
└── var/        # Variable data (logs, spool, etc.)
```

### Key Directories to Remember

| Directory | Purpose | What You'll Find There |
|-----------|---------|----------------------|
| `/home/username` | **Your home directory** | Your personal files, documents, downloads |
| `/bin` | Binaries | Essential commands like `ls`, `cp`, `cat` |
| `/etc` | Etcetera (config) | System-wide configuration files |
| `/var` | Variable data | Logs, web server files, mail queues |
| `/tmp` | Temporary | Files that don't need to persist |

---

## Essential Navigation Commands

### pwd - Print Working Directory

Shows your current location in the file system.

```bash
$ pwd
/home/student
```

### ls - List Directory Contents

Shows files and directories in your current location.

```bash
$ ls
Documents  Downloads  Music  Pictures  Videos
```

**Common `ls` options:**

| Option | Purpose | Example |
|--------|---------|---------|
| `-l` | Long format (details) | `ls -l` |
| `-a` | Show hidden files (dotfiles) | `ls -a` |
| `-h` | Human-readable sizes | `ls -lh` |
| `-la` | Combined: long + all | `ls -la` |
| `-t` | Sort by time modified | `ls -lt` |

```bash
$ ls -la
drwxr-xr-x 5 student student 4096 Jan 15 10:30 Documents
drwxr-xr-x 2 student student 4096 Jan 14 09:15 Downloads
-rw-r--r-- 1 student student  123 Jan 15 10:00 notes.txt
```

### cd - Change Directory

Navigate to different directories.

```bash
cd           # Go to home directory (same as cd ~)
cd ..        # Go up one level (parent directory)
cd /         # Go to root directory
cd -         # Go to previous directory
cd ~/Music   # Go to Music subdirectory in home
```

**Examples:**

```bash
$ pwd
/home/student

$ cd /etc
$ pwd
/etc

$ cd ..
$ pwd
/

$ cd -
$ pwd
/etc
```

---

## Paths: Absolute vs. Relative

Understanding paths is fundamental to Linux navigation.

### Absolute Paths

Start from `/` (root) and specify the complete path.

```bash
cd /home/student/Documents
cd /etc/nginx
cd /var/log
```

**When to use:** When you want to be precise about the location, regardless of where you currently are.

### Relative Paths

Start from your **current directory** (also called "working directory").

```bash
cd Documents           # Go into Documents in current directory
cd ../Pictures         # Go up one level, then into Pictures
cd ./Music/songs       # Explicit reference to current directory
```

**Special path symbols:**

| Symbol | Meaning |
|--------|---------|
| `.` | Current directory |
| `..` | Parent directory (one level up) |
| `~` | Home directory |
| `/` | Root directory |

### Visual Example

```
/
└── home/
    └── student/
        ├── Documents/
        │   └── project.txt
        ├── Downloads/
        └── Pictures/
```

If you're in `/home/student/Documents`:

| Command | Destination | Path Type |
|---------|-------------|-----------|
| `cd /home/student` | `/home/student` | Absolute |
| `cd ..` | `/home/student` | Relative |
| `cd ../Pictures` | `/home/student/Pictures` | Relative |
| `cd ~/Pictures` | `/home/student/Pictures` | Absolute (with ~) |

---

## Creating and Removing Directories

### mkdir - Make Directory

Create new directories.

```bash
mkdir project                    # Create single directory
mkdir -p a/b/c                  # Create nested directories (parents)
mkdir -p ~/linux-course/session{01..10}  # Create multiple directories
```

The `-p` flag is particularly useful:
- Creates parent directories if they don't exist
- Doesn't error if directory already exists

### rmdir - Remove Directory

Remove **empty** directories only.

```bash
rmdir empty-folder
```

### rm - Remove Files and Directories

Remove files and directories (use with caution!).

```bash
rm file.txt                     # Remove file (asks for confirmation)
rm -f file.txt                  # Force remove (no confirmation)
rm -r directory/                # Remove directory and contents
rm -rf directory/               # Force remove directory recursively
```

> **Warning:** `rm -rf` is permanent. There's no recycle bin in the terminal!

---

## Creating Files

### touch - Create Empty File

Create an empty file or update timestamps.

```bash
touch notes.txt                 # Create new empty file
touch report.md todo.txt        # Create multiple files
```

### Creating Files with Content

You'll learn more about text editors and redirection in Chapter 5, but here's a quick preview:

```bash
echo "Hello, Linux!" > hello.txt    # Create file with content
cat > newfile.txt << EOF
Line 1
Line 2
Line 3
EOF
```

---

## Tab Completion: Your Best Friend

Tab completion saves time and prevents typos. Start typing a command or filename, then press **Tab**.

### How It Works

```bash
$ cd Doc[TAB]          # Expands to: cd Documents
$ ls /hom[TAB]         # Expands to: ls /home/
$ cat note[TAB]        # Expands to: cat notes.txt
```

### Multiple Matches

If multiple files match your prefix, press Tab twice to see all options:

```bash
$ cd Do[TAB][TAB]
Documents/  Downloads/
```

### Tips for Tab Completion

1. **Always use it** - It prevents typos and saves keystrokes
2. **Press Tab twice** - If nothing happens, press again to see options
3. **Works with commands too** - Type `cha[TAB]` to get `chattr`, `chacl`, etc.
4. **Handles paths** - `~/Doc[TAB]` → `~/Documents/`

---

## Hidden Files and Directories

In Linux, files starting with a dot (`.`) are **hidden** by default. They're not shown in regular `ls` output.

```bash
$ ls
Documents  Downloads  Music

$ ls -a
.  ..  .bashrc  .config  .gitconfig  Documents  Downloads  Music
```

**Common hidden files:**

| File | Purpose |
|------|---------|
| `.bashrc` | Bash shell configuration |
| `.bash_history` | Command history |
| `.gitconfig` | Git configuration |
| `.ssh/` | SSH keys and config |
| `.config/` | Application settings |

---

## Practical Examples

### Example 1: Setting Up a Course Directory

```bash
# Start from home
cd ~

# Create course structure
mkdir -p linux-course/{session{01..10},exercises,notes}

# Navigate into session 01
cd linux-course/session01

# Create a notes file
touch notes.txt

# Go back to course root
cd ~/linux-course

# Verify structure
ls -R
```

### Example 2: Exploring System Directories

```bash
# Look at what's in /etc
cd /etc
ls

# Find SSH configuration
cd ssh
ls -la

# Return home quickly
cd ~
```

### Example 3: Cleaning Up Downloads

```bash
# Go to Downloads
cd ~/Downloads

# List files sorted by size (you'll learn this in Chapter 5)
ls -lhS

# Remove old files (be careful!)
rm old-file.zip

# Clean up empty directories
cd ..
rmdir Downloads/old-empty-folder
```

---

## Summary

In this chapter, you learned:

- **File System Structure**: Linux uses a single tree starting at `/`
- **Key Directories**: `/home` (user files), `/bin` (programs), `/etc` (config)
- **Navigation Commands**:
  - `pwd` - Where am I?
  - `ls` - What's here?
  - `cd` - Go somewhere
- **Paths**: Absolute (from `/`) vs relative (from current directory)
- **Special Symbols**: `.` (current), `..` (parent), `~` (home)
- **Directory Management**: `mkdir`, `rmdir`, `rm -r`
- **Tab Completion**: Always press Tab to save time and avoid typos

---

## Chapter Quiz

Test your understanding of the Linux file system and navigation:

{{#quiz ../../quizzes/chapter-04.toml}}

---

## Exercises

### Exercise 1: Directory Navigation

1. Open your terminal
2. Navigate to `/etc`
3. List the contents
4. Go to `/var/log`
5. Return to your home directory using a single command
6. Verify you're home with `pwd`

### Exercise 2: Create Course Structure

Create this directory structure in your home folder:

```
~/linux-course/
├── session01/
├── session02/
├── session03/
└── exercises/
    ├── basic/
    └── advanced/
```

### Exercise 3: File Creation Practice

1. Navigate to `~/linux-course/session01`
2. Create an empty file called `notes.txt`
3. Go back to `~/linux-course`
4. Use tab completion to navigate to `session01/notes.txt`

### Exercise 4: Path Practice

Starting from `/home/student`, use relative paths to:

1. Go to `/home/student/Documents`
2. From there, go to `/home/student/Pictures` using only relative paths
3. Return to `/home/student` using the shortest command

### Exercise 5: Exploration Challenge

1. Navigate to `/usr/bin`
2. Count how many executables are there (hint: `ls | wc -l`)
3. Find if `python3` exists there (hint: `ls python*`)
4. Explore what's in `/tmp`

---

## Expected Output

### Exercise 1 Solution

```bash
$ cd /etc
$ ls
aiccu          cups           ld.so.cache   nsswitch.conf  udev
alias          dbus           libao.conf    opt            udisks2
...

$ cd /var/log
$ pwd
/var/log

$ cd ~
$ pwd
/home/student
```

### Exercise 2 Solution

```bash
$ mkdir -p ~/linux-course/session{01..03}
$ mkdir -p ~/linux-course/exercises/{basic,advanced}
$ cd ~/linux-course
$ tree -L 2    # or use: find . -type d -maxdepth 2
.
├── exercises
│   ├── advanced
│   └── basic
├── session01
├── session02
└── session03
```

### Exercise 3 Solution

```bash
$ cd ~/linux-course/session01
$ touch notes.txt
$ cd ~/linux-course
$ cd ses[TAB]    # Expands to session01
$ ls
notes.txt
```

### Exercise 4 Solution

```bash
$ cd ~/linux-course/session01
$ cd ../../Pictures      # or: cd ../session02
$ cd ~                   # or: cd /home/student
```

---

## Next Chapter

In Chapter 5, you'll learn **CLI Fundamentals** - viewing files, copying and moving, pipes, redirection, and wildcards. This will build on your navigation skills to make you truly productive in the terminal.
