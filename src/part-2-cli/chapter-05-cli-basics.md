# Chapter 5: CLI Fundamentals

## Learning Objectives

By the end of this chapter, you will be able to:

- [x] View file contents using `cat`, `less`, `head`, and `tail`
- [x] Copy and move files with `cp` and `mv`
- [x] Use pipes (`|`) to chain commands together
- [x] Redirect input and output with `>`, `>>`, and `<`
- [x] Use wildcards (`*`, `?`, `[]`) to work with multiple files
- [x] Combine commands with `;`, `&&`, and `||`

## Prerequisites

- Completed Chapter 4: File System & Navigation
- Comfortable with basic directory navigation

---

## Command Syntax Basics

Every Linux command follows this basic structure:

```
command [options] [arguments]
```

| Component | Description | Example |
|-----------|-------------|---------|
| `command` | The program to run | `ls` |
| `options` | Modify behavior (usually start with `-`) | `-l`, `-a`, `-la` |
| `arguments` | What to operate on (files, directories) | `/etc`, `*.txt` |

```bash
ls -l /home          # ls=command, -l=option, /home=argument
grep -r "TODO" .     # grep=command, -r=option, "TODO"=arg1, .=arg2
```

### Getting Help

```bash
command --help       # Show help for most commands
man command          # Show manual page (press q to quit)
man ls               # Manual for ls command
```

---

## Viewing File Contents

### cat - Concatenate and Display

`cat` displays the entire contents of a file to the terminal.

```bash
cat file.txt                    # Display file
cat file1.txt file2.txt         # Display multiple files
cat -n file.txt                 # Show with line numbers
cat -b file.txt                 # Number non-blank lines
```

**When to use:** Small files, quickly viewing contents, combining files.

```bash
$ cat notes.txt
Welcome to Linux!
This is your second day.
Learn the terminal well.
```

### less - View with Pagination

`less` lets you scroll through large files.

```bash
less large-file.log             # View file with scrolling
less +F /var/log/syslog         # Follow file (like tail -f)
```

**Navigation in less:**

| Key | Action |
|-----|--------|
| `Space` or `f` | Page forward |
| `b` | Page backward |
| `Arrow keys` | Line by line |
| `/pattern` | Search forward |
| `n` | Next search result |
| `q` | Quit |

**When to use:** Large files, when you need to search, or want to scroll.

### head - Show First Lines

Display the beginning of a file.

```bash
head file.txt                   # First 10 lines (default)
head -n 20 file.txt             # First 20 lines
head -n 5 /var/log/syslog       # First 5 lines of syslog
```

### tail - Show Last Lines

Display the end of a file.

```bash
tail file.txt                   # Last 10 lines (default)
tail -n 20 file.txt             # Last 20 lines
tail -f /var/log/syslog         # Follow file in real-time
```

**The `-f` flag is incredibly useful** for watching logs as they're written!

### GUI vs CLI Comparison

| Task | GUI Action | CLI Command |
|------|------------|-------------|
| Open file | Double-click | `cat file.txt` |
| View large file | Scroll with mouse | `less large.txt` |
| Check beginning | Scroll to top | `head file.txt` |
| Check end | Scroll to bottom | `tail file.txt` |
| Watch log | Reopen file | `tail -f log.txt` |

---

## Copying and Moving Files

### cp - Copy Files and Directories

```bash
cp file.txt backup.txt          # Copy file
cp file.txt ~/Documents/        # Copy to directory
cp file1.txt file2.txt docs/    # Copy multiple files
cp -r directory/ new-dir/       # Copy directory recursively
cp -p file.txt preserve.txt     # Preserve permissions/timestamps
```

**Common options:**

