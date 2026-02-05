# Chapter 6 Solutions: Text Processing

## Exercise Solutions

### Exercise 1: Search with grep

**Task:** Find patterns in files using grep.

**Solution:**

```bash
# Create test file
cat > testfile.txt << EOF
Hello World
Linux is awesome
Hello Linux
World of Linux
EOF

# Search for "Hello"
grep "Hello" testfile.txt
```

**Expected Output:**
```
Hello World
Hello Linux
```

```bash
# Case-insensitive search
grep -i "linux" testfile.txt
```

**Expected Output:**
```
Linux is awesome
Hello Linux
World of Linux
```

```bash
# Show line numbers
grep -n "Linux" testfile.txt
```

**Expected Output:**
```
2:Linux is awesome
3:Hello Linux
4:World of Linux
```

```bash
# Invert match (show lines NOT containing pattern)
grep -v "Hello" testfile.txt
```

**Expected Output:**
```
Linux is awesome
World of Linux
```

```bash
# Count matches
grep -c "Linux" testfile.txt
```

**Expected Output:**
```
3
```

```bash
# Search recursively in directories
grep -r "Hello" . 2>/dev/null
```

**Explanation:**
- `grep pattern file`: Search for pattern in file
- `-i`: Case-insensitive search
- `-n`: Show line numbers
- `-v`: Invert (show non-matching lines)
- `-c`: Count matching lines
- `-r`: Recursive search through directories

---

### Exercise 2: Find Files

**Task:** Use find to locate files.

**Solution:**

```bash
# Create test structure
mkdir -p testdir/{src,docs}
touch testdir/src/{main.py,utils.py} testdir/docs/{README.md,config.txt}

# Find all Python files
find testdir -name "*.py"
```

**Expected Output:**
```
testdir/src/main.py
testdir/src/utils.py
```

```bash
# Find directories only
find testdir -type d
```

**Expected Output:**
```
testdir
testdir/src
testdir/docs
```

```bash
# Find files modified in last 24 hours
find testdir -mtime -1
```

```bash
# Find files larger than 1K
find testdir -size +1k
```

```bash
# Find and execute command
find testdir -name "*.py" -exec ls -lh {} \;
```

**Expected Output:**
```
-rw-r--r-- 1 user user 0 Feb  7 10:30 testdir/src/main.py
-rw-r--r-- 1 user user 0 Feb  7 10:30 testdir/src/utils.py
```

```bash
# Delete found files (careful!)
find testdir -name "*.txt" -delete

# Cleanup
rm -rf testdir
```

**Explanation:**
- `find path -name pattern`: Find files by name pattern
- `-type d`: Find directories only
- `-type f`: Find files only
- `-mtime -n`: Modified less than n days ago
- `-size +n`: Larger than n (c=bytes, k=KB, M=MB)
- `-exec command {} \;`: Execute command on each found file

---

### Exercise 3: Stream Editing with sed

**Task:** Replace and transform text with sed.

**Solution:**

```bash
# Create test file
cat > cities.txt << EOF
New York
Los Angeles
Chicago
Houston
Phoenix
EOF

# Replace first occurrence of "New" with "Old"
sed 's/New/Old/' cities.txt
```

**Expected Output:**
```
Old York
Los Angeles
Chicago
Houston
Phoenix
```

```bash
# Replace all occurrences
sed 's/o/O/g' cities.txt
```

**Expected Output:**
```
New YOrk
LOs Angeles
ChicagO
HOustOn
PhOenix
```

```bash
# Delete lines matching pattern
sed '/Chicago/d' cities.txt
```

**Expected Output:**
```
New York
Los Angeles
Houston
Phoenix
```

```bash
# Edit file in-place
sed -i 's/New/Old/' cities.txt
cat cities.txt
```

**Expected Output:**
```
Old York
Los Angeles
Chicago
Houston
Phoenix
```

```bash
# Cleanup
rm cities.txt
```

**Explanation:**
- `sed 's/old/new/'`: Replace first occurrence
- `sed 's/old/new/g'`: Replace all occurrences (global)
- `sed '/pattern/d'`: Delete lines matching pattern
- `sed -i`: Edit file in-place (modifies original file)
- sed is powerful for text transformation pipelines

---

### Exercise 4: Text Processing with awk

**Task:** Process columnar data with awk.

**Solution:**

```bash
# Create sample data
cat > employees.txt << EOF
Alice Developer 50000
Bob Designer 45000
Charlie Manager 60000
Diana Developer 55000
EOF

# Print first field (name)
awk '{print $1}' employees.txt
```

**Expected Output:**
```
Alice
Bob
Charlie
Diana
```

```bash
# Print name and salary
awk '{print $1, $3}' employees.txt
```

**Expected Output:**
```
Alice 50000
Bob 45000
Charlie 60000
Diana 55000
```

```bash
# Calculate total salary
awk '{sum += $3} END {print "Total:", sum}' employees.txt
```

**Expected Output:**
```
Total: 210000
```

```bash
# Filter rows (developers only)
awk '/Developer/ {print $1, $3}' employees.txt
```

