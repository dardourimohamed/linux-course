# Chapter 7 Quiz: Permissions & Users

## Quiz Questions

### Multiple Choice

**1. What does the permission string "rwxr-xr--" mean?**
- A) Owner can read/write/execute, group can read/execute, others can read
- B) Everyone can read/write/execute
- C) Only owner can do anything
- D) Owner can read, group can write, others can execute

**Answer:** A

**2. Which command changes file permissions?**
- A) chown
- B) chmod
- C) chgrp
- D) attrib

**Answer:** B

**2. What permission mode makes a script executable?**
- A) chmod 644 script.sh
- B) chmod 755 script.sh
- C) chmod +x script.sh
- D) Both B and C

**Answer:** D

**3. What does the setuid bit do?**
- A) Makes a file readable by everyone
- B) Allows a file to run with the permissions of its owner
- C) Prevents deletion of files in a directory
- D) Makes files inherit group permissions

**Answer:** B

**5. Which command changes the owner of a file?**
- A) chmod
- B) chown
- C) chgrp
- D) usermod

**Answer:** B

### True/False

**6. True or False: The root user can access any file regardless of its permissions.**

**Answer:** True

**7. True or False: You must use sudo to change file ownership.**

**Answer:** True

**8. True or False: The sticky bit prevents users from deleting files they don't own.**

**Answer:** True

**9. True or False: The umask command sets default permissions for new files.**

**Answer:** True

**10. True or False: Group membership is always additive when using usermod -aG.**

**Answer:** True

### Short Answer

**11. What are the three permission types (r, w, x) and what do they mean for files vs. directories?**

**Answer:**
| Permission | Files | Directories |
|------------|-------|-------------|
| **r (read)** | View file contents | List directory contents |
| **w (write)** | Modify file contents | Add/remove files in directory |
| **x (execute)** | Run as program/enter directory | Enter/search directory |

**12. Explain the difference between symbolic mode (chmod u+x) and numeric mode (chmod 755).**

**Answer:**
**Symbolic mode** (e.g., `chmod u+x`):
- More readable and intuitive
- Uses letters: u (user), g (group), o (others), a (all)
- Operators: + (add), - (remove), = (set exactly)
- Permissions: r, w, x
- Example: `chmod u+x,g-w file` adds execute for user, removes write for group

**Numeric mode** (e.g., `chmod 755`):
- Uses octal numbers: r=4, w=2, x=1
- Three digits: user, group, others
- Faster to type once learned
- Example: `chmod 755` = rwxr-xr-x (user: 4+2+1=7, group: 4+1=5, others: 4+1=5)

**13. What are the three special permission bits (SUID, SGID, sticky bit) and what does each do?**

**Answer:**
1. **SUID (Set User ID) - 4**: When executed, file runs with permissions of file owner (not the user running it). Example: passwd command needs to modify /etc/shadow.

2. **SGID (Set Group ID) - 2**: Files created in directory inherit the directory's group. Useful for shared project directories.

3. **Sticky bit - 1**: Only file owner can delete files in directory (even if others have write permission). Used for /tmp.

### Discussion Question

**14. Why does Linux have a multi-user permission model instead of making everything accessible to everyone? Discuss the security implications and give examples of why this is important for both personal systems and multi-user servers.**

**Sample Answer:**

**Security Benefits of Permission Model:**

1. **Principle of Least Privilege**: Users and processes only have access to what they need. If compromised, damage is limited.

2. **User Isolation**: Multiple users can share a system without accessing each other's files.

3. **System Protection**: Critical system files (/etc/passwd, binaries) can't be modified by regular users.

4. **Process Security**: Services run as dedicated users (www-data, postgres) - if compromised, attacker is confined.

**Personal System Examples:**
- Browser malware can't read your SSH keys if permissions are correct
- If you accidentally run a malicious script, it can't modify system binaries
- Your personal documents remain private even if someone briefly accesses your computer

**Server Examples:**
- Web server (www-data) can only read web files, can't access user home directories
- Database user can only access database files, not system configuration
- Multiple developers on a server can't accidentally modify each other's code
- If an FTP server is compromised, attacker is confined to FTP directory

**Without Permissions:**
- Any user could read anyone's email, documents, SSH keys
- Malicious software could modify system binaries to persist
- Accidental deletion of critical system files
- No accountability - can't track who did what

**Conclusion**: The permission model is fundamental to Linux security. It creates a defense-in-depth strategy where even if one layer fails, damage is contained by permission boundaries.

## Answer Key

| Question | Answer | Points |
|----------|--------|--------|
| 1 | A | 1 |
| 2 | B | 1 |
| 3 | D | 1 |
| 4 | B | 1 |
| 5 | B | 1 |
| 6 | True | 1 |
| 7 | True | 1 |
| 8 | True | 1 |
| 9 | True | 1 |
| 10 | True | 1 |
| 11 | Three types with file/directory | 4 |
| 12 | Both modes explained | 3 |
| 13 | Three special bits explained | 3 |
| 14 | Security implications discussed | 5 |

**Total Points:** 27

**Passing Score:** 18/27 (67%)

---

## Instructor Notes

### Common Misconceptions to Address

1. **"Permissions only matter on servers"** - Personal systems need permissions too, especially for security and privacy.

2. **"777 permissions make things easier"** - Emphasize that 777 is a security risk. Only use when absolutely necessary.

3. **"root can do anything, so permissions don't matter"** - Root can override permissions, but that's exactly why we limit root usage and use sudo.

4. **"I'll just use chmod -R 777 to fix permission errors"** - This is a bad habit. Teach students to identify specific permission problems and fix them correctly.

### Teaching Tips

- **Permission calculator**: Use visual tools or diagrams to show how numeric permissions work
- **Live demonstration**: Create a file, try to access it with wrong permissions, then fix it
- **Real-world scenarios**: Show how web servers use specific users and permissions for security
- **ls -l analysis**: Spend time teaching students to read and understand ls -l output
- **Security stories**: Share real examples of how permissions prevent (or fail to prevent) security breaches

### Extension Activities

- Set up a shared directory with correct group permissions for team collaboration
- Analyze permission problems in broken systems and fix them
- Research the difference between DAC (Discretionary Access Control) and MAC (Mandatory Access Control)
- Explore how containers handle permissions differently
- Set up a web directory with proper permissions for a project
