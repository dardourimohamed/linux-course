# Chapter 6: Text Processing

## Learning Objectives

By the end of this chapter, you will be able to:

- [x] Search text patterns using `grep`
- [x] Find files by name, size, or type with `find`
- [x] Process text with `sed` for search and replace
- [x] Use `awk` for text extraction and formatting
- [x] Sort, count, and deduplicate with `sort`, `uniq`, and `wc`
- [x] Build powerful text processing pipelines

## Prerequisites

- Completed Chapter 5: CLI Fundamentals
- Comfortable with pipes and redirection
- Understanding of wildcards and basic regex

---

## Searching with Grep

`grep` is one of the most powerful and frequently used commands in Linux. It searches for patterns in files.

### Basic Usage

```bash
grep pattern file.txt            # Search for pattern in file
grep "hello world" file.txt      # Search for phrase
grep -i pattern file.txt         # Case-insensitive search
grep -r "TODO" .                 # Recursive search in current directory
grep -v pattern file.txt         # Invert match (show non-matching lines)
```

### Common Options

| Option | Purpose | Example |
|--------|---------|---------|
| `-i` | Ignore case | `grep -i error log.txt` |
| `-r` | Recursive | `grep -r "function" src/` |
| `-n` | Show line numbers | `grep -n TODO *.py` |
| `-c` | Count matches | `grep -c "error" log.txt` |
| `-v` | Invert match | `grep -v "#" config.txt` |
| `-l` | List matching files | `grep -l "import" *.py` |
| `-w` | Whole word only | `grep -w "cat" file.txt` |

### Practical Examples

```bash
# Find all TODO comments in code
grep -rn "TODO" src/

# Count errors in log file
grep -c "ERROR" /var/log/syslog

# Find IP addresses in logs
grep -E "[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}" access.log

# Show lines without comments
grep -v "^#" config.txt

# Find processes
ps aux | grep firefox
```

### Extended Regular Expressions

```bash
grep -E "error|warning" log.txt          # Match error OR warning
grep -E "^Start|End$" file.txt           # Match lines starting with Start or ending with End
grep -E "[0-9]+" file.txt                # Match numbers
grep -E "\b[A-Za-z0-9._%+-]+@[A-Za-z0-9.-]+\.[A-Z|a-z]{2,}\b" emails.txt  # Email pattern
```

---

## Finding Files with Find

`find` searches for files and directories based on various criteria.

### Basic Syntax

```bash
find [path] [options] [expression]
```

### Common Usage Patterns

```bash
# Find by name
find . -name "file.txt"
find . -name "*.jpg"
find / -name "config.yml" 2>/dev/null

# Find by type (f=file, d=directory, l=symlink)
find . -type f
find . -type d
find . -type l

# Find by size
find . -size +10M                  # Larger than 10MB
find . -size -1k                   # Smaller than 1KB
find . -size 100M                  # Exactly 100MB

# Find by modification time
find . -mtime -7                   # Modified in last 7 days
find . -mtime +30                  # Modified more than 30 days ago
find . -mmin -60                   # Modified in last 60 minutes

# Find by permissions
find . -perm 777                   # Exactly 777 permissions
find . -perm -u+x                  # Files with user execute
```

### Combining Find with Actions

```bash
# Delete files matching pattern (be careful!)
find . -name "*.tmp" -delete

# Execute command on each file
find . -name "*.jpg" -cp {} backup/   # Copy all .jpg files

# Find and list details
find . -name "*.py" -ls

# Find empty files
find . -empty

# Find owned by specific user
find /home -user student
```

### Powerful Find Pipelines

```bash
# Find large files and sort by size
find . -type f -size +10M -exec ls -lh {} \; | sort -k5 -h

# Find Python files modified in last 7 days
find . -name "*.py" -mtime -7

# Find and count file types
find . -type f -name "*.*" | sed 's/.*\.//' | sort | uniq -c | sort -rn
```

---

## Text Manipulation with Sed

`sed` (stream editor) is used for filtering and transforming text.

### Basic Substitution

