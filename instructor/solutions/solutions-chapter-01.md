# Chapter 1 Solutions: Introduction to Linux

## Exercise Solutions

### Exercise 1: Check Linux Version

**Task:** Determine which Linux distribution and version you're running.

**Solution:**

```bash
# Method 1: Check /etc/os-release (most reliable)
cat /etc/os-release
```

**Expected Output:**
```
NAME="Fedora Linux"
VERSION="39 (Workstation Edition)"
ID=fedora
PRETTY_NAME="Fedora Linux 39 (Workstation Edition)"
...
```

```bash
# Method 2: Use hostnamectl
hostnamectl
```

**Expected Output:**
```
Static hostname: fedora-pc
Icon name: computer-laptop
Chassis: portable
Operating System: Fedora Linux 39
Kernel: Linux 6.5.6-300.fc39.x86_64
Architecture: x86-64
```

```bash
# Method 3: For older systems
cat /etc/issue
lsb_release -a    # Ubuntu/Debian
```

**Explanation:**
- `/etc/os-release` is the modern standard and available on all current distributions
- `hostnamectl` provides detailed system information including OS, kernel, and hardware
- Different distributions may have different files (`/etc/redhat-release`, `/etc/debian_version`)

---

### Exercise 2: Open a Terminal

**Task:** Open the terminal and identify your shell.

**Solution:**

**Opening Terminal:**
- **GNOME**: Press Super (Windows key), type "Terminal", press Enter
- **Keyboard shortcut**: Ctrl+Alt+T (on most distributions)
- **Right-click**: Right-click on desktop → "Open in Terminal"

**Identify Shell:**

```bash
echo $SHELL
```

**Expected Output:**
```
/bin/bash
```

```bash
# Alternative method
ps -p $$
```

**Expected Output:**
```
PID TTY          TIME CMD
12345 pts/0    00:00:01 bash
```

**Explanation:**
- `$SHELL` environment variable stores your default shell path
- `ps -p $$` shows the current process (your shell)
- Most modern distributions use bash as the default shell

---

### Exercise 3: Explore the Terminal

**Task:** Navigate directories and use basic commands.

**Solution:**

```bash
# Navigate to home directory
cd ~
pwd
```

**Expected Output:**
```
/home/yourusername
```

```bash
# List files
ls -la
```

**Expected Output:**
```
total 32
drwx------  5 user user 4096 Feb  7 10:00 .
drwxr-xr-x  3 root root 4096 Feb  1 00:00 ..
drwxr-xr-x  2 user user 4096 Feb  6 15:30 Desktop
drwxr-xr-x  2 user user 4096 Feb  5 09:12 Documents
drwxr-xr-x  2 user user 4096 Feb  7 08:45 Downloads
```

```bash
# Create a directory
mkdir test_directory
cd test_directory
pwd
```

**Expected Output:**
```
/home/yourusername/test_directory
```

```bash
# Go back to home
cd ~
# or
cd ..
```

**Explanation:**
- `pwd` (print working directory) shows your current location
- `ls -la` lists all files including hidden ones with details
- `cd ~` navigates to home directory (same as just `cd`)
- `cd ..` goes up one directory level

---

### Exercise 4: Run Your First Command

**Task:** Run commands and understand output.

**Solution:**

```bash
# Display current date and time
date
```

**Expected Output:**
```
Wed Feb  7 10:30:45 CET 2025
```

```bash
# Display who is logged in
whoami
```

**Expected Output:**
```
yourusername
```

```bash
# Display calendar for current month
cal
```

**Expected Output:**
```
   February 2025
Su Mo Tu We Th Fr Sa
                   1
 2  3  4  5  6  7  8
 9 10 11 12 13 14 15
16 17 18 19 20 21 22
23 24 25 26 27 28
```

**Explanation:**
- `date` shows current system date and time
- `whoami` displays your username (equivalent to `id -un`)
- `cal` displays a formatted calendar

---

### Exercise 5: Get Help

**Task:** Use man pages and help flags.

**Solution:**

```bash
# Read the manual page for ls
man ls
```

**Navigation within man:**
- `Space` or `f`: Scroll down
- `b`: Scroll up
- `/pattern`: Search for pattern
- `q`: Quit

```bash
# Use --help flag
ls --help
```

**Expected Output:** (truncated)
```
Usage: ls [OPTION]... [FILE]...
List information about files.

Mandatory arguments to long options are mandatory for short options too.
  -a, --all                  do not ignore entries starting with .
  -l                         use a long listing format
...
```

```bash
# Get help on builtin commands (help is built-in)
help cd
```

**Expected Output:**
```
cd: cd [-L|[-P [-e]] [-@]] [dir]
    Change the shell working directory.
...
```

**Explanation:**
- `man` displays comprehensive manual pages for most commands
- `--help` flag shows quick usage information (works with GNU commands)
- `help` command provides help for shell built-ins (cd, pwd, etc.)
- Not all commands have man pages, and not all support --help

---

## Additional Practice Questions

**Q1: What command would you use to find out where a command is located on disk?**

**Answer:**
```bash
which ls
# Output: /usr/bin/ls

whereis ls
# Output: ls: /usr/bin/ls /usr/share/man/man1/ls.1.gz
```

**Q2: How do you clear the terminal screen?**

**Answer:**
```bash
clear
# or
reset   # more thorough reset
```

**Q3: What's the difference between `echo` and `printf`?**

**Answer:**
```bash
echo "Hello\nWorld"
# Output: Hello\nWorld (literal)

printf "Hello\nWorld"
# Output: Hello
#        World (actual newline)
```

`echo` is simpler but less portable; `printf` offers formatted output similar to C.

---

## Common Mistakes and Tips

| Mistake | Explanation | Fix |
|---------|-------------|-----|
| `CD directory` | Case-sensitive | Use lowercase `cd` |
| `cd ~/Documents ` | Trailing space in path | Use tab completion |
| Assuming Windows paths | Linux uses `/` not `\` | Use `/home/user/Documents` |
| `man` for everything | Some built-ins don't have man pages | Try `help command` instead |
| Forgetting quotes with spaces | `cd My Documents` fails | Use `cd "My Documents"` |

---

## Instructor Notes

### Teaching Tips

1. **Tab Completion**: Emphasize early and often - it prevents typos and saves time
2. **Case Sensitivity**: Unix is case-sensitive - this is a common stumbling block
3. **Absolute vs Relative Paths**: Demonstrate both, explain when each is useful
4. **Hidden Files**: Explain `.files` - students need to know about `.bashrc`, `.config`, etc.
5. **PATH Variable**: Briefly mention why some commands work without full path

### Common Student Questions

**Q: Why are there so many ways to do the same thing?**
A: Unix philosophy - multiple tools evolved over time. Different tools for different use cases.

**Q: Do I need to memorize all commands?**
A: No! Learn the basics, know how to find help, use cheatsheets, and build up over time.

**Q: Is bash the only shell?**
A: No, there are many (zsh, fish, tcsh), but bash is the most common and what we're learning.

### Extension Activities

- Compare `ls` output with and without `-a` flag
- Explore `/usr/bin` and see how many commands are available
- Try `echo *` and explain the result
- Use `history` to see previous commands
- Set up an alias: `alias ll='ls -la'`
