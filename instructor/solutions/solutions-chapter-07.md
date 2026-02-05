# Chapter 7 Solutions: Permissions & Users

## Exercise Solutions

### Exercise 1: Understand Permissions

**Task:** Examine and understand file permissions.

**Solution:**

```bash
# Create test files and directories
touch file1.txt file2.sh
mkdir testdir

# View permissions
ls -l
```

**Expected Output:**
```
-rw-r--r-- 1 user user    0 Feb  7 10:00 file1.txt
-rw-r--r-- 1 user user    0 Feb  7 10:00 file2.sh
drwxr-xr-x 2 user user 4096 Feb  7 10:00 testdir
```

```bash
# Understand the permission string
ls -ld testdir
```

**Expected Output:**
```
drwxr-xr-x 2 user user 4096 Feb  7 10:00 testdir
```

**Breakdown:**
```
d  rwx  r-x  r-x
|  |    |    |
|  |    |    +-- Others: read+execute
|  |    +------- Group: read+execute
|  +------------ Owner: read+write+execute
+--------------- File type (d=directory, -=file)
```

```bash
# View numeric permissions
stat -c '%a %n' *
```

**Expected Output:**
```
644 file1.txt
644 file2.sh
755 testdir
```

```bash
# Cleanup
rm file1.txt file2.sh
rmdir testdir
```

**Explanation:**
- Permission string: 10 characters (type + 3x permissions)
- Each group of 3: read (r=4), write (w=2), execute (x=1)
- Numeric: Sum permissions in each group
- 644 = rw-r--r-- (owner: rw, group: r, others: r)
- 755 = rwxr-xr-x (owner: rwx, group: rx, others: rx)

---

### Exercise 2: Change Permissions

**Task:** Use chmod to modify permissions.

**Solution:**

```bash
# Create a script
echo '#!/bin/bash\necho "Hello"' > hello.sh

# Current permissions (not executable)
ls -l hello.sh
```

**Expected Output:**
```
-rw-r--r-- 1 user user 25 Feb  7 10:00 hello.sh
```

```bash
# Make executable using symbolic mode
chmod +x hello.sh
ls -l hello.sh
```

**Expected Output:**
```
-rwxr-xr-x 1 user user 25 Feb  7 10:00 hello.sh
```

```bash
# Run the script
./hello.sh
```

**Expected Output:**
```
Hello
```

```bash
# Remove execute permission
chmod -x hello.sh
ls -l hello.sh
```

**Expected Output:**
```
-rw-r--r-- 1 user user 25 Feb  7 10:00 hello.sh
```

```bash
# Set specific permissions using numeric mode
chmod 755 hello.sh
ls -l hello.sh
```

**Expected Output:**
```
-rwxr-xr-x 1 user user 25 Feb  7 10:00 hello.sh
```

```bash
# Cleanup
rm hello.sh
```

**Explanation:**
- **Symbolic mode**: `chmod [ugoa][+-=][rwx]`
  - `chmod +x file`: Add execute for all
  - `chmod u+x file`: Add execute for owner only
  - `chmod go-w file`: Remove write for group and others
- **Numeric mode**: `chmod 755 file`
  - 7 (rwx) for owner, 5 (r-x) for group, 5 (r-x) for others
  - Calculate: r=4, w=2, x=1 (sum them)

---

### Exercise 3: Change Ownership

**Task:** Use chown and chgrp to change file ownership.

**Solution:**

```bash
# Create test file
echo "test content" > testfile.txt

# View ownership
ls -l testfile.txt
```

**Expected Output:**
```
-rw-r--r-- 1 user user 12 Feb  7 10:00 testfile.txt
```

```bash
# Try changing ownership (will fail - not root)
chown root testfile.txt
```

**Expected Output:**
```
chown: changing ownership of 'testfile.txt': Operation not permitted
```

```bash
# Must use sudo
sudo chown root testfile.txt
ls -l testfile.txt
```

**Expected Output:**
```
-rw-r--r-- 1 root user 12 Feb  7 10:00 testfile.txt
```

```bash
# Change ownership back
sudo chown $USER testfile.txt

# Change group (create a test group first)
sudo groupadd testgroup
sudo chown :testgroup testfile.txt
ls -l testfile.txt
```

**Expected Output:**
```
-rw-r--r-- 1 user testgroup 12 Feb  7 10:00 testfile.txt
```

```bash
# Change both user and group at once
sudo chown $USER:$USER testfile.txt
ls -l testfile.txt
```

**Expected Output:**
```
-rw-r--r-- 1 user user 12 Feb  7 10:00 testfile.txt
```

```bash
# Cleanup
sudo groupdel testgroup
rm testfile.txt
```

**Explanation:**
- `chown user file`: Change owner
- `chown user:group file`: Change both owner and group
- `chown :group file`: Change group only
- `chgrp group file`: Alternative way to change group
- Requires root/sudo privileges

---

### Exercise 4: Special Permissions

**Task:** Explore setuid, setgid, and sticky bit.

**Solution:**

```bash
# View setuid examples (common system commands)
ls -l /usr/bin/passwd /usr/bin/sudo
```

**Expected Output:**
```
-rwsr-xr-x 1 root root ... /usr/bin/passwd
-rwsr-xr-x 1 root root ... /usr/bin/sudo
```

**Note the 's' in owner permissions - this is setuid**

```bash
# View sticky bit on /tmp
ls -ld /tmp
```

**Expected Output:**
```
drwxrwxrwt 18 root root 4096 Feb  7 10:00 /tmp
```

