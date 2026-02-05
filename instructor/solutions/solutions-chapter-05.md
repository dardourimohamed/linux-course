# Chapter 5 Solutions: CLI Fundamentals

## Exercise Solutions

### Exercise 1: File Content Commands

**Task:** Practice cat, less, head, and tail.

**Solution:**

```bash
# Create a test file with multiple lines
seq 1 100 > numbers.txt

# View entire file with cat
cat numbers.txt
```

**Expected Output:**
```
1
2
3
...
100
```

```bash
# View first 10 lines with head
head numbers.txt
```

**Expected Output:**
```
1
2
3
4
5
6
7
8
9
10
```

```bash
# View last 10 lines with tail
tail numbers.txt
```

**Expected Output:**
```
91
92
93
94
95
96
97
98
99
100
```

```bash
# View first 20 lines
head -n 20 numbers.txt
```

```bash
# View last 5 lines
tail -n 5 numbers.txt
```

```bash
# Use less for large files (scrollable)
less numbers.txt
# Navigation: Space (page down), b (page up), q (quit)
```

**Explanation:**
- `cat`: Display entire file at once (good for small files)
- `head`: Show beginning (default 10 lines)
- `tail`: Show end (default 10 lines)
- `less`: Interactive pager for large files (scrollable)

---

### Exercise 2: Output Redirection

**Task:** Save command output to files.

**Solution:**

```bash
# Save output to file (overwrites)
ls ~ > filelist.txt
cat filelist.txt
```

**Expected Output:**
```
Desktop
Documents
Downloads
...
```

```bash
# Append output to file
echo "End of list" >> filelist.txt
tail filelist.txt
```

**Expected Output:**
```
...
End of list
```

```bash
# Count lines and save to file
wc -l filelist.txt > linecount.txt
cat linecount.txt
```

**Expected Output:**
```
15 filelist.txt
```

```bash
# Save both stdout and stderr
ls /root /nonexistent > output.txt 2>&1
cat output.txt
```

**Expected Output:**
```
ls: cannot access '/root': Permission denied
ls: cannot access '/nonexistent': No such file or directory
```

**Explanation:**
- `>`: Redirect stdout, overwrites existing file
- `>>`: Redirect stdout, appends to existing file
- `2>`: Redirect stderr (file descriptor 2)
- `2>&1`: Redirect stderr to stdout
- `&>`: Redirect both stdout and stderr (bash)

---

### Exercise 3: Pipes

**Task:** Chain commands with pipes.

**Solution:**

```bash
# Count files in current directory
ls | wc -l
```

**Expected Output:**
```
25
```

```bash
# Find largest files
ls -lS | head -5
```

**Expected Output:**
```
-rw-r--r-- 1 user user 1024000 Feb  7 10:00 largefile.bin
-rw-r--r-- 1 user user 512000 Feb  6 15:30 video.mp4
...
```

```bash
# Search for specific process
ps aux | grep bash
```

**Expected Output:**
```
user      1234  0.0  0.1  12548  9524 pts/0  Ss   10:00   0:00 -bash
user      5678  0.0  0.0   7824   892 pts/0  S+   10:30   0:00 grep bash
```

```bash
# Chain multiple commands
seq 1 100 | grep 5 | sort -rn | head -5
```

**Expected Output:**
```
59
58
57
56
55
```

```bash
# Real-world example: Find text files sorted by size
find . -name "*.txt" -exec ls -lh {} \; | sort -k5 -h
```

**Explanation:**
- `|`: Pass stdout of left command as stdin to right command
- Each command processes output of previous
- Enables powerful data transformation pipelines
- Each command does one thing well (Unix philosophy)

---

### Exercise 4: Wildcards

**Task:** Use wildcards to match multiple files.

**Solution:**

```bash
# Create test files
touch file1.txt file2.txt file3.txt file1.log file2.log image.jpg

# List all .txt files
ls *.txt
```

**Expected Output:**
```
file1.txt  file2.txt  file3.txt
```

```bash
# List all files starting with "file"
ls file*
```

**Expected Output:**
```
file1.txt  file1.log  file2.txt  file2.log  file3.txt
```

```bash
# List all files with single character before extension
ls file?.txt
```

