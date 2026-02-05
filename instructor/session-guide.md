# Session Planning Guide

## Course Structure Overview

This course consists of **10 sessions × 1.5 hours = 15 hours total**.

### Content Distribution

| Session | Content | Chapter(s) | Focus |
|---------|---------|------------|-------|
| 1 | Introduction & Philosophy | 1 | What is Linux, open source mindset |
| 2 | Installation Lab | 2 | Hands-on installation |
| 3 | GNOME Desktop | 3 | GUI navigation, settings |
| 4 | File System & Navigation | 4 | Directory structure, paths |
| 5 | CLI Fundamentals | 5 | Commands, pipes, redirection |
| 6 | Text Processing & Permissions | 6-7 | grep, sed, chmod, chown |
| 7 | Package Management | 8 | dnf/apt, repositories |
| 8 | Processes & Services | 9 | systemd, monitoring |
| 9 | Shell Scripting Basics | 10 | Automation fundamentals |
| 10 | Git, Docker & Capstone | 11-13 + Capstone | DevOps intro, final project |

## Detailed Session Plans

### Session 1: Introduction & Philosophy (Chapter 1)

**Learning Objectives:**
- Understand what Linux is and its history
- Appreciate open-source philosophy
- Know the difference between kernel and distributions
- Be excited about learning Linux

**Time Breakdown (90 minutes):**
- **10 min:** Welcome and course overview
- **15 min:** Linux history and philosophy (GNU, Richard Stallman, Linus Torvalds)
- **15 min:** What is a kernel? What is a distribution?
- **10 min:** Why Linux matters - privacy, freedom, development power
- **20 min:** Live demo - Fedora live USB exploration
- **15 min:** Q&A and course logistics
- **5 min:** Exit ticket: "What interests you most about Linux?"

**Key Concepts to Emphasize:**
- Everything is a file
- Small programs that do one thing well
- Freedom to study, modify, share
- Community-driven development

**Potential Pitfalls:**
- Students may find history boring
  - **Fix:** Connect to modern applications (Android, servers, supercomputers)
- Technical overload
  - **Fix:** Keep it high-level, save details for later

**Materials Needed:**
- Fedora live USB (3-4 for class)
- Projector
- Handout with key terminology

---

### Session 2: Installation Lab (Chapter 2)

**Learning Objectives:**
- Understand dual-boot vs virtual machine
- Successfully install Linux
- Partition disks safely
- Boot into new Linux system

**Time Breakdown (90 minutes):**
- **10 min:** Recap and installation options overview
- **15 min:** Disk partitions, dual-boot, UEFI explained
- **5 min:** Backup warnings and safety tips
- **50 min:** Guided installation (students install Linux)
- **10 min:** First boot configuration
- **10 min:** Troubleshooting common issues

**Preparation:**
- Have 3-4 bootable USB drives ready
- Prepare for dual-boot (shrink Windows partitions beforehand if possible)
- Have VM software installed as fallback
- Emergency live USBs available

**Teaching Tips:**
- Walk through installation step-by-step on projector
- Emphasize the "Erase disk" warning
- Have students work in pairs if hardware limited
- Be ready for diverse hardware issues

**Common Issues:**
- Secure Boot problems → Disable in BIOS
- Wireless not working → Have USB dongle ready
- Graphics issues → Try nomodeset first

**After Session:**
- Verify everyone can boot into Linux
- Collect any hardware issues for next session

---

### Session 3: GNOME Desktop (Chapter 3)

**Learning Objectives:**
- Navigate GNOME shell efficiently
- Customize desktop environment
- Use system settings
- Install applications from GUI

**Time Breakdown (90 minutes):**
- **10 min:** Recap and overview
- **20 min:** GNOME shell tour (activities, dash, workspaces)
- **15 min:** System settings deep dive
- **15 min:** Customizing GNOME (extensions, themes)
- **20 min:** Installing software (GNOME Software, Flatpak)
- **10 min:** Keyboard shortcuts demonstration

**Key Topics:**
- Activities overview (Super key)
- Virtual desktops/workspaces
- Notification system
- Settings categories
- GNOME Software vs CLI package managers

**Demonstration Ideas:**
- Set up custom keyboard shortcuts
- Install a GNOME extension (Dash to Dock)
- Show different themes
- Configure dark mode

**Exercise:**
- Students customize their desktop
- Set up 3-4 useful keyboard shortcuts
- Install one application via GNOME Software

---

### Session 4: File System & Navigation (Chapter 4)

**Learning Objectives:**
- Understand Linux directory structure
- Navigate using CLI
- Use absolute and relative paths
- Create, copy, move, delete files