```bash
sed 's/old/new/' file.txt          # Replace first occurrence per line
sed 's/old/new/g' file.txt         # Replace all occurrences (global)
sed -i 's/old/new/g' file.txt      # Edit file in-place

# Remove empty lines
sed '/^$/d' file.txt

# Remove comments
sed 's/#.*$//' file.txt

# Replace spaces with tabs
sed 's/ /\t/g' file.txt
```

### Common Sed Operations

| Operation | Command | Description |
|-----------|---------|-------------|
| Delete lines | `sed '5d' file.txt` | Delete line 5 |
| Delete range | `sed '5,10d' file.txt` | Delete lines 5-10 |
| Print specific | `sed -n '5p' file.txt` | Print only line 5 |
| Delete pattern | `sed '/pattern/d' file.txt` | Delete matching lines |
| Print pattern | `sed -n '/pattern/p' file.txt` | Print only matching lines |

### Practical Examples

```bash
# Replace "foo" with "bar" in all .txt files
sed -i 's/foo/bar/g' *.txt

# Remove trailing whitespace
sed -i 's/[[:space:]]*$//' file.txt

# Convert Windows line endings to Unix
sed -i 's/\r$//' file.txt

# Add line numbers
sed = file.txt | sed 'N;s/\n/\t/'

# Extract text between delimiters
sed -n 's/.*<title>\(.*\)<\/title>.*/\1/p' file.html
```

---

## Text Processing with Awk

`awk` is a powerful text processing language. It's especially good at columnar data.

### Basic Structure

```bash
awk 'pattern { action }' file.txt
```

### Field Processing

```bash
# Print specific columns (space-separated by default)
awk '{print $1}' file.txt           # Print first column
awk '{print $1, $3}' file.txt       # Print first and third columns
awk '{print $NF}' file.txt          # Print last column

# Different delimiter
awk -F: '{print $1}' /etc/passwd    # : as delimiter (first field)
awk -F, '{print $2, $4}' file.csv   # CSV file
```

### Built-in Variables

| Variable | Meaning |
|----------|---------|
| `$0` | Entire line |
| `$1`, `$2`, ... | Fields 1, 2, ... |
| `$NF` | Number of fields (last field) |
| `NR` | Current record number (line number) |
| `FS` | Field separator (default: space) |
| `OFS` | Output field separator |

### Practical Examples

```bash
# Print lines longer than 80 characters
awk 'length($0) > 80' file.txt

# Sum values in column 3
awk '{sum += $3} END {print sum}' numbers.txt

# Calculate average
awk '{sum += $1; count++} END {print sum/count}' data.txt

# Filter and format
awk '$3 > 100 {print $1, "is high"}' data.txt

# Extract unique values from first column
awk '!seen[$1]++' file.txt

# Reorder columns (swap 1 and 2)
awk '{print $2, $1, $3}' file.txt
```

### Pattern Matching

```bash
# Print lines matching pattern
awk '/error/ {print}' log.txt

# Print if field matches
awk '$3 == "success" {print $1, $2}' data.txt

# Range pattern (print between START and END)
awk '/START/,/END/' file.txt

# Multiple conditions
awk '$1 > 50 && $2 < 100' file.txt
```

---

## Sorting and Counting

### sort - Arrange Lines

```bash
sort file.txt                     # Sort alphabetically
sort -n file.txt                  # Sort numerically
sort -r file.txt                  # Reverse sort
sort -u file.txt                  # Sort and remove duplicates
sort -k2 file.txt                 # Sort by 2nd column
sort -t: -k3 -n /etc/passwd       # Sort passwd by 3rd field (UID)

# Sort by human-readable sizes
sort -h sizes.txt

# Random shuffle
sort -R file.txt

# Sort and save
sort -o sorted.txt file.txt       # Same as: sort file.txt > sorted.txt
```

### uniq - Remove Duplicates

```bash
uniq file.txt                     # Remove adjacent duplicates
uniq -c file.txt                  # Count occurrences
uniq -d file.txt                  # Show only duplicates
uniq -u file.txt                  # Show only unique lines

# Common pattern: sort first, then uniq
sort file.txt | uniq              # Remove all duplicates
sort file.txt | uniq -c           # Count occurrences
sort file.txt | uniq -d           # Show only duplicated lines
```

### wc - Word Count

