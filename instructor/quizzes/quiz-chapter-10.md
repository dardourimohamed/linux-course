# Chapter 10 Quiz: Shell Scripting

## Quiz Questions

### Multiple Choice

**1. What must be at the beginning of a bash script?**
- A) #!/bin/bash
- B) # Bash script
- C) #!/bin/sh
- D) #!/usr/bin/env bash

**Answer:** A (D is also valid but A is most direct)

**2. How do you make a script executable?**
- A) chmod +x script.sh
- B) chmod 755 script.sh
- C) Both A and B
- D) ./script.sh

**Answer:** C

**3. What does `read` do in a bash script?**
- A) Reads a file
- B) Reads input from the user
- C) Reads script source code
- D) Reads environment variables

**Answer:** B

**4. Which operator tests if two numbers are equal?**
- A) =
- B) ==
- C) -eq
- D) ===

**Answer:** C

**5. What is the correct syntax for a for loop in bash?**
- A) for i in 1..10
- B) for i in {1..10}
- C) for i to 10
- D) for (i=1; i<=10; i++)

**Answer:** B

### True/False

**6. True or False: Variables in bash scripts cannot have spaces around the = sign.**

**Answer:** True

**7. True or False: The $@ variable holds all command-line arguments.**

**Answer:** True

**8. True or False: The elif keyword is used for else-if conditions.**

**Answer:** True

**9. True or False: Bash scripts can only be run from the terminal.**

**Answer:** False

**10. True or False: Functions in bash scripts can return values like in other programming languages.**

**Answer:** False (they return exit codes, use echo for values)

### Short Answer

**11. What are the three main types of loops in bash scripting, and when would you use each?**

**Answer:**
1. **for loop**: Iterate over a list of items or range. Use when you know the number of iterations or are processing a list.
   - Example: `for file in *.txt; do ...; done`

2. **while loop**: Repeat while a condition is true. Use when you don't know how many iterations in advance.
   - Example: `while [ $count -lt 10 ]; do ...; done`

3. **until loop**: Repeat until a condition becomes true. Use when you want to continue until something happens.
   - Example: `until ping -c 1 host; do ...; done`

**12. Explain the difference between $*, $@, and $# in bash scripts.**

**Answer:**
- **$***: Expands all arguments as a single word: "arg1 arg2 arg3"
- **$@**: Expands each argument as a separate word: "arg1" "arg2" "arg3"
- **$#**: Returns the number of arguments (count)

**Key difference**: When quoted, "$*" treats all arguments as one string, while "$@" preserves the separation. Use "$@" when iterating over arguments to preserve individual arguments.

**13. What is command substitution and how do you use it?**

**Answer:**
**Command substitution** allows you to use the output of a command as a variable value.

**Modern syntax** (preferred):
```bash
result=$(command)
```

**Old syntax** (still works):
```bash
result=`command`
```

**Examples:**
```bash
# Get current date
today=$(date +%Y-%m-%d)

# Count files in directory
count=$(ls | wc -l)

# Get disk usage
usage=$(df -h / | tail -1 | awk '{print $5}')
```

### Discussion Question

**14. Shell scripting is often described as "glue code" that connects other tools together. Explain why this description is accurate and give examples of how even simple scripts can automate complex tasks that would be time-consuming to do manually. What are the trade-offs between writing a shell script vs. writing in a "full" programming language like Python?**

**Sample Answer:**

**Why Shell Scripting is "Glue Code":**

Shell scripts excel at combining existing Linux tools to create automated workflows. Rather than rewriting functionality, you pipe together specialized commands:

**Example 1: Log Analysis**
```bash
#!/bin/bash
# Analyze web server logs for errors
grep "ERROR" /var/log/nginx/access.log | \
  awk '{print $1}' | \
  sort | uniq -c | \
  sort -rn | head -10
```
This 5-line script does what would require 20-30 lines in Python.

**Example 2: Automated Backup**
```bash
#!/bin/bash
# Backup important directories
for dir in Documents Pictures Projects; do
  tar -czf "backup-$dir-$(date +%Y%m%d).tar.gz" ~/"$dir"
done
```
Automates repetitive backup tasks with minimal code.

**Example 3: System Health Check**
```bash
#!/bin/bash
# Check system health
echo "Disk usage:"
df -h | grep -E "Filesystem|/$"
echo -e "\nMemory:"
free -h
echo -e "\nTop processes:"
ps aux | sort -rk 3 | head -5
```
One script replaces manually running multiple commands.

**Trade-offs: Shell vs. Python:**

**Choose Shell Scripting When:**
- Manipulating files and processes
- Running system commands
- Simple text processing with existing tools
- Quick automation tasks
- System administration scripts
- Startup/boot scripts
- Need maximum speed for command execution

**Choose Python When:**
- Complex data structures
- Mathematical calculations
- Cross-platform compatibility
- Error handling is critical
- Working with APIs/databases
- Complex string manipulation
- Large projects (>100 lines)
- Need for testing frameworks

**Key Differences:**
| Aspect | Shell | Python |
|--------|-------|--------|
| Learning curve | Shallow (quick start) | Steeper |
| Speed (dev) | Fast for simple tasks | Slower for simple |
| Speed (run) | Fast (external commands) | Slower (interpreter) |
| Error handling | Limited | Excellent |
| Portability | Linux/Unix only | Cross-platform |
| Use case | System glue | General programming |

**Conclusion**: Shell scripting shines for system administration and automation where you're combining existing tools. Python (or other languages) is better for application development and complex logic. The best approach is often to use both: shell for orchestration, Python for logic.

## Answer Key

| Question | Answer | Points |
|----------|--------|--------|
| 1 | A | 1 |
| 2 | C | 1 |
| 3 | B | 1 |
| 4 | C | 1 |
| 5 | B | 1 |
| 6 | True | 1 |
| 7 | True | 1 |
| 8 | True | 1 |
| 9 | False | 1 |
| 10 | False | 1 |
| 11 | Three loops explained | 4 |
| 12 | Three variables explained | 3 |
| 13 | Command substitution explained | 3 |
| 14 | Glue code explained + trade-offs | 5 |

**Total Points:** 27

**Passing Score:** 18/27 (67%)

---

## Instructor Notes

### Common Misconceptions to Address

1. **"Bash scripts are only for experts"** - Even simple 5-line scripts can save hours of manual work.

2. **"Shell scripting is obsolete"** - Still essential for system administration, DevOps, and automation.

3. **"Python can do everything bash can"** - True, but bash is often faster and simpler for system tasks.

4. **"Scripts don't need comments"** - Emphasize that comments are crucial for maintainability.

### Teaching Tips

- **Write scripts live**: Build scripts incrementally during class so students see the process
- **Debug demonstration**: Show bash -x for debugging scripts
- **Common errors**: Demonstrate common mistakes (spaces around =, missing shebang)
- **Real examples**: Use scripts that solve actual problems students encounter
- **Code review**: Review student scripts and show different ways to accomplish the same thing

### Extension Activities

- Script challenge: Write a script that monitors disk space and sends an alert
- Create a menu-driven script using select/case
- Build a simple system administration tool (user creation, backup automation)
- Convert a Python script to bash and compare
- Research shellcheck for script linting
- Explore advanced bash features (arrays, associative arrays, functions)
