# Chapter 10 Solutions: Shell Scripting

## Exercise Solutions

### Exercise 1: Hello Script

**Task:** Create a script that greets you and shows file count.

**Solution:**

```bash
#!/bin/bash
# hello.sh

echo "What is your name?"
read name

echo ""
echo "Hello, $name!"
echo "Today is $(date)"
echo "Files in this directory: $(ls | wc -l)"
```

**Save and make executable:**

```bash
chmod +x hello.sh
./hello.sh
```

**Expected Output:**
```
What is your name?
Alice

Hello, Alice!
Today is Wed Feb  7 12:34:56 CET 2025
Files in this directory: 15
```

**Explanation:**
- `#!/bin/bash`: Shebang line tells Linux to run with bash
- `read name`: Prompt user for input, store in variable `name`
- `$name`: Variable expansion
- `$(date)`: Command substitution - captures output of date command
- `$(ls | wc -l)`: Count files in current directory

---

### Exercise 2: Number Guessing Game

**Task:** Create a guessing game with random number.

**Solution:**

```bash
#!/bin/bash
# guess.sh

random=$((1 + RANDOM % 100))
guess=0
attempts=0

echo "Guess the number (1-100)"

while [ $guess -ne $random ]; do
    read -p "Enter your guess: " guess
    ((attempts++))

    if [ $guess -lt $random ]; then
        echo "Too low!"
    elif [ $guess -gt $random ]; then
        echo "Too high!"
    fi
done

echo "Correct! You guessed it in $attempts attempts."
```

**Run the script:**

```bash
chmod +x guess.sh
./guess.sh
```

**Expected Output:**
```
Guess the number (1-100)
Enter your guess: 50
Too low!
Enter your guess: 75
Too high!
Enter your guess: 62
Too low!
Enter your guess: 68
Too high!
Enter your guess: 65
Correct! You guessed it in 5 attempts.
```

**Explanation:**
- `$RANDOM`: Built-in variable that generates random number (0-32767)
- `$((1 + RANDOM % 100))`: Random number 1-100
- `while [ condition ]`: Loop while condition is true
- `$guess -ne $random`: Numeric comparison (not equal)
- `$((attempts++))`: Increment counter (arithmetic expansion)
- `if-elif-fi`: Conditional branching

---

### Exercise 3: File Backup

**Task:** Create script to back up .txt files with timestamps.

**Solution:**

```bash
#!/bin/bash
# backup.sh

BACKUP_DIR="backup"
TIMESTAMP=$(date +%Y%m%d_%H%M%S)
COUNT=0

# Create backup directory
mkdir -p "$BACKUP_DIR"

# Copy all .txt files
for file in *.txt; do
    if [ -f "$file" ]; then
        cp "$file" "$BACKUP_DIR/${TIMESTAMP}_$file"
        ((COUNT++))
        echo "Backed up: $file"
    fi
done

echo ""
echo "Backup complete! Copied $COUNT files to $BACKUP_DIR/"
```

**Create test files and run:**

```bash
touch file1.txt file2.txt file3.txt other.log
chmod +x backup.sh
./backup.sh
```

**Expected Output:**
```
Backed up: file1.txt
Backed up: file2.txt
Backed up: file3.txt

Backup complete! Copied 3 files to backup/
```

```bash
# Verify backup
ls -la backup/
```

**Expected Output:**
```
total 12
-rw-r--r-- 1 user user 0 Feb  7 10:30 20250207_103000_file1.txt
-rw-r--r-- 1 user user 0 Feb  7 10:30 20250207_103000_file2.txt
-rw-r--r-- 1 user user 0 Feb  7 10:30 20250207_103000_file3.txt
```

**Explanation:**
- `mkdir -p`: Create directory (and parents if needed)
- `for file in *.txt`: Loop through all .txt files
- `if [ -f "$file" ]`: Check if it's a regular file
- `cp "$file" "destination"`: Copy file with timestamp prefix
- `${TIMESTAMP}_$file`: Variable expansion with string concatenation

---

### Exercise 4: System Monitor

**Task:** Check disk usage and service status.

**Solution:**

```bash
#!/bin/bash
# monitor.sh

# Check disk usage
DISK_USAGE=$(df / | tail -1 | awk '{print $5}' | sed 's/%//')
echo "Disk usage: $DISK_USAGE%"

if [ $DISK_USAGE -gt 80 ]; then
    echo "WARNING: Disk usage is above 80%!"
fi

# Check if SSH is running
echo ""
if systemctl is-active --quiet ssh; then
    echo "SSH service: Running"
else
    echo "SSH service: Not running"
fi

# Memory check
echo ""
echo "Memory usage:"
free -h | grep "Mem:"
```

**Run the script:**

```bash
chmod +x monitor.sh
./monitor.sh
```

**Expected Output:**
```
Disk usage: 46%

SSH service: Running

Memory usage:
Mem:           15Gi       4.5Gi       6.2Gi       512Mi       4.8Gi       9.8Gi
```

