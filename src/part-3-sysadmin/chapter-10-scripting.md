# Chapter 10: Shell Scripting

## Learning Objectives

By the end of this chapter, you will be able to:

- [x] Write and execute bash scripts
- [x] Use variables and user input
- [x] Implement conditionals and comparisons
- [x] Use loops for repetitive tasks
- [x] Create functions for reusable code
- [x] Automate real-world system administration tasks

## Prerequisites

- Completed Chapter 9: Processes & Services
- Comfortable with basic commands and pipes
- Understanding of file permissions

---

## What is Shell Scripting?

A **shell script** is a text file containing a sequence of commands that the shell can execute. Think of it as:
- **Automation** - Run multiple commands with one command
- **Programming** - Use variables, loops, and conditionals
- **Productivity** - Save time on repetitive tasks

```bash
#!/bin/bash
# This is a simple script
echo "Hello, World!"
date
whoami
```

### Why Write Scripts?

| Task | Manual Way | Script Way |
|------|------------|------------|
| Backup files | Copy each folder | `./backup.sh` |
| Deploy app | Type 10 commands | `./deploy.sh` |
| Clean logs | Delete each file | `./cleanup.sh` |
| Monitor system | Run top, ps, free | `./health.sh` |

---

## Creating Your First Script

### Script Structure

Every bash script starts with a **shebang**:

```bash
#!/bin/bash
# ^ This tells Linux to run this file with bash

# Comments start with #
echo "This is my first script!"
```

### Step 1: Create the Script

```bash
# Create the file
nano myscript.sh

# Or use echo
cat > myscript.sh << 'EOF'
#!/bin/bash
echo "Hello, World!"
echo "Today is $(date)"
echo "Current user: $USER"
EOF
```

### Step 2: Make It Executable

```bash
chmod +x myscript.sh

# Verify permissions
ls -l myscript.sh
-rwxr-xr-x 1 user user ... myscript.sh
# ^^^
# The 'x' means executable
```

### Step 3: Run the Script

```bash
# Execute directly
./myscript.sh

# Or with bash explicitly
bash myscript.sh

# Or with sh
sh myscript.sh
```

---

## Variables

### Defining Variables

```bash
#!/bin/bash

# No spaces around '='
name="Alice"
age=25
greeting="Hello, $name!"

echo $name           # Alice
echo $greeting       # Hello, Alice!
echo "I am $age"     # I am 25
```

### Variable Rules

```bash
# Correct
NAME="value"
my_var="value"
_var2="value"

# WRONG - spaces around =
name = "Alice"    # Error

# WRONG - using $ on left side
$name="Bob"       # Error
```

### Using Variables

```bash
#!/bin/bash

file="report.txt"
count=5

echo "File: $file"           # File: report.txt
echo "Count: $count"         # Count: 5

# Braces for clarity
echo "${file}_backup"        # report.txt_backup
```

### Environment Variables

These are predefined variables available in every script:

```bash
#!/bin/bash

echo "Current user: $USER"
echo "Home directory: $HOME"
echo "Current directory: $PWD"
echo "Shell: $SHELL"
echo "Hostname: $HOSTNAME"
echo "Path: $PATH"
```

| Variable | Description |
|----------|-------------|
| `$USER` | Current username |
| `$HOME` | User's home directory |
| `$PWD` | Present working directory |
| `$SHELL` | Default shell path |
| `$HOSTNAME` | System hostname |
| `$PATH` | Command search paths |

---

## Command Substitution

Use the output of a command as a variable value.

### Syntax: `$(command)` or `` `command` ``

```bash
#!/bin/bash

# Get today's date
today=$(date +%Y-%m-%d)
echo "Today is: $today"

# Count files
file_count=$(ls | wc -l)
echo "Files in current directory: $file_count"

# Get disk usage
disk_usage=$(df -h / | tail -1 | awk '{print $5}')
echo "Disk usage: $disk_usage"
```

### Old Syntax (Backticks)

```bash
# Still works, but prefer $(...)
today=`date +%Y-%m-%d`
```

