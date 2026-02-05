# Chapter 6 Quiz: Text Processing

## Quiz Questions

### Multiple Choice

**1. Which command is used to search for patterns in files?**
- A) find
- B) grep
- C) search
- D) locate

**Answer:** B

**2. What option makes grep case-insensitive?**
- A) -c
- B) -v
- C) -i
- D) -n

**Answer:** C

**3. Which command finds files by name, size, or type?**
- A) grep
- B) find
- C) locate
- D) search

**Answer:** B

**4. What does the -v option do in grep?**
- A) Shows line numbers
- B) Inverts the match (shows non-matching lines)
- C) Verbose mode
- D) Counts matches

**Answer:** B

**5. Which tool is best for columnar data processing?**
- A) grep
- B) sed
- C) awk
- D) sort

**Answer:** C

### True/False

**6. True or False: The find command can execute commands on files it finds.**

**Answer:** True

**7. True or False: awk is only used for simple text substitution.**

**Answer:** False (it's a powerful text processing language)

**8. True or False: The uniq command removes duplicate lines only if the file is sorted first.**

**Answer:** True

**9. True or False: The sort command can only sort alphabetically.**

**Answer:** False (it can sort numerically with -n)

**10. True or False: Regular expressions work the same in all Linux commands.**

**Answer:** False (different tools support different regex flavors)

### Short Answer

**11. What are the five core text processing commands covered in this chapter, and what does each do?**

**Answer:**
1. **grep** - Search for patterns in files using regular expressions
2. **find** - Locate files by name, size, type, or time
3. **sed** - Stream editor for text substitution and transformation
4. **awk** - Text processing language for columnar data and formatting
5. **sort** - Arrange lines alphabetically or numerically

(Bonus: **uniq** - Remove duplicate lines, **wc** - count lines/words/characters)

**12. Explain the difference between these two commands and when you'd use each:**
**`grep "pattern" file.txt` vs `find . -name "*.txt" -exec grep "pattern" {} \;`**

**Answer:**
- **`grep "pattern" file.txt`**: Searches for "pattern" in a single file called file.txt. Use when you know exactly which file to search.

- **`find . -name "*.txt" -exec grep "pattern" {} \;`**: Finds all .txt files in the current directory and subdirectories, then runs grep on each one. Use when you want to search through multiple files or don't know exactly where the files are.

The find command is recursive and can search entire directory trees, while grep alone only searches specified files.

**13. How would you count the occurrences of each unique word in a text file using Linux text processing commands?**

**Answer:**
```bash
tr '[:space:]' '\n' < file.txt | sort | uniq -c | sort -rn
```

Or alternatively:
```bash
cat file.txt | tr -cs '[:alpha:]' '\n' | sort | uniq -c | sort -rn
```

**Breakdown:**
- `tr '[:space:]' '\n'` - Convert all spaces to newlines (one word per line)
- `sort` - Group identical words together
- `uniq -c` - Count occurrences of each unique word
- `sort -rn` - Sort by count in descending order

### Discussion Question

**14. Text processing tools are some of the most powerful commands in Linux. Explain why the ability to combine these tools (with pipes) enables data analysis that would require complex scripts in other environments. Give specific examples of real-world tasks that become simple with these tools.**

**Sample Answer:**

**Why combined text processing is so powerful:**

1. **Composability**: Each tool does one thing well, and pipes let you chain them for complex results without scripting.

2. **No intermediate files**: Process data streaming through the pipeline - efficient and fast.

3. **Data transformation pipeline**: You can filter, transform, sort, and format data in one continuous flow.

**Real-world examples:**

**Log Analysis:**
```bash
cat /var/log/nginx/access.log | awk '{print $1}' | sort | uniq -c | sort -rn | head -10
```
Finds top 10 IP addresses by request count. Would require complex parsing in most languages.

**Security Scanning:**
```bash
grep -r "password" /var/www/html 2>/dev/null | grep -v ".git"
```
Finds any files containing "password" in web directory - quick security audit.

**File Type Analysis:**
```bash
find . -type f -name "*.*" | sed 's/.*\.//' | sort | uniq -c | sort -rn
```
Counts files by extension - shows what types of files are in a project.

**Code Quality:**
```bash
find . -name "*.py" -exec wc -l {} + | sort -n | tail -10
```
Finds the 10 largest Python files by line count - identify code that needs refactoring.

**System Monitoring:**
```bash
ps aux | sort -rk 3 | head -n 5 | awk '{print $2, $11}'
```
Shows PID and command of top 5 CPU-using processes.

**Data Cleaning:**
```bash
sed 's/\r$//' windows.txt > unix.txt
```
Converts Windows line endings to Unix format - essential for processing data from Windows sources.

**Conclusion**: The Unix philosophy of small, composable tools means you can perform sophisticated data analysis and manipulation without writing complex programs. Each tool is a building block, and pipes are the connectors.

## Answer Key

| Question | Answer | Points |
|----------|--------|--------|
| 1 | B | 1 |
| 2 | C | 1 |
| 3 | B | 1 |
| 4 | B | 1 |
| 5 | C | 1 |
| 6 | True | 1 |
| 7 | False | 1 |
| 8 | True | 1 |
| 9 | False | 1 |
| 10 | False | 1 |
| 11 | Five commands explained | 4 |
| 12 | Correct distinction | 3 |
| 13 | Correct pipeline | 3 |
| 14 | Examples and explanation | 5 |

**Total Points:** 27

**Passing Score:** 18/27 (67%)

---

## Instructor Notes

### Common Misconceptions to Address

1. **"grep is just for searching text"** - Grep is powerful with regex for complex pattern matching, not just simple text.

2. **"find is slow compared to GUI search"** - Find is actually very fast and can search entire filesystems efficiently.

3. **"sed and awk are obsolete"** - These tools are still widely used in scripts and for one-off text processing tasks.

4. **"Regular expressions are too hard to learn"** - Start simple and build up. Even basic regex knowledge is incredibly useful.

### Teaching Tips

- **Live demo with real logs**: Download a sample web server log and analyze it live during class
- **Build pipelines incrementally**: Start with cat, add grep, add awk, add sort - students see complexity build
- **Regex playground**: Use regex101.com or similar to visually demonstrate regex patterns
- **Compare tools**: Show how grep, sed, and awk can accomplish similar tasks but with different approaches
- **Practical examples**: Use real-world scenarios students might encounter (log analysis, data cleaning)

### Extension Activities

- Create a "log detective" challenge: Find specific patterns in sample logs
- Pipeline puzzle: Provide the desired output and have students build the pipeline
- Regex bingo: Create regex patterns that match specific sets of strings
- Compare Python vs Linux text processing for the same task
- Analyze the course's own source code with these tools
- Research and present on advanced awk one-liners