**Expected Output:**
```
file1.txt  file2.txt  file3.txt
```

```bash
# List all files ending in .txt or .log
ls *.{txt,log}
```

**Expected Output:**
```
file1.txt  file1.log  file2.txt  file2.log  file3.txt
```

```bash
# Match specific characters
ls file[123].txt
```

**Expected Output:**
```
file1.txt  file2.txt  file3.txt
```

```bash
# Clean up
rm file*.txt file*.log image.jpg
```

**Explanation:**
- `*`: Matches any number of any characters (including none)
- `?`: Matches exactly one character
- `[abc]`: Matches exactly one character from set
- `{a,b,c}`: Brace expansion - expands to multiple items
- Wildcards work with any command that accepts file arguments

---

### Exercise 5: Command Combination

**Task:** Use operators to combine commands.

**Solution:**

```bash
# Sequential execution (;)
mkdir testdir; cd testdir; pwd
```

**Expected Output:**
```
/home/yourusername/testdir
```

```bash
# Conditional execution (&&)
mkdir newdir && echo "Directory created successfully"
```

**Expected Output:**
```
Directory created successfully
```

```bash
# If first command fails, second doesn't run
mkdir /root/testdir && echo "This won't run"
```

**Expected Output:**
```
mkdir: cannot create directory '/root/testdir': Permission denied
```

```bash
# OR operator (||)
grep "pattern" file.txt || echo "Pattern not found"
```

```bash
# Combining operators
cd testdir && ls || echo "Directory doesn't exist"
```

```bash
# Cleanup
cd .. && rm -rf testdir newdir
```

**Explanation:**
- `;`: Run commands sequentially (regardless of success/failure)
- `&&`: Run next command only if previous succeeded (exit code 0)
- `||`: Run next command only if previous failed (non-zero exit code)
- Useful for error handling and conditional operations

---

## Advanced Pipeline Examples

### Log Analysis

```bash
# Find errors in system logs
grep -i error /var/log/syslog | tail -20
```

```bash
# Count occurrences of each error type
grep -i error /var/log/syslog | awk '{print $5}' | sort | uniq -c | sort -rn
```

### System Monitoring

```bash
# Find top 5 CPU users
ps aux | sort -rk 3 | head -6
```

**Expected Output:**
```
USER       PID %CPU %MEM
user      1234 15.3  2.1
user      5678 10.2  1.5
root        45  5.1  0.3
```

```bash
# Find large files
find ~ -type f -size +100M 2>/dev/null | xargs ls -lh | sort -k5 -h
```

### Data Processing

```bash
# Extract specific fields
cat /etc/passwd | cut -d: -f1,7 | head -10
```

**Expected Output:**
```
root:/bin/bash
daemon:/usr/sbin/nologin
bin:/usr/sbin/nologin
```

```bash
# Process CSV-like data
echo -e "name,age\nAlice,30\nBob,25" | column -t -s,
```

**Expected Output:**
```
name  age
Alice 30
Bob   25
```

---

## Instructor Notes

### Teaching Tips

1. **Build pipelines incrementally**: Start with one command, add pipes one at a time
2. **Real-world examples**: Use actual system data (logs, processes) for demonstrations
3. **Compare operators**: Show && vs ; vs || with failing commands to clarify differences
4. **Wildcards practice**: Create many files with patterns for hands-on practice
5. **Redirection visualization**: Draw data flow diagrams showing > vs |

### Common Student Issues

| Issue | Explanation | Fix |
|-------|-------------|-----|
| Pipe "doesn't work" | Command expecting file, not stdin | Use `-` for stdin: `command -` |
| >> vs > confusion | Using > when intending to append | Use >> for append |
| Wildcard matches too much | `*.log` matches `logfile.txtlog` | Be specific: `*.log` or use find |
| && command doesn't run | Previous command failed silently | Check exit code of first command |
| Overwritten important file | Used > instead of >> | Make backups, use >> first |

### Extension Activities

- Pipeline puzzle: Give output, have students build the pipeline
- Create a log analyzer one-liner for specific patterns
- Compare different wildcards with the same files
- Build a system monitoring dashboard with pipes
- Research and test advanced text processing tools (awk, sed)
- Create command aliases for complex pipelines