**Note the 't' at the end - this is sticky bit**

```bash
# Create a test directory with sticky bit
mkdir testdir
ls -ld testdir
```

**Expected Output:**
```
drwxr-xr-x 2 user user 4096 Feb  7 10:00 testdir
```

```bash
# Add sticky bit
chmod +t testdir
ls -ld testdir
```

**Expected Output:**
```
drwxr-xr-xT 2 user user 4096 Feb  7 10:00 testdir
```

```bash
# Add setgid (useful for shared directories)
chmod g+s testdir
ls -ld testdir
```

**Expected Output:**
```
drwxr-xr-xT 2 user user 4096 Feb  7 10:00 testdir
```

**Wait, let me check properly:**
```bash
chmod g+s testdir
ls -ld testdir
```

**Expected Output:**
```
drwxr-xr-xT 2 user user 4096 Feb  7 10:00 testdir
```

Actually, the s appears in group permissions:
```bash
chmod 2775 testdir
ls -ld testdir
```

**Expected Output:**
```
drwxrwsr-x 2 user user 4096 Feb  7 10:00 testdir
```

```bash
# Cleanup
rm -rf testdir
```

**Explanation:**
- **setuid (4 or s)**: File runs with owner's permissions (not user's)
  - Example: `passwd` needs to modify /etc/shadow as root
  - Only works on files (not directories in most cases)
- **setgid (2 or s)**: Files created inherit directory's group
  - Useful for shared project directories
- **sticky bit (1 or t)**: Only file owner can delete files
  - Used on /tmp so users can't delete others' files
- **Numeric syntax**: `chmod 2755` = setgid + rwxr-xr-x
  - First digit: special permissions (4=setuid, 2=setgid, 1=sticky)

---

### Exercise 5: Default Permissions (umask)

**Task:** Understand and change umask.

**Solution:**

```bash
# View current umask
umask
```

**Expected Output:**
```
0002    # or 022
```

```bash
# Symbolic umask (more readable)
umask -S
```

**Expected Output:**
```
u=rwx,g=rx,o=rx
```

```bash
# Create a file and directory
touch testfile
mkdir testdir
ls -l testfile testdir
```

**Expected Output:**
```
-rw-r--r-- 1 user user 0 Feb  7 10:00 testfile
drwxr-xr-x 2 user user 4096 Feb  7 10:00 testdir
```

```bash
# Files: 666 - 022 = 644 (rw-r--r--)
# Directories: 777 - 022 = 755 (rwxr-xr-x)

# Change umask temporarily
umask 077
touch securefile
mkdir securedir
ls -l securefile securedir
```

**Expected Output:**
```
-rw------- 1 user user 0 Feb  7 10:01 securefile
drwx------ 2 user user 4096 Feb  7 10:01 securedir
```

```bash
# Reset umask to default
umask 022

# Cleanup
rm testfile securefile
rmdir testdir securedir
```

**Explanation:**
- **umask**: Subtracts from default permissions (666 for files, 777 for directories)
- **Default umask 022**:
  - Files: 666 - 022 = 644 (rw-r--r--)
  - Directories: 777 - 022 = 755 (rwxr-xr-x)
- **umask 077**:
  - Files: 666 - 077 = 600 (rw-------)
  - Directories: 777 - 077 = 700 (rwx------)
- Temporary change lasts only for current shell session
- Permanent change: Add to `~/.bashrc` or `~/.profile`

---

## Real-World Permission Scenarios

### Web Server Files

```bash
# Typical web directory permissions
sudo mkdir -p /var/www/html
sudo chown -R www-data:www-data /var/www/html
sudo chmod -R 755 /var/www/html
```

### Shared Project Directory

```bash
# Create shared project directory
sudo mkdir /shared-project
sudo chgrp developers /shared-project
sudo chmod 2775 /shared-project
# setgid (2) ensures all files inherit the group
# 775 = rwxrwxr-x (group members can write)
```

### Secure Script

```bash
# Script with sensitive data
echo "#!/bin/bash
mysql -u root -ppassword" > db_backup.sh

# Make executable but readable only by owner
chmod 700 db_backup.sh
ls -l db_backup.sh
```

**Expected Output:**
```
-rwx------ 1 user user ... db_backup.sh
```

---

## Instructor Notes

### Teaching Tips

1. **Permission calculator**: Use visual tools to show numeric vs symbolic modes
2. **Live demonstration**: Create files, change permissions, try to access/deny
3. **Real examples**: Show permissions on /etc/passwd, /tmp, system binaries
4. **Security emphasis**: Explain why 777 is almost always wrong
5. **Group scenarios**: Demonstrate shared directories with setgid

### Common Student Mistakes

| Mistake | Why It Happens | Fix |
|---------|----------------|-----|
| Using 777 for everything | "It just works" | Use minimum necessary permissions |
| Forgetting execute on directories | Can't cd into directory | Directories need execute (x) to enter |
| Confusing owner/group roles | Mixed up who is who | Remember: YOU=owner, YOUR group=group |
| chmod without sudo | Trying to change system files | sudo required for files you don't own |
| Ignoring umask | Strange default permissions | Set umask in ~/.bashrc |

### Extension Activities

- Set up a shared project directory with proper group permissions
- Analyze permission problems in broken systems
- Research and practice ACLs (Access Control Lists)
- Explore the difference between removing and changing ownership
- Create a permission troubleshooting guide
- Set up a web directory with proper permissions
