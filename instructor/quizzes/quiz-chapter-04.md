# Chapter 4 Quiz: File System & Navigation

## Quiz Questions

### Multiple Choice

**1. What character represents the root directory in Linux?**
- A) ~
- B) /
- C) .
- D) ..

**Answer:** B

**2. Which command shows your current location in the file system?**
- A) ls
- B) cd
- C) pwd
- D) where

**Answer:** C

**3. What does the tilde (~) represent in Linux paths?**
- A) Root directory
- B) Current directory
- C) Home directory
- D) Parent directory

**Answer:** C

**4. Which option for ls shows hidden files?**
- A) -h
- B) -a
- C) -l
- D) -t

**Answer:** B

**5. What is the difference between a relative and absolute path?**
- A) Relative paths start with /, absolute paths don't
- B) Absolute paths start with /, relative paths don't
- C) Absolute paths only work with cd
- D) Relative paths are longer

**Answer:** B

### True/False

**6. True or False: The /home directory contains user home directories.**

**Answer:** True

**7. True or False: The command cd .. takes you to the root directory.**

**Answer:** False (it takes you to the parent directory)

**8. True or False: Tab completion works for both commands and file paths.**

**Answer:** True

**9. True or False: The /etc directory is meant for storing user data.**

**Answer:** False (it's for configuration files)

**10. True or False: The mkdir -p command creates parent directories if they don't exist.**

**Answer:** True

### Short Answer

**11. What do the following special directory symbols mean: ., .., ~, and /?**

**Answer:**
- . (dot) - Current directory
- .. (dot dot) - Parent directory (one level up)
- ~ (tilde) - Home directory of the current user
- / (forward slash) - Root directory of the entire file system

**12. List five important top-level directories in the Linux file system and explain what each is used for.**

**Answer:**
(Choose any five)
- /bin - Essential user binaries (commands like ls, cp, cat)
- /etc - System-wide configuration files
- /home - User home directories and personal files
- /var - Variable data (logs, spool files, web server data)
- /tmp - Temporary files that don't need to persist
- /usr - User-related programs and data
- /root - Home directory for the root user
- /boot - Boot loader files
- /dev - Device files
- /lib - System libraries

**13. What happens when you use rm -rf on a directory, and why must you be careful?**

**Answer:**
The rm -rf command recursively and forcefully removes a directory and all its contents without asking for confirmation. It's dangerous because:
- It's permanent - there's no undo or recycle bin in the terminal
- It can delete system files if used incorrectly
- It doesn't ask for confirmation on each file
- A simple typo (like adding an extra space) can delete important data
Always double-check your command before pressing Enter!

### Discussion Question

**14. Why does Linux use a single unified directory tree starting at root (/) instead of drive letters like Windows (C:, D:, etc.)? What are the advantages and disadvantages of each approach?**

**Sample Answer:**

**Linux unified directory approach:**
**Advantages:**
- Unified view of all storage - everything is in one tree
- More flexible storage management - can add/remove storage without changing paths
- Unix philosophy - everything is a file, accessible consistently
- Easier scripting - same paths work regardless of physical storage
- Mount points make any storage appear as part of the tree

**Disadvantages:**
- Can be confusing for Windows users
- Must understand mount points and device mapping
- Harder to see which physical device contains which files at a glance

**Windows drive letter approach:**
**Advantages:**
- Simple separation of physical devices
- Clear which files are on which drive
- Familiar to many users
- Easy to see available space per drive

**Disadvantages:**
- Limited to 26 drive letters
- Different paths depending on what's connected
- No unified file hierarchy
- Less flexible for modern storage needs

**Conclusion**: Linux's unified approach is more flexible and powerful, especially for servers and systems with multiple storage devices. Windows' approach is simpler for basic use but doesn't scale well.

## Answer Key

| Question | Answer | Points |
|----------|--------|--------|
| 1 | B | 1 |
| 2 | C | 1 |
| 3 | C | 1 |
| 4 | B | 1 |
| 5 | B | 1 |
| 6 | True | 1 |
| 7 | False | 1 |
| 8 | True | 1 |
| 9 | False | 1 |
| 10 | True | 1 |
| 11 | Four symbols explained | 4 |
| 12 | Five directories with purposes | 3 |
| 13 | Correct explanation | 3 |
| 14 | Both approaches compared | 5 |

**Total Points:** 27

**Passing Score:** 18/27 (67%)

---

## Instructor Notes

### Common Misconceptions to Address

1. **"Where's my C: drive?"** - Explain that Linux doesn't use drive letters. All storage is mounted into the single directory tree.

2. **"I can delete anything in /tmp"** - True, but emphasize that /tmp is for temporary files. Don't use it for important documents.

3. **"cd / takes me home"** - No, that takes you to root. cd or cd ~ takes you home. This is a common confusion.

4. **"Hidden files are secret"** - Hidden files (starting with .) are just configuration files, not necessarily secret. They're hidden to reduce clutter.

### Teaching Tips

- **Tree diagrams**: Draw the file system hierarchy on the board to visualize the structure
- **Live navigation**: Demonstrate moving around the file system in real-time, explaining each command
- **Tab completion game**: Challenge students to navigate using tab completion only
- **rm warning**: Tell the story of someone who accidentally deleted important files (fictional but cautionary)
- **pwd importance**: Emphasize always knowing where you are before running commands that affect files

### Extension Activities

- File system scavenger hunt: Find specific files in /etc, /var, /usr
- Create a visual map of the directory tree
- Explore /proc and /sys - what weird things live there?
- Research why Unix uses forward slashes while Windows uses backslashes
- Compare the Linux file system structure with macOS (they're similar!)
- Use tree command to generate directory trees and analyze them