**Explanation:**
- `df / | tail -1 | awk '{print $5}'`: Extract disk usage percentage
- `sed 's/%//'`: Remove the % sign
- `systemctl is-active --quiet`: Check service status (no output)
- `free -h`: Show memory usage in human-readable format

---

### Exercise 5: Calculator

**Task:** Create a simple calculator.

**Solution:**

```bash
#!/bin/bash
# calculator.sh

read -p "Enter first number: " num1
read -p "Enter second number: " num2
read -p "Enter operation (+, -, *, /): " op

case $op in
    +)
        result=$(($num1 + $num2))
        ;;
    -)
        result=$(($num1 - $num2))
        ;;
    \*)
        result=$(($num1 * $num2))
        ;;
    /)
        if [ $num2 -eq 0 ]; then
            echo "Error: Division by zero"
            exit 1
        fi
        result=$(($num1 / $num2))
        ;;
    *)
        echo "Invalid operation"
        exit 1
        ;;
esac

echo ""
echo "$num1 $op $num2 = $result"
```

**Run the script:**

```bash
chmod +x calculator.sh
./calculator.sh
```

**Expected Output:**
```
Enter first number: 15
Enter second number: 3
Enter operation (+, -, *, /): *
15 * 3 = 45
```

**Explanation:**
- `read -p "prompt" var`: Prompt and read input
- `case $var in`: Pattern matching on variable
- `\*`: Escaped asterisk (matches literal *, not wildcard)
- `$((expr))`: Arithmetic expansion
- `exit 1`: Exit with error code 1

---

## Additional Script Examples

### Log Cleaner Script

```bash
#!/bin/bash
# clean_logs.sh

LOG_DIR="/var/log"
DAYS=30

echo "Finding logs older than $DAYS days..."

# Find old logs (dry run)
find "$LOG_DIR" -name "*.log" -type f -mtime +$DAYS

read -p "Delete these files? (y/n) " confirm

if [ "$confirm" = "y" ]; then
    find "$LOG_DIR" -name "*.log" -type f -mtime +$DAYS -delete
    echo "Old logs deleted."
else
    echo "Operation cancelled."
fi
```

### Batch File Renamer

```bash
#!/bin/bash
# rename.sh

PREFIX="backup_"

for file in *.txt; do
    if [ -f "$file" ]; then
        mv "$file" "${PREFIX}${file}"
        echo "Renamed: $file -> ${PREFIX}${file}"
    fi
done
```

---

## Scripting Best Practices Demonstrated

### 1. Shebang Line
```bash
#!/bin/bash  # Always specify interpreter
```

### 2. Comments
```bash
# This script backs up important files
# Author: Your Name
# Date: 2025-02-07
```

### 3. Quote Variables
```bash
# Good
echo "$name"
rm "$file"

# Bad (breaks with spaces)
echo $name
rm $file
```

### 4. Error Checking
```bash
#!/bin/bash
set -e  # Exit on error

# Or check specific commands
if ! cp source.txt dest.txt; then
    echo "Copy failed"
    exit 1
fi
```

### 5. Meaningful Names
```bash
# Good
backup_source="$HOME/Documents"
backup_destination="$HOME/backup"

# Bad
a="$HOME/Documents"
b="$HOME/backup"
```

---

## Debugging Scripts

### Enable Debugging

```bash
#!/bin/bash
set -x  # Print each command before executing

# Or run: bash -x script.sh
```

### Syntax Check

```bash
bash -n script.sh  # Check syntax without running
```

### Common Errors and Fixes

| Error | Cause | Fix |
|-------|-------|-----|
| `command not found` | Typo or command not installed | Check spelling |
| `permission denied` | Not executable | Run `chmod +x script.sh` |
| `syntax error` | Missing quote, bracket, etc. | Check matching pairs |
| `bad substitution` | Wrong variable syntax | Use `${var}` not `{var}` |
| `[: too many arguments` | Variable contains spaces | Quote variables: `"$var"` |

---

## Instructor Notes

### Teaching Tips

1. **Build scripts incrementally**: Start with simple, add features step by step
2. **Live coding**: Write scripts during class showing the thought process
3. **Debug demonstration**: Intentionally make mistakes and show how to fix them
4. **Comparison**: Show bash vs Python for same task to illustrate trade-offs
5. **Real examples**: Use scripts that solve actual problems students encounter

### Common Student Issues

| Issue | Solution |
|-------|----------|
| Permission denied when running | `chmod +x script.sh` to make executable |
| Script doesn't run | Check shebang line: `#!/bin/bash` |
| Variables not expanding | Use `$var` not `var`, quote properly |
| If statements failing | Check spaces: `[ $x -eq 5 ]` (spaces required) |
| Loop not executing | Check for files: `if [ -f "$file" ]` |

### Extension Activities

- Create a menu-driven script using select/case
- Build a system administration tool (user creation, backup automation)
- Parse command-line arguments with getopts
- Create functions for reusable code blocks
- Implement error handling and logging
- Write a script that monitors multiple system metrics
- Create an interactive text-based UI with dialog or whiptail