**Expected Output:**
```
Alice 50000
Diana 55000
```

```bash
# Format output
awk '{printf "%-10s $%d\n", $1, $3}' employees.txt
```

**Expected Output:**
```
Alice      $50000
Bob        $45000
Charlie    $60000
Diana      $55000
```

```bash
# Cleanup
rm employees.txt
```

**Explanation:**
- `awk '{print $N}'`: Print Nth field (whitespace-separated)
- `$1, $2, $3`: First, second, third fields
- `NR`: Record number (line number)
- `NF`: Number of fields
- `sum += $3`: Sum third field
- `END {action}`: Execute after processing all lines
- `printf`: Formatted output (like C)

---

### Exercise 5: Sorting and Unique

**Task:** Sort data and find unique entries.

**Solution:**

```bash
# Create unsorted file with duplicates
cat > unsorted.txt << EOF
banana
apple
cherry
apple
banana
date
elderberry
cherry
EOF

# Sort alphabetically
sort unsorted.txt
```

**Expected Output:**
```
apple
apple
banana
banana
cherry
cherry
date
elderberry
```

```bash
# Sort and remove duplicates
sort unsorted.txt | uniq
```

**Expected Output:**
```
apple
banana
cherry
date
elderberry
```

```bash
# Count occurrences
sort unsorted.txt | uniq -c
```

**Expected Output:**
```
2 apple
2 banana
2 cherry
1 date
1 elderberry
```

```bash
# Sort numerically
cat > numbers.txt << EOF
42
7
100
15
3
EOF

sort -n numbers.txt
```

**Expected Output:**
```
3
7
15
42
100
```

```bash
# Sort reverse numerically
sort -rn numbers.txt
```

**Expected Output:**
```
100
42
15
7
3
```

```bash
# Cleanup
rm unsorted.txt numbers.txt
```

**Explanation:**
- `sort`: Sort lines alphabetically
- `sort -n`: Sort numerically
- `sort -r`: Reverse sort
- `uniq`: Remove adjacent duplicate lines
- `uniq -c`: Count occurrences of each line
- **Important**: `uniq` only removes adjacent duplicates - sort first!

---

## Real-World Text Processing Examples

### Log File Analysis

```bash
# Create sample access log
cat > access.log << EOF
192.168.1.1 - - [07/Feb/2025:10:00:00] "GET /index.html" 200
192.168.1.2 - - [07/Feb/2025:10:00:05] "GET /about.html" 200
192.168.1.1 - - [07/Feb/2025:10:00:10] "POST /api/login" 401
192.168.1.3 - - [07/Feb/2025:10:00:15] "GET /contact.html" 200
192.168.1.1 - - [07/Feb/2025:10:00:20] "GET /dashboard" 200
EOF

# Count requests by IP
awk '{print $1}' access.log | sort | uniq -c | sort -rn
```

**Expected Output:**
```
3 192.168.1.1
1 192.168.1.2
1 192.168.1.3
```

```bash
# Find failed logins
grep "401" access.log
```

**Expected Output:**
```
192.168.1.1 - - [07/Feb/2025:10:00:10] "POST /api/login" 401
```

```bash
# Extract unique pages visited
awk '{print $7}' access.log | sort -u
```

**Expected Output:**
```
"GET
"POST
/about.html"
/contact.html"
/dashboard
/index.html"
```

```bash
# Cleanup
rm access.log
```

### System Information Processing

```bash
# Get system load and format output
uptime | awk -F'load average:' '{print $2}'
```

**Expected Output:**
```
 0.50, 0.45, 0.40
```

```bash
# Find top 5 processes by memory
ps aux | sort -rk 4 | head -6 | awk '{printf "%-10s %s\n", $11, $4}'
```

### File Analysis

```bash
# Count lines, words, characters in all .txt files
wc *.txt
```

```bash
# Find files modified in last 7 days and sort by date
find . -type f -mtime -7 -exec ls -lt {} + | head -10
```

---

## Instructor Notes

### Teaching Tips

1. **Start simple**: Begin with basic grep, then add options gradually
2. **Live demo with real logs**: Download a sample web server log for realistic examples
3. **Pipeline building**: Show how commands combine to solve complex problems
4. **Regex basics**: Introduce basic regex patterns for grep (. matches any char, * means zero or more)
5. **Efficiency emphasis**: Show how one-liners replace multi-line scripts

### Common Student Issues

| Issue | Solution |
|-------|----------|
| grep returns nothing | Check case sensitivity (`grep -i`), verify pattern |
| `uniq` not removing duplicates | Input must be sorted first |
| awk not splitting correctly | Default separator is whitespace, use `-F` for other separators |
| find permission errors | Use `2>/dev/null` to suppress permission denied errors |
| sed -i not working | Some systems use `sed -i.bak` for backup |

### Extension Activities

- Create a "log detective" challenge: Find specific patterns in sample logs
- Build a word frequency counter from a text file
- Process CSV data to extract and format specific columns
- Create a disk usage report with find, du, and sort
- Analyze system logs to find unusual patterns
- Compare performance of grep vs find for large file searches