**Time Breakdown (90 minutes):**
- **10 min:** Recap
- **15 min:** File system hierarchy explained
- **20 min:** Basic navigation commands (pwd, cd, ls)
- **15 min:** Path types (absolute, relative, ~, ., ..)
- **25 min:** File manipulation (mkdir, touch, cp, mv, rm)
- **5 min:** Summary and next session preview

**Key Commands:**
- `pwd` - Print working directory
- `cd` - Change directory
- `ls` - List files
- `mkdir` - Make directory
- `touch` - Create empty file
- `cp` - Copy
- `mv` - Move/rename
- `rm` - Remove

**Teaching Tips:**
- Draw filesystem hierarchy on board
- Use tree diagram for visualization
- Emphasize the danger of `rm`
- Practice with safe commands first

**Exercise Ideas:**
- Create a directory structure for a project
- Navigate using only relative paths
- Play "directory treasure hunt"

---

### Session 5: CLI Fundamentals (Chapter 5)

**Learning Objectives:**
- Master essential CLI commands
- Understand pipes and redirection
- Use wildcards effectively
- Read manual pages

**Time Breakdown (90 minutes):**
- **10 min:** Recap
- **15 min:** Command anatomy and flags
- **20 min:** Pipes and redirection (|, >, >>, <)
- **15 min:** Wildcards (*, ?, [])
- **20 min:** Manual pages and help
- **10 min:** Practice exercises

**Key Concepts:**
- Standard streams (stdin, stdout, stderr)
- Chaining commands with pipes
- Output redirection
- Input redirection
- Pattern matching with wildcards

**Demonstration:**
```bash
# Pipe chain example
ls -l | grep ".txt" | wc -l
# Count .txt files in current directory

# Redirection example
ls -l > filelist.txt
# Save listing to file
```

**Exercise:**
- Create a file listing
- Search for specific patterns
- Count occurrences
- Combine commands in useful ways

---

### Session 6: Text Processing & Permissions (Chapters 6-7)

**Learning Objectives:**
- Process text with grep, sed, awk
- Understand Linux permissions model
- Change permissions and ownership
- Use sudo effectively

**Time Breakdown (90 minutes):**
- **10 min:** Recap
- **20 min:** grep, head, tail, wc, sort
- **15 min:** sed basics (find and replace)
- **20 min:** Permissions explained (rwx, user/group/other)
- **15 min:** chmod, chown, sudo
- **10 min:** Practice

**Text Processing Commands:**
- `grep` - Search patterns
- `head`/`tail` - File beginning/end
- `wc` - Word/line count
- `sort` - Sort lines
- `sed` - Stream editor

**Permissions Model:**
- Read (r), Write (w), Execute (x)
- User, Group, Others
- Numeric notation (7, 5, 4, 0)

**Exercise Ideas:**
- Analyze log files
- Fix permission errors
- Create shared directories

---

### Session 7: Package Management (Chapter 8)

**Learning Objectives:**
- Understand package repositories
- Install, update, remove software
- Search for packages
- Manage dependencies

**Time Breakdown (90 minutes):**
- **10 min:** Recap
- **15 min:** What are packages and repositories?
- **25 min:** dnf commands (install, update, remove, search)
- **15 min:** APT equivalents (for Debian users)
- **15 min:** Flatpak and Snap (universal packages)
- **10 min:** Practice

**DNF Commands:**
- `sudo dnf install <package>`
- `sudo dnf update`
- `sudo dnf remove <package>`
- `dnf search <keyword>`
- `dnf info <package>`

**APT Equivalents:**
- `sudo apt install <package>`
- `sudo apt update && sudo apt upgrade`
- `sudo apt remove <package>`
- `apt search <keyword>`

**Teaching Tips:**
- Show side-by-side DNF vs APT table
- Demonstrate searching before installing
- Emphasize the `-y` flag with caution
- Show dependency resolution

**Exercise:**
- Install a useful utility (htop, tree)
- Search for packages
- Update system
- Remove unused packages

---

### Session 8: Processes & Services (Chapter 9)

**Learning Objectives:**
- View running processes
- Monitor system resources
- Control processes (start, stop, kill)
- Understand systemd services

**Time Breakdown (90 minutes):**
- **10 min:** Recap
- **20 min:** Process monitoring (ps, top, htop)
- **15 min:** Controlling processes (kill, pkill, killall)
- **20 min:** systemd services
- **15 min:** Managing services
- **10 min:** Practice

**Key Commands:**
- `ps` - Process snapshot
- `top`/`htop` - Interactive monitoring
- `kill` - Send signals to processes
- `systemctl` - Control systemd services

**Demonstration:**
- Start a background process
- Find its PID
- Kill it gracefully
- Check system service status

**Exercise:**
- Monitor system resources
- Start/stop a service
- Kill a frozen application
- Check boot time