---

## User Input

### Reading Input with `read`

```bash
#!/bin/bash

echo "What is your name?"
read name
echo "Hello, $name!"
```

### Prompt and Input on One Line

```bash
read -p "Enter your name: " name
echo "Hello, $name!"
```

### Silent Input (for passwords)

```bash
read -s -p "Enter password: " password
echo  # newline
echo "Password received (not shown)"
```

### Reading Multiple Values

```bash
read -p "Enter name age: " name age
echo "Name: $name, Age: $age"
```

---

## Conditionals

### if Statements

```bash
#!/bin/bash

count=10

if [ $count -eq 10 ]; then
    echo "Count is 10"
fi
```

### if-else

```bash
#!/bin/bash

age=18

if [ $age -ge 18 ]; then
    echo "You are an adult"
else
    echo "You are a minor"
fi
```

### if-elif-else

```bash
#!/bin/bash

score=75

if [ $score -ge 90 ]; then
    echo "Grade: A"
elif [ $score -ge 80 ]; then
    echo "Grade: B"
elif [ $score -ge 70 ]; then
    echo "Grade: C"
else
    echo "Grade: F"
fi
```

### Comparison Operators

| File Test | Meaning |
|-----------|---------|
| `-f file` | File exists |
| `-d dir` | Directory exists |
| `-e file` | Exists (file or dir) |
| `-r file` | File is readable |
| `-w file` | File is writable |
| `-x file` | File is executable |

| String Comparison | Meaning |
|------------------|---------|
| `"$s1" = "$s2"` | Strings equal |
| `"$s1" != "$s2"` | Strings not equal |
| `-z "$s"` | String is empty |
| `-n "$s"` | String is not empty |

| Number Comparison | Meaning |
|------------------|---------|
| `$n1 -eq $n2` | Equal |
| `$n1 -ne $n2` | Not equal |
| `$n1 -gt $n2` | Greater than |
| `$n1 -ge $n2` | Greater or equal |
| `$n1 -lt $n2` | Less than |
| `$n1 -le $n2` | Less or equal |

### Examples

```bash
#!/bin/bash

# File check
if [ -f "/etc/passwd" ]; then
    echo "Password file exists"
fi

# String comparison
name="Alice"
if [ "$name" = "Alice" ]; then
    echo "Welcome, Alice!"
fi

# Number comparison
age=20
if [ $age -ge 18 ]; then
    echo "You can vote"
fi

# Combining conditions
if [ -f "data.txt" ] && [ -r "data.txt" ]; then
    echo "File exists and is readable"
fi
```

### Logical Operators

```bash
-a    # AND (within single test)
-o    # OR (within single test)
!     # NOT
&&    # AND (between commands)
||    # OR (between commands)

# Example
if [ $age -ge 18 ] && [ $age -le 65 ]; then
    echo "Working age"
fi
```

---

## Loops

### for Loop

```bash
#!/bin/bash

# Loop through numbers
for i in 1 2 3 4 5; do
    echo "Number: $i"
done

# Loop through range
for i in {1..10}; do
    echo "Count: $i"
done

# Loop through files
for file in *.txt; do
    echo "Processing: $file"
done
```

### while Loop

```bash
#!/bin/bash

count=1
while [ $count -le 5 ]; do
    echo "Count: $count"
    count=$((count + 1))
done
```

### until Loop

```bash
#!/bin/bash

count=1
until [ $count -gt 5 ]; do
    echo "Count: $count"
    count=$((count + 1))
done
```

### Breaking Out of Loops

```bash
#!/bin/bash

# break - exit loop entirely
for i in {1..10}; do
    if [ $i -eq 5 ]; then
        break
    fi
    echo $i
done
# Output: 1 2 3 4

# continue - skip to next iteration
for i in {1..5}; do
    if [ $i -eq 3 ]; then
        continue
    fi
    echo $i
done
# Output: 1 2 4 5
```

---

## Functions

### Defining Functions

