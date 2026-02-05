# Chapter 5 Quiz: CLI Fundamentals

## Quiz Questions

### Multiple Choice

**1. Which command displays the entire contents of a file?**
- A) less
- B) head
- C) cat
- D) tail

**Answer:** C

**2. What does the pipe character (|) do?**
- A) Redirects output to a file
- B) Passes output of one command as input to another
- C) Comments out text
- D) Joins two commands together

**Answer:** B

**3. Which operator redirects output to a file and overwrites it?**
- A) >
- B) >>
- C) <
- D) |

**Answer:** A

**4. What does the wildcard * match?**
- A) Exactly one character
- B) Any number of characters (including none)
- C) Only numbers
- D) Only hidden files

**Answer:** B

**5. Which command shows the first 10 lines of a file?**
- A) head
- B) tail
- C) top
- D) less

**Answer:** A

### True/False

**6. True or False: The && operator runs the next command only if the previous one succeeded.**

**Answer:** True

**7. True or False: The >> operator overwrites the existing file content.**

**Answer:** False (it appends)

**8. True or False: The ? wildcard matches exactly one character.**

**Answer:** True

**9. True or False: Pipes and redirection are the same thing.**

**Answer:** False (pipes send to commands, redirection sends to files)

**10. True or False: The ; operator runs commands regardless of previous success or failure.**

**Answer:** True

### Short Answer

**11. What is the difference between pipes (|) and redirection (>), and when would you use each?**

**Answer:**
**Pipes (|)**: Send output of one command as input to another command. Use when you want to process or filter output through multiple commands.
Example: `ls | grep txt | sort`

**Redirection (>)**: Send output to a file (or input from a file). Use when you want to save output to a file for later, or read from a file.
Example: `ls > files.txt` or `wc -l < files.txt`

**12. Explain the difference between these three wildcards: *, ?, and [abc]**

**Answer:**
- **`*`** (asterisk): Matches any number of any characters (including zero)
  - `*.txt` matches all .txt files
  - `file*` matches file, file1, file_backup, etc.
- **`?`** (question mark): Matches exactly one character
  - `file?.txt` matches file1.txt, fileA.txt but NOT file10.txt
- **`[abc]`** (character class): Matches exactly one character from the set
  - `file[123].txt` matches file1.txt, file2.txt, file3.txt only

**13. What are the three command combination operators (;, &&, ||) and what does each do?**

**Answer:**
- **`;`** (semicolon): Runs commands sequentially regardless of success/failure
  - `mkdir newdir; cd newdir; ls` - all commands run
- **`&&`** (AND): Runs next command only if previous succeeded (exit code 0)
  - `mkdir newdir && cd newdir` - cd only runs if mkdir succeeded
- **`||`** (OR): Runs next command only if previous failed (non-zero exit code)
  - `grep pattern file || echo "Not found"` - echo only runs if grep found nothing

### Discussion Question

**14. Why is the ability to combine commands with pipes and redirection so powerful in Linux? Give at least three practical examples of how this enables more efficient work compared to using individual commands.**

**Sample Answer:**

**Power of pipes and redirection:**

1. **Building complex workflows from simple tools**: Each command does one thing well. Pipes let you chain them for powerful results.
   - Example: `cat access.log | grep "ERROR" | wc -l` counts errors in a log file
   - Without pipes: You'd need to manually search and count, or write a custom script

2. **Efficient data processing without intermediate files**: Process data directly without creating temporary files.
   - Example: `find . -name "*.log" -exec grep "ERROR" {} \;` searches all log files
   - Without pipes: You'd need to save output to files, then process each file

3. **Real-time monitoring and filtering**: Watch specific events in logs or processes.
   - Example: `tail -f /var/log/syslog | grep "error"` monitors errors in real-time
   - Without pipes: You'd see everything and have to visually filter

4. **Creating one-liners for complex tasks**: Replace multi-step scripts with simple command chains.
   - Example: `ps aux | sort -rk 3 | head -n 10` shows top 10 CPU users
   - Without pipes: Need to run ps, save output, sort manually, then pick top 10

5. **Composability**: Any command that reads from stdin and writes to stdout can be combined with any other.
   - Example: `curl api.com/data | jq '.users[] | .name' | sort | uniq` fetches API data, extracts names, sorts, and removes duplicates
   - Without pipes: Would need multiple files or complex scripts

**Conclusion**: Pipes and redirection embody the Unix philosophy - small, focused tools that can be combined to solve complex problems efficiently. This composability is what makes the CLI so powerful for experienced users.

## Answer Key

| Question | Answer | Points |
|----------|--------|--------|
| 1 | C | 1 |
| 2 | B | 1 |
| 3 | A | 1 |
| 4 | B | 1 |
| 5 | A | 1 |
| 6 | True | 1 |
| 7 | False | 1 |
| 8 | True | 1 |
| 9 | False | 1 |
| 10 | True | 1 |
| 11 | Correct distinction with examples | 4 |
| 12 | Three wildcards explained | 3 |
| 13 | Three operators explained | 3 |
| 14 | At least three examples | 5 |

**Total Points:** 27

**Passing Score:** 18/27 (67%)

---

## Instructor Notes

### Common Misconceptions to Address

1. **"Pipes and redirection are the same"** - Emphasize the difference: pipes connect commands, redirection connects to files.

2. **"> creates a new file, >> doesn't do anything different"** - Show the difference by appending multiple times and comparing.

3. **"Wildcards are only for filenames"** - Wildcards work with any command that accepts file arguments.

4. **"&& and ; do the same thing"** - Demonstrate with a failing command to show the difference clearly.

### Teaching Tips

- **Live pipeline building**: Start with a simple command, then add pipes one at a time to show how complexity grows
- **Error demonstration**: Show what happens when using && after a command that fails
- **Wildcard playground**: Create test files and demonstrate each wildcard pattern
- **Redirection visualization**: Draw the flow of data when using pipes vs redirection
- **Real-world examples**: Use actual log files or system data to show practical applications

### Extension Activities

- Pipeline puzzle: Give students a goal and have them build a pipeline to achieve it
- Create a "command chain" challenge: Who can create the longest useful pipeline?
- Compare grep output with and without pipes to show the difference
- Create files with different patterns and practice wildcards extensively
- Build a log analysis one-liner for a specific scenario
- Research and present on advanced text processing tools (awk, sed)
