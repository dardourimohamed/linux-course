# Final Exam: System Administration & DevOps Introduction

---

## Part 1: Multiple Choice (15 questions, 1 point each = 15 points)

### Chapter 8 — Package Management

**1. Which command installs a package on Fedora?**
- A) apt install
- B) dnf install
- C) yum install
- D) snap install

**2. What does `apt update` do?**
- A) Upgrades all installed packages
- B) Removes unused packages
- C) Refreshes the list of available packages
- D) Fixes broken dependencies

**3. Which command removes a package AND its configuration files on Debian?**
- A) apt remove
- B) apt delete
- C) apt purge
- D) apt clean

### Chapter 9 — Processes & Services

**4. Which command shows a real-time, continuously updating view of running processes?**
- A) ps
- B) top
- C) jobs
- D) pstree

**5. What does the command `kill -9 1234` do?**
- A) Sends SIGTERM to process 1234 (graceful stop)
- B) Sends SIGKILL to process 1234 (force stop)
- C) Pauses process 1234
- D) Restarts process 1234

**6. Which `systemctl` command makes a service start automatically at boot?**
- A) systemctl start nginx
- B) systemctl enable nginx
- C) systemctl boot nginx
- D) systemctl init nginx

### Chapter 10 — Shell Scripting

**7. What does the first line `#!/bin/bash` in a script do?**
- A) Comments out the line
- B) Tells the system to use the Bash interpreter
- C) Creates a new Bash session
- D) Installs Bash if not present

**8. In a shell script, what does `$1` refer to?**
- A) The script's name
- B) The first argument passed to the script
- C) The exit code of the last command
- D) The number of arguments

**9. How do you make a script executable?**
- A) chmod 755 script.sh
- B) exec script.sh
- C) bash --exec script.sh
- D) sudo script.sh

### Chapter 11 — Networking Basics

**10. Which command shows your machine's IP addresses?**
- A) ip addr
- B) ip show
- C) ifconfig -a
- D) netstat -i

**11. What is the default SSH port?**
- A) 21
- B) 22
- C) 80
- D) 443

### Chapter 12 — Git Version Control

**12. What is the correct sequence for saving changes in Git?**
- A) git commit → git add → git push
- B) git add → git commit → git push
- C) git push → git add → git commit
- D) git save → git push

**13. Which command creates AND switches to a new branch?**
- A) git branch new-feature
- B) git switch new-feature
- C) git checkout -b new-feature
- D) git new-branch new-feature

### Chapter 13 — Docker Containers

**14. How do containers differ from virtual machines?**
- A) Containers include their own full operating system
- B) Containers share the host's kernel and are more lightweight
- C) Containers are slower than VMs
- D) Containers can only run Linux applications

**15. Which Dockerfile instruction sets the default command to run when a container starts?**
- A) RUN
- B) EXEC
- C) CMD
- D) START

---

## Part 2: Short Answer (3 questions, 5 points total)

**16.** (1 point) What is the difference between `systemctl start` and `systemctl enable`?

**17.** (2 points) Explain what a merge conflict is in Git and how you resolve it.

**18.** (2 points) You need to install the `htop` package, check that the `nginx` service is running, and view its recent logs. Write the three commands for a Fedora system.