| Option | Purpose |
|--------|---------|
| `-i` | Interactive (prompt before overwriting) |
| `-r` | Recursive (for directories) |
| `-p` | Preserve mode, ownership, timestamps |
| `-v` | Verbose (show what's being copied) |

```bash
$ cp -v notes.txt backup.txt
'notes.txt' -> 'backup.txt'
```

### mv - Move or Rename Files

```bash
mv old.txt new.txt              # Rename file
mv file.txt ~/Documents/        # Move file
mv directory/ ~/new-location/   # Move directory
mv *.txt docs/                  # Move all .txt files
```

**Important:** `mv` is both "move" and "rename" - they're the same operation in Linux!

---

## Pipes: Chaining Commands

The pipe (`|`) takes the **output** of one command and uses it as **input** to another.

### Basic Syntax

```
command1 | command2 | command3
```

### Practical Examples

```bash
# Long directory listing, piped to less for pagination
ls -l /etc | less

# Find large files
ls -lS | head -n 5              # Sort by size, show top 5

# Count files
ls | wc -l                      # Count number of files

# Search for process
ps aux | grep firefox           # Find firefox process

# View system info
neofetch                        # (if installed)
```

### Building Complex Pipelines

```bash
# Find all .txt files, count lines, show largest
find . -name "*.txt" -exec wc -l {} \; | sort -n | tail -n 5

# View Apache errors
cat /var/log/apache2/error.log | grep "ERROR" | less

# Count unique IP addresses in access log
cat access.log | awk '{print $1}' | sort | uniq
```

---

## Redirection: Controlling Input/Output

Redirection sends command output to files or reads input from files.

### Output Redirection

```bash
# Overwrite file (> or 1>)
echo "Hello" > file.txt         # Write to file (overwrites)
ls -l > directory-list.txt

# Append to file (>> or 1>>)
echo "World" >> file.txt        # Add to end of file
date >> log.txt                 # Append timestamp

# Redirect errors (2> or 2>>)
command 2> errors.txt           # Redirect error messages
command &> output.txt           # Redirect both output and errors
```

### Input Redirection

```bash
# Read from file (< or 0<)
wc -l < file.txt                # Count lines in file

# Here document (<<)
cat << EOF > config.txt
Host: localhost
Port: 8080
Debug: true
EOF
```

### Redirection vs Pipes

| Feature | Pipe `|` | Redirection `>` |
|---------|----------|----------------|
| Sends to | Another command | A file |
| Chaining | Yes (multiple) | No (single file) |
| Real-time | Yes | No |
| Example | `ls \| grep txt` | `ls > files.txt` |

---

## Wildcards: Pattern Matching

Wildcards let you work with multiple files matching a pattern.

### The Asterisk (`*`)

Matches **any number** of any characters.

```bash
*.txt                   # All .txt files
file*                   # All files starting with "file"
*backup*                # All files containing "backup"
*.tar.gz                # All .tar.gz files
```

### The Question Mark (`?`)

Matches **exactly one** character.

```bash
file?.txt               # file1.txt, fileA.txt (not file10.txt)
image?.jpg              # image1.jpg, image2.jpg
```

### Character Classes (`[]`)

Matches **one character** from the set.

```bash
file[123].txt           # file1.txt, file2.txt, file3.txt
`file[abc].txt`         # filea.txt, fileb.txt, filec.txt
[0-9]                   # Any single digit
[a-z]                   # Any lowercase letter
[A-Z]                   # Any uppercase letter
[a-zA-Z]                # Any letter
```

### Negation (`[!]` or `[^]`)

Matches anything **except** the specified characters.

```bash
file[!0-9].txt          # file.txt but not file1.txt, file2.txt, etc.
```

### Practical Wildcard Examples

```bash
# Copy all .jpg files to backup
cp *.jpg backup/

# Remove all temporary files
rm *.tmp

# List all hidden files
ls .*

# Find all log files
ls /var/log/*.log

# Work with numbered files
rm chapter-[0-5].md     # Remove chapters 0-5
```

---

## Combining Commands

### Sequential Execution (`;`)

Run commands regardless of previous success.

```bash
mkdir newdir; cd newdir; touch file.txt
```

### AND Operator (`&&`)

Run next command **only if** previous succeeded.

```bash
mkdir newdir && cd newdir && ls    # cd only if mkdir succeeded
./configure && make && sudo make install
```

### OR Operator (`||`)

Run next command **only if** previous failed.

```bash
mkdir newdir || echo "Directory already exists"
grep "pattern" file.txt || echo "Pattern not found"
```

### Combining AND and OR

```bash
# Try to start service, report success or failure
sudo systemctl start nginx && echo "Started!" || echo "Failed!"

# Create backup or report error
cp important.txt backup.txt || echo "Backup failed!"
```

---

## Practical Examples

### Example 1: Organize Downloads

```bash
cd ~/Downloads

# Create organized directories
mkdir -p images documents archives

# Move files by type
mv *.jpg *.png *.gif images/
mv *.pdf *.docx *.txt documents/
mv *.zip *.tar.gz archives/
```

### Example 2: Log Analysis

```bash
# Find all 404 errors in web log
grep " 404 " /var/log/nginx/access.log | less

# Count unique IPs
awk '{print $1}' /var/log/nginx/access.log | sort | uniq -c | sort -n

# Find errors from last 50 lines
tail -n 50 /var/log/syslog | grep -i error
```

### Example 3: Backup Strategy

```bash
# Create timestamped backup
DATE=$(date +%Y%m%d)
cp important.txt backup-$DATE.txt

# Archive and compress
tar -czf backup-$(date +%Y%m%d).tar.gz ~/Documents

# Verify backup was created
ls -lh backup-*.tar.gz
```

### Example 4: Process Management

```bash
# Find CPU-intensive processes
ps aux | sort -rk 3 | head -n 10

# Kill specific process
pkill firefox

# Or be more specific
kill $(pidof firefox)
```

---

## Summary

In this chapter, you learned:

- **Viewing Files**:
  - `cat` - Display entire file
  - `less` - Scroll through large files
  - `head` / `tail` - View start/end of files
- **File Operations**:
  - `cp` - Copy files/directories
  - `mv` - Move/rename files
- **Pipes** (`|`): Chain commands, pass output to input
- **Redirection** (`>`, `>>`, `<`): Control where input/output goes
- **Wildcards** (`*`, `?`, `[]`): Match multiple files
- **Command Combos** (`;`, `&&`, `||`): Control command flow

---

## Chapter Quiz

Test your understanding of CLI fundamentals:

{{#quiz ../../quizzes/chapter-05.toml}}

---

## Exercises

### Exercise 1: View Different Files

1. Create a test file: `echo -e "Line 1\nLine 2\nLine 3" > test.txt`
2. View it with `cat`
3. View the first line only
4. View the last line only
5. View it with `less`, then search for "Line 2"

### Exercise 2: Copy and Organize

1. Create these files: `touch file1.txt file2.txt file3.txt doc1.pdf doc2.pdf`
2. Create directories: `mkdir text docs`
3. Move all `.txt` files to `text/`
4. Copy all `.pdf` files to `docs/`
5. Verify with `ls`

### Exercise 3: Pipeline Practice

1. List all files in `/bin`
2. Pipe to `wc -l` to count them
3. Pipe to `grep zip` to find compression tools
4. Find the 5 largest files in `/usr/bin` (hint: `ls -lS | head`)

### Exercise 4: Redirection

1. Create a file with content: `echo "First line" > output.txt`
2. Add another line: `echo "Second line" >> output.txt`
3. View the result
4. Count lines: `wc -l < output.txt`

### Exercise 5: Wildcards

1. Create files: `touch file1.txt file2.txt fileA.txt file10.txt data.csv data.txt`
2. List all files starting with "file"
3. List all `.txt` files
4. List `file` followed by exactly one character
5. List `file` followed by a digit

---

## Expected Output

### Exercise 1 Solution

```bash
$ echo -e "Line 1\nLine 2\nLine 3" > test.txt
$ cat test.txt
Line 1
Line 2
Line 3

$ head -n 1 test.txt
Line 1

$ tail -n 1 test.txt
Line 3

$ less test.txt
# /Line 2 (press Enter, then 'q' to quit)
```

### Exercise 2 Solution

```bash
$ touch file1.txt file2.txt file3.txt doc1.pdf doc2.pdf
$ mkdir text docs
$ mv *.txt text/
$ cp *.pdf docs/
$ ls
text/  docs/

$ ls text/
file1.txt  file2.txt  file3.txt

$ ls docs/
doc1.pdf  doc2.pdf
```

### Exercise 3 Solution

```bash
$ ls /bin | wc -l
152

$ ls /bin | grep zip
gzip
gunzip
unzip
zip
zipcloak
zipdetails
zipgrep
zipinfo
zipsplit

$ ls -lS /usr/bin | head -n 5
```

### Exercise 4 Solution

```bash
$ echo "First line" > output.txt
$ echo "Second line" >> output.txt
$ cat output.txt
First line
Second line

$ wc -l < output.txt
2
```

### Exercise 5 Solution

```bash
$ touch file1.txt file2.txt fileA.txt file10.txt data.csv data.txt

$ ls file*
file1.txt  file10.txt  file2.txt  fileA.txt

$ ls *.txt
data.txt  file1.txt  file10.txt  file2.txt  fileA.txt

$ ls file?.txt
file1.txt  file2.txt  fileA.txt

$ ls file[0-9].txt
file1.txt  file2.txt
```

---

## Next Chapter

In Chapter 6, you'll learn **Text Processing** - searching with `grep`, finding files with `find`, and manipulating text with `sed`, `awk`, `sort`, and more.