```bash
#!/bin/bash

# Define a function
greet() {
    echo "Hello, $1!"
}

# Call the function
greet "Alice"
greet "Bob"
```

### Functions with Multiple Arguments

```bash
#!/bin/bash

greet() {
    name=$1
    age=$2
    echo "Hello, $name! You are $age years old."
}

greet "Alice" 25
greet "Bob" 30
```

### Returning Values

```bash
#!/bin/bash

add() {
    result=$(($1 + $2))
    echo $result
}

sum=$(add 5 3)
echo "Sum is: $sum"
```

### Functions with Return Code

```bash
#!/bin/bash

check_file() {
    if [ -f "$1" ]; then
        return 0    # Success
    else
        return 1    # Failure
    fi
}

if check_file "data.txt"; then
    echo "File exists"
else
    echo "File not found"
fi
```

---

## Arithmetic

### Arithmetic Expansion

```bash
#!/bin/bash

a=10
b=5

# Addition
echo $(($a + $b))      # 15

# Subtraction
echo $(($a - $b))      # 5

# Multiplication
echo $(($a * $b))      # 50

# Division
echo $(($a / $b))      # 2

# Modulo
echo $(($a % $b))      # 0

# Exponentiation
echo $(($a ** 2))      # 100
```

### Shorthand Arithmetic

```bash
#!/bin/bash

count=5

count=$((count + 1))   # Increment
echo $count            # 6

((count++))            # Post-increment
echo $count            # 7

((count+=5))           # Add 5
echo $count            # 12
```

---

## Special Variables

### Positional Parameters

```bash
#!/bin/bash

# $0    - Script name
# $1    - First argument
# $2    - Second argument
# $@    - All arguments
# $#    - Number of arguments

echo "Script name: $0"
echo "First arg: $1"
echo "Second arg: $2"
echo "All args: $@"
echo "Number of args: $#"
```

### Exit Status

```bash
#!/bin/bash

# $? - Exit status of last command

ls /nonexistent
if [ $? -ne 0 ]; then
    echo "Command failed"
fi
```

---

## Practical Scripts

### Example 1: Backup Script

```bash
#!/bin/bash
# backup.sh - Backup important files

SOURCE="$HOME/Documents"
DESTINATION="$HOME/backup"
DATE=$(date +%Y%m%d_%H%M%S)
BACKUP_DIR="$DESTINATION/backup_$DATE"

# Create backup directory
mkdir -p "$BACKUP_DIR"

# Copy files
echo "Backing up $SOURCE to $BACKUP_DIR"
cp -r "$SOURCE"/* "$BACKUP_DIR/"

# Compress
tar -czf "$BACKUP_DIR.tar.gz" "$BACKUP_DIR"
rm -rf "$BACKUP_DIR"

echo "Backup complete: $BACKUP_DIR.tar.gz"
```

### Example 2: System Health Check

```bash
#!/bin/bash
# health.sh - Check system health

echo "=== System Health Check ==="
echo "Date: $(date)"
echo ""

# Disk usage
echo "--- Disk Usage ---"
df -h | grep -E "Filesystem|/$"

# Memory
echo ""
echo "--- Memory Usage ---"
free -h

# CPU
echo ""
echo "--- CPU Usage ---"
top -bn1 | grep "Cpu(s)"

# Top processes
echo ""
echo "--- Top 5 Processes by CPU ---"
ps aux | sort -rk 3 | head -n 6

echo ""
echo "=== Health Check Complete ==="
```

### Example 3: Log Cleaner

```bash
#!/bin/bash
# clean_logs.sh - Remove old log files

LOG_DIR="/var/log"
DAYS=30

echo "Finding logs older than $DAYS days..."

# Find and remove old logs (dry run)
find "$LOG_DIR" -name "*.log" -type f -mtime +$DAYS

read -p "Delete these files? (y/n) " confirm

if [ "$confirm" = "y" ]; then
    find "$LOG_DIR" -name "*.log" -type f -mtime +$DAYS -delete
    echo "Old logs deleted."
else
    echo "Operation cancelled."
fi
```

### Example 4: Batch File Renamer

