# Chapter 4 Solutions: File System & Navigation

## Exercise Solutions

### Exercise 1: Navigate Directories

**Task:** Practice moving through the file system.

**Solution:**

```bash
# Start at home
cd ~
pwd
```

**Expected Output:**
```
/home/yourusername
```

```bash
# Go to root
cd /
pwd
ls
```

**Expected Output:**
```
/
bin  boot  dev  etc  home  lib  media  mnt  opt  proc  root  run  sbin  srv  sys  tmp  usr  var
```

```bash
# Navigate using relative paths
cd home
cd yourusername
pwd
```

**Expected Output:**
```
/home/yourusername
```

```bash
# Navigate using absolute path
cd /etc
pwd

# Jump back to previous directory
cd -
pwd
```

**Expected Output:**
```
/etc
/home/yourusername
```

```bash
# Go to parent directory twice
cd ../..
pwd
```

**Explanation:**
- `cd ~` or just `cd` takes you to your home directory
- `cd /` goes to root (top of directory tree)
- `cd -` toggles between previous and current directory
- `..` is parent directory, can chain: `../../..` goes up three levels
- Always use `pwd` to confirm your location

---

### Exercise 2: List Files

**Task:** Use ls to explore different directories.

**Solution:**

```bash
# Basic listing
ls ~
```

**Expected Output:**
```
Desktop  Documents  Downloads  Music  Pictures  Public  Templates  Videos
```

```bash
# Detailed listing
ls -l ~
```

**Expected Output:**
```
total 32
drwxr-xr-x 2 user user 4096 Feb  7 10:00 Desktop
drwxr-xr-x 2 user user 4096 Feb  5 09:12 Documents
drwxr-xr-x 2 user user 4096 Feb  7 08:45 Downloads
```

```bash
# Show all files including hidden
ls -la ~
```

**Expected Output:**
```
total 64
drwxr-xr-x 20 user user 4096 Feb  7 10:00 .
drwxr-xr-x  3 root root 4096 Feb  1 00:00 ..
-rw-------  1 user user  220 Feb  1 00:00 .bash_logout
-rw-r--r--  1 user user 3576 Feb  1 00:00 .bashrc
drwx------  3 user user  4096 Feb  6 15:30 .config
-rw-r--r--  1 user user  220 Feb  1 00:00 .profile
```

```bash
# Human-readable sizes
ls -lh /var/log
```

**Expected Output:**
```
total 1.2M
drwxr-xr-x  2 root root 4.0K Feb  7 00:00 apt
-rw-r-----  1 root adm  150K Feb  7 08:45 syslog
-rw-r-----  1 root adm  250K Feb  7 00:00 kern.log
```

```bash
# List by modification time (newest first)
ls -lt ~/Downloads | head -10
```

```bash
# List by size (largest first)
ls -lS ~/Downloads
```

**Explanation:**
- `-l`: Long format (permissions, owner, size, date)
- `-a`: All files (including hidden starting with `.`)
- `-h`: Human-readable sizes (K, M, G)
- `-t`: Sort by modification time
- `-S`: Sort by size
- `-r`: Reverse sort order

---

### Exercise 3: Create and Remove Directories

**Task:** Practice creating and deleting directories.

**Solution:**

```bash
# Create a single directory
mkdir test_dir
ls -ld test_dir
```

**Expected Output:**
```
drwxr-xr-x 2 user user 4096 Feb  7 10:30 test_dir
```

```bash
# Create nested directories (with parents)
mkdir -p projects/linux-course/chapter04
tree projects
```

**Expected Output:**
```
projects/
└── linux-course/
    └── chapter04/
```

```bash
# Create multiple directories at once
mkdir dir1 dir2 dir3
ls -ld dir*
```

**Expected Output:**
```
drwxr-xr-x 2 user user 4096 Feb  7 10:35 dir1
drwxr-xr-x 2 user user 4096 Feb  7 10:35 dir2
drwxr-xr-x 2 user user 4096 Feb  7 10:35 dir3
```

```bash
# Remove empty directories
rmdir dir1 dir2 dir3
```

```bash
# Remove directories with contents
rm -rf test_dir projects
```

**Expected Output:**
```
(No output on success)
```

```bash
# Verify deletion
ls -ld test_dir projects 2>/dev/null || echo "Directories deleted successfully"
```

**Expected Output:**
```
ls: cannot access 'test_dir': No such file or directory
ls: cannot access 'projects': No such file or directory
Directories deleted successfully
```

**Explanation:**
- `mkdir`: Create directory
- `-p`: Create parent directories as needed (no error if exists)
- `rmdir`: Remove empty directory only (safe)
- `rm -rf`: Remove directory and all contents (force, recursive - dangerous!)
- Always verify `rm -rf` targets before pressing Enter