---

### Session 9: Shell Scripting Basics (Chapter 10)

**Learning Objectives:**
- Write simple shell scripts
- Use variables and arguments
- Create conditional logic
- Automate repetitive tasks

**Time Breakdown (90 minutes):**
- **10 min:** Recap
- **15 min:** What is scripting and why automate?
- **20 min:** Variables, arguments, basic syntax
- **20 min:** Conditionals (if/else)
- **15 min:** Loops (for, while)
- **10 min:** Writing first script

**Script Example:**
```bash
#!/bin/bash
# My first script

echo "Hello, $USER!"
today=$(date +%Y-%m-%d)
echo "Today is $today"
```

**Teaching Tips:**
- Start with simple examples
- Show common mistakes (missing #!)
- Demonstrate script execution
- Emphasize the power of automation

**Exercise:**
- Create a backup script
- Write a system info script
- Automate a daily task

---

### Session 10: Git, Docker & Capstone (Chapters 11-13 + Capstone)

**Learning Objectives:**
- Understand basic Git workflow
- Know what containers are
- Apply all learned skills
- Complete capstone project

**Time Breakdown (90 minutes):**
- **10 min:** Recap
- **15 min:** Git basics (init, add, commit, status)
- **15 min:** Docker introduction
- **30 min:** Capstone project work
- **15 min:** Final review and celebration
- **5 min:** Course feedback

**Git Commands:**
- `git init`
- `git add`
- `git commit`
- `git status`
- `git log`

**Docker Concepts:**
- Images vs containers
- Basic commands: run, ps, stop
- Why containers matter

**Capstone Project:**
- Personal Linux server setup
- Demonstrate all learned skills
- Document the process

---

## Teaching Strategies

### For Each Session

**Before Class:**
1. Review content and exercises
2. Prepare demonstration commands
3. Test all examples
4. Have backup plans ready

**During Class:**
1. Start with recap
2. Explain concepts clearly
3. Demonstrate live
4. Guide practice
5. Check understanding

**After Class:**
1. Note what worked/didn't work
2. Adjust next session if needed
3. Answer questions via email/forum
4. Prepare for next session

### Handling Different Skill Levels

**Advanced Students:**
- Provide bonus exercises
- Let them explore ahead
- Pair them with beginners
- Give them "teacher assistant" roles

**Struggling Students:**
- Provide extra examples
- Pair with peer mentor
- Offer one-on-one time
- Simplify exercises if needed

### Keeping Engagement High

**Ideas:**
- Live coding demonstrations
- Interactive exercises
- Pair programming
- "Show and tell" sessions
- Real-world examples
- Linux news updates
- Terminal games

---

## Assessment Schedule

| Session | Assessment Type | Time |
|---------|----------------|------|
| 2 | Installation check | During lab |
| 4 | File system exercise | 10 min |
| 6 | Text processing quiz | 10 min |
| 8 | Process management practical | 15 min |
| 10 | Capstone presentation | 20 min |

---

## Flexibility Notes

### If Time Runs Short
- Skip optional topics
- Move exercises to homework
- Focus on core concepts
- Provide written materials

### If Extra Time Available
- Bonus demonstrations
- Advanced topics
- Student questions
- Practice time
- Linux customization

### Adapting to Student Interest
- More graphics → Focus on drivers, GPU setup
- More programming → Emphasize dev tools, scripting
- More security → Cover permissions, encryption in depth
- More administration → Add systemd, networking depth

---

## Materials Checklist

### Per Session
- [ ] Lesson plan reviewed
- [ ] Demonstrations tested
- [ ] Exercises prepared
- [ ] Quiz materials ready
- [ ] Backup activities planned

### Physical Materials
- [ ] Bootable USB drives (Session 2)
- [ ] Printed cheat sheets (optional)
- [ ] Whiteboard/markers
- [ ] Projector working

### Digital Materials
- [ ] Course materials built
- [ ] Demo environment ready
- [ ] Backup of all materials
- [ ] Access to online docs

---

## Emergency Procedures

### Technical Failures
- Internet down → Use offline package cache
- Projector broken → Use terminal only (opportunity!)
- Student laptop broken → Pair with another student

### Time Issues
- Running behind → Skip advanced topics
- Running ahead → Bonus content, Q&A
- Session cancelled → Adjust future sessions

### Student Issues
- Lost data → Help with recovery, use as teaching moment
- Broke system → Live USB rescue, reinstall
- Not understanding → Extra examples, one-on-one help

---

## Feedback and Improvement

After each session:
1. What worked well?
2. What confused students?
3. What should be changed?
4. What needs more time?

After course completion:
1. Collect student feedback
2. Review assessments
3. Update materials
4. Share insights with other instructors