```bash
#!/bin/bash
# rename.sh - Rename files in batch

# Add prefix to all files
PREFIX="backup_"

for file in *.txt; do
    if [ -f "$file" ]; then
        mv "$file" "${PREFIX}${file}"
        echo "Renamed: $file -> ${PREFIX}${file}"
    fi
done
```

### Example 5: User Creation Script

```bash
#!/bin/bash
# create_user.sh - Create a new user

if [ $(id -u) -ne 0 ]; then
    echo "This script must be run as root"
    exit 1
fi

read -p "Enter username: " username

# Check if user exists
if id "$username" &>/dev/null; then
    echo "User $username already exists"
    exit 1
fi

# Create user
useradd -m "$username"

# Set password
passwd "$username"

echo "User $username created successfully"
```

---

## Best Practices

### 1. Use Shebang

```bash
#!/bin/bash
# Always specify the interpreter
```

### 2. Use Comments

```bash
#!/bin/bash
# This script backs up important files
# Author: Your Name
# Date: 2025-02-07

# Create backup directory
mkdir -p backup
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

### 4. Check for Errors

```bash
#!/bin/bash
set -e    # Exit on error

# Or check specific commands
if ! cp source.txt dest.txt; then
    echo "Copy failed"
    exit 1
fi
```

### 5. Use Meaningful Names

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
set -x    # Print each command before executing
# or run: bash -x script.sh

set -v    # Print script lines as they are read
```

### Syntax Check

```bash
bash -n script.sh    # Check syntax without running
```

### Common Errors

| Error | Cause | Fix |
|-------|-------|-----|
| `command not found` | Typo or command not installed | Check spelling |
| `permission denied` | Not executable | Run `chmod +x script.sh` |
| `syntax error` | Missing quote, bracket, etc. | Check matching pairs |
| `bad substitution` | Wrong variable syntax | Use `${var}` not `{var}` |

---

## Summary

In this chapter, you learned:

- **Script Basics**: Shebang, creating, and executing scripts
- **Variables**: Defining and using variables, environment variables
- **Input/Output**: `echo`, `read`, command substitution
- **Conditionals**: `if`, `else`, `elif`, comparison operators
- **Loops**: `for`, `while`, `until`, `break`, `continue`
- **Functions**: Creating reusable code blocks
- **Arithmetic**: Math operations and calculations
- **Best Practices**: Comments, error checking, quoting

---

## Chapter Quiz

Test your understanding of shell scripting:

{{#quiz ../../quizzes/chapter-10.toml}}

---

## Exercises

### Exercise 1: Hello Script

1. Create a script that asks for your name
2. Greets you with the current date
3. Tells you how many files are in the current directory

### Exercise 2: Number Guessing Game

1. Generate a random number: `random=$((1 + RANDOM % 100))`
2. Ask the user to guess
3. Tell them if it's too high or too low
4. Continue until they guess correctly

### Exercise 3: File Backup

1. Create a script that backs up all `.txt` files
2. Create a `backup/` directory if it doesn't exist
3. Copy all `.txt` files with a timestamp prefix
4. Print a summary of files copied

### Exercise 4: System Monitor

1. Create a script that checks disk usage
2. If disk usage is above 80%, print a warning
3. Check if a specific service is running
4. Print a summary report

### Exercise 5: Calculator

1. Create a script that takes two numbers
2. Ask for the operation (+, -, *, /)
3. Perform the calculation
4. Display the result

---

## Expected Output

### Exercise 1 Solution

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

```bash
$ chmod +x hello.sh
$ ./hello.sh
What is your name?
Alice

Hello, Alice!
Today is Wed Feb  7 12:34:56 CET 2025
Files in this directory: 15
```

### Exercise 2 Solution

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

### Exercise 3 Solution

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

### Exercise 4 Solution

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

### Exercise 5 Solution

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

---

## Next Chapter

In Chapter 11, you'll learn **Networking Basics** - understanding IP addresses, checking connectivity, using SSH for remote access, and troubleshooting network issues.