---

### Exercise 4: Understanding Paths

**Task:** Practice with absolute and relative paths.

**Solution:**

```bash
# Create test structure
mkdir -p test_project/{src,docs,tests}
cd test_project
pwd
```

**Expected Output:**
```
/home/yourusername/test_project
```

```bash
# Relative paths (from current directory)
cd src
pwd
```

**Expected Output:**
```
/home/yourusername/test_project/src
```

```bash
# Go to sibling directory using relative path
cd ../docs
pwd
```

**Expected Output:**
```
/home/yourusername/test_project/docs
```

```bash
# Go to tests using absolute path
cd /home/yourusername/test_project/tests
pwd
```

**Expected Output:**
```
/home/yourusername/test_project/tests
```

```bash
# Use tilde for home directory
cd ~/test_project/src
pwd
```

**Expected Output:**
```
/home/yourusername/test_project/src
```

```bash
# Go up two levels
cd ../..
pwd
```

**Expected Output:**
```
/home/yourusername
```

```bash
# Clean up
rm -rf test_project
```

**Explanation:**
- **Absolute path**: Starts with `/`, specifies complete path from root
- **Relative path**: Doesn't start with `/`, relative to current directory
- **Tilde (`~`)**: Shortcut for home directory
- **Dot (`.`)**: Current directory
- **Dot-dot (`..`)**: Parent directory

---

### Exercise 5: Tab Completion

**Task:** Use tab completion to navigate faster.

**Solution:**

```bash
# Start typing directory name, press Tab
cd Do<Tab>
```

**Result:** Completes to `cd Downloads/`

```bash
# If multiple matches, press Tab twice
cd D<Tab><Tab>
```

**Expected Output:**
```
Desktop/  Documents/  Downloads/
```

```bash
# Type more characters to disambiguate
cd Do<Tab>  # completes to cd Downloads/
cd De<Tab>  # shows Desktop/ and Documents/
cd Des<Tab> # completes to cd Desktop/
```

```bash
# Tab completion also works with commands
fi<Tab>      # completes to 'file'
gre<Tab>     # completes to 'grep'
systemc<Tab> # completes to 'systemctl'
```

**Explanation:**
- Tab completion saves typing and prevents errors
- One Tab: completes if unique
- Two Tabs: shows all possible completions
- Works for commands, files, directories, command options
- Use it constantly - it's a huge productivity booster

---

## File System Exploration Exercise

### Explore Important Directories

```bash
# /etc - Configuration files
ls /etc | head -20
```

**Expected Output:**
```
fstab        group        hostname     hosts        issue
networks     passwd       profile      resolv.conf  systemd
```

```bash
# /usr - User programs
ls /usr/bin | wc -l    # Count installed programs
```

**Expected Output:**
```
2345    # (approximately)
```

```bash
# /var - Variable data
ls /var
```

**Expected Output:**
```
backups  cache  lib  local  lock  log  mail  opt  run  spool  tmp  www
```

```bash
# View your own .bashrc (hidden configuration file)
cat ~/.bashrc | head -20
```

**Expected Output:**
```
# ~/.bashrc: executed by bash(1) for non-login shells.
# see /usr/share/doc/bash/examples/startup-files (in the package bash-doc)

# If not running interactively, don't do anything
case $- in
    *i*) ;;
      *) return;;
esac
```

---

## Instructor Notes

### Teaching Tips

1. **Visual Aids**: Draw directory trees on board to visualize structure
2. **Live Navigation**: Navigate through directories during lecture explaining each step
3. **Tab Completion**: Emphasize constantly - have students practice until it's muscle memory
4. **Safety First**: Show `rm -rf` on test directories only, explain dangers
5. **Path Practice**: Create exercises that require using both relative and absolute paths

### Common Student Mistakes

| Mistake | Why It's Wrong | Correct Approach |
|---------|----------------|------------------|
| `cd /home` | Goes to wrong home directory | Use `cd ~` or `cd` to go to YOUR home |
| Not using tab completion | Wastes time, causes errors | Always use tab completion |
| `rm -rf` without checking | Can delete important files | Always verify path before running |
| Confusing `.` and `..` | Single dot = current, double dot = parent | Practice with `cd .` and `cd ..` |
| Assuming Windows paths | Linux uses `/` not `\` | Use `/home/user/Documents` |

### Extension Activities

- Create a file system scavenger hunt
- Use `tree` command to visualize directory structure
- Compare `ls` output sorted by time vs size
- Explore `/proc` and `/sys` - what weird things exist there?
- Research why `/usr` and `/var` are separate from root
- Set up `alias ll='ls -lah'` for convenience