```bash
wc file.txt                       # Lines, words, bytes
wc -l file.txt                    # Line count only
wc -w file.txt                    # Word count only
wc -c file.txt                    # Byte count only
wc -m file.txt                    # Character count

# Count multiple files
wc -l *.txt                       # Count lines in all .txt files
ls | wc -l                        # Count files in directory
```

---

## Building Processing Pipelines

The real power comes from combining these tools.

### Log Analysis Pipeline

```bash
# Find top 5 IP addresses by request count
cat access.log | awk '{print $1}' | sort | uniq -c | sort -rn | head -n 5
```

Breakdown:
1. `cat access.log` - Read the log file
2. `awk '{print $1}'` - Extract first column (IP address)
3. `sort` - Sort IPs to group duplicates
4. `uniq -c` - Count occurrences
5. `sort -rn` - Sort by count, descending, numerically
6. `head -n 5` - Show top 5

### Disk Usage Analysis

```bash
# Find largest directories
du -h | sort -rh | head -n 10
```

### File Type Statistics

```bash
# Count file extensions
find . -type f | sed 's/.*\.//' | sort | uniq -c | sort -rn
```

### Process Monitoring

```bash
# Find top CPU users
ps aux | sort -rk 3 | head -n 10

# Find specific process and kill it
ps aux | grep chrome | awk '{print $2}' | xargs kill
```

### Code Analysis

```bash
# Count lines of code (excluding comments and blanks)
find src -name "*.py" -exec cat {} \; | grep -v "^#" | grep -v "^$" | wc -l

# Find all TODO comments
grep -rn "TODO" src/ | wc -l

# Find files containing specific functions
grep -rl "def my_function" src/
```

---

## Practical Examples

### Example 1: Clean Up Logs

```bash
# Extract only errors from today's logs
grep "ERROR" /var/log/syslog | grep "$(date +%Y-%m-%d)" > today-errors.txt

# Count errors by type
grep "ERROR" /var/log/syslog | awk '{print $5}' | sort | uniq -c
```

### Example 2: Email Processing

```bash
# Extract email addresses from text
grep -E "\b[A-Za-z0-9._%+-]+@[A-Za-z0-9.-]+\.[A-Z|a-z]{2,}\b" contacts.txt

# Count unique domains
grep -E "@[A-Za-z0-9.-]+\.[A-Z|a-z]{2,}\b" contacts.txt | \
  sed 's/.*@//' | sort | uniq -c
```

### Example 3: CSV Processing

```bash
# Calculate sum of column 3 in CSV
awk -F, '{sum += $3} END {print "Total:", sum}' data.csv

# Filter rows where column 2 > 100
awk -F, '$2 > 100' data.csv

# Extract specific columns and create new CSV
awk -F, '{print $1, $3, $5}' OFS=, input.csv > output.csv
```

### Example 4: File Management

```bash
# Find and delete old temporary files
find /tmp -name "*.tmp" -mtime +7 -delete

# Find large log files and compress them
find /var/log -name "*.log" -size +100M -exec gzip {} \;

# Rename files (replace spaces with underscores)
find . -name "* *" -type f | rename 's/ /_/g'
```

---

## Summary

In this chapter, you learned:

- **grep**: Search for patterns in files with powerful regex support
- **find**: Locate files by name, size, type, or time
- **sed**: Stream editor for search and replace operations
- **awk**: Text processing language for columnar data
- **sort**: Sort lines alphabetically or numerically
- **uniq**: Remove or count duplicate lines
- **wc**: Count lines, words, and characters
- **Pipelines**: Combine tools for powerful text processing

### Quick Reference

| Task | Command |
|------|---------|
| Search pattern | `grep "pattern" file.txt` |
| Find file by name | `find . -name "file.txt"` |
| Replace text | `sed 's/old/new/g' file.txt` |
| Extract column | `awk '{print $1}' file.txt` |
| Count lines | `wc -l file.txt` |
| Sort file | `sort file.txt` |
| Remove duplicates | `sort file.txt \| uniq` |

---

## Chapter Quiz

Test your understanding of text processing commands:

{{#quiz ../../quizzes/chapter-06.toml}}

---

## Exercises

### Exercise 1: Grep Practice

Create a sample file and search:
```bash
cat > sample.txt << EOF
Hello World
Linux is great
ERROR: Something went wrong
WARNING: Low disk space
ERROR: Connection failed
Success: All good
EOF
```

1. Find all lines containing "ERROR"
2. Count how many lines have "ERROR"
3. Find lines with "ERROR" or "WARNING"
4. Show line numbers with matches

### Exercise 2: Find and Process

1. Find all `.txt` files in your home directory
2. Find all files larger than 1MB in `/var/log`
3. Find all files modified in the last 24 hours
4. Count how many `.conf` files exist in `/etc`

### Exercise 3: Awk Processing

Create a data file:
```bash
cat > data.csv << EOF
name,age,score
Alice,25,95
Bob,30,87
Charlie,25,92
Diana,35,88
EOF
```

1. Extract the name column (column 1)
2. Calculate the average score
3. Find rows where age is 25
4. Format as: "Name: Alice, Score: 95"

### Exercise 4: Pipeline Building

1. Create a list of random numbers: `for i in {1..20}; do echo $RANDOM; done > numbers.txt`
2. Find the 5 largest numbers
3. Count how many are greater than 10000
4. Sort them and remove duplicates

### Exercise 5: Log Analysis Challenge

Simulate a web log:
```bash
cat > access.log << EOF
192.168.1.1 - - [01/Jan/2025] "GET /index.html" 200
192.168.1.2 - - [01/Jan/2025] "GET /about.html" 200
192.168.1.1 - - [01/Jan/2025] "GET /index.html" 200
192.168.1.3 - - [01/Jan/2025] "GET /contact.html" 404
192.168.1.2 - - [01/Jan/2025] "POST /submit" 500
EOF
```

1. Count how many 404 errors occurred
2. Find the IP address that made the most requests
3. Extract all unique pages visited
4. Show only failed requests (status code >= 400)

---

## Expected Output

### Exercise 1 Solution

```bash
$ grep "ERROR" sample.txt
ERROR: Something went wrong
ERROR: Connection failed

$ grep -c "ERROR" sample.txt
2

$ grep -E "ERROR|WARNING" sample.txt
ERROR: Something went wrong
WARNING: Low disk space
ERROR: Connection failed

$ grep -n "ERROR" sample.txt
3:ERROR: Something went wrong
5:ERROR: Connection failed
```

### Exercise 2 Solution

```bash
$ find ~ -name "*.txt"
/home/student/notes.txt
/home/student/sample.txt

$ find /var/log -size +1M
/var/log/journal/... (large journal files)

$ find ~ -mtime -1
/home/student/.bash_history
/home/student/sample.txt

$ find /etc -name "*.conf" | wc -l
42
```

### Exercise 3 Solution

```bash
$ awk -F, '{print $1}' data.csv
name
Alice
Bob
Charlie
Diana

$ awk -F, 'NR>1 {sum += $3; count++} END {print sum/count}' data.csv
90.5

$ awk -F, '$2 == 25' data.csv
Alice,25,95
Charlie,25,92

$ awk -F, 'NR>1 {print "Name:", $1, ", Score:", $3}' data.csv
Name: Alice , Score: 95
Name: Bob , Score: 87
Name: Charlie , Score: 92
Name: Diana , Score: 88
```

### Exercise 4 Solution

```bash
$ for i in {1..20}; do echo $RANDOM; done > numbers.txt

$ sort -rn numbers.txt | head -n 5
32145
29834
28745
27653
26847

$ awk '$1 > 10000' numbers.txt | wc -l
8

$ sort numbers.txt | uniq
1024
5678
... (unique sorted numbers)
```

### Exercise 5 Solution

```bash
$ grep " 404 " access.log | wc -l
1

$ awk '{print $1}' access.log | sort | uniq -c | sort -rn | head -n 1
2 192.168.1.1

$ awk -F'"' '{print $2}' access.log | sort -u
GET /about.html
GET /contact.html
GET /index.html
POST /submit

$ awk '$9 >= 400' access.log
192.168.1.3 - - [01/Jan/2025] "GET /contact.html" 404
192.168.1.2 - - [01/Jan/2025] "POST /submit" 500
```

---

## Next Chapter

In Chapter 7, you'll learn **Permissions & Users** - understanding Linux security, managing permissions with `chmod` and `chown`, and user administration with `sudo` and user management commands.
