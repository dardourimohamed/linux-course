# Linux Course Implementation Plan

## Progress Checklist

- [ ] Step 1: Project setup and mdBook initialization
- [ ] Step 2: Create book structure and SUMMARY.md
- [ ] Step 3: Write Part I - Foundations (Chapters 1-3)
- [ ] Step 4: Write Part II - CLI Mastery (Chapters 4-7)
- [ ] Step 5: Write Part III - System Administration (Chapters 8-11)
- [ ] Step 6: Write Part IV - DevOps Introduction (Chapters 12-13)
- [ ] Step 7: Create Capstone chapter
- [ ] Step 8: Create Appendices (Cheat Sheet, Command Reference, Troubleshooting)
- [ ] Step 9: Add visual elements (screenshots, diagrams, terminal outputs)
- [ ] Step 10: Build and verify mdBook output
- [ ] Step 11: Create instructor materials (quizzes, exercise solutions)
- [ ] Step 12: Final review and polish

---

## Step 1: Project Setup and mdBook Initialization

**Objective:** Set up the development environment and create the basic mdBook project structure.

**Implementation Guidance:**
1. Create project directory `linux-course/`
2. Install mdBook: `cargo install mdbook` or use package manager
3. Initialize mdBook: `mdbook init`
4. Configure `book.toml` with:
   - Title: "Linux for Everyone"
   - Authors: [Your Name]
   - Description: "A comprehensive Linux course from beginner to intermediate"
   - Language: en
5. Create subdirectories: `images/`, `exercises/`
6. Set up Git repository and initial commit

**Test Requirements:**
- Running `mdbook build` produces no errors
- `mdbook serve` displays the default book in browser
- Directory structure matches design

**Integration Notes:**
This creates the foundation for all content. No content yet, but structure is ready.

**Demo Description:** Empty book loads in browser with table of contents showing placeholder chapters.

---

## Step 2: Create Book Structure and SUMMARY.md

**Objective:** Define the complete table of contents reflecting the 5-part structure.

**Implementation Guidance:**
1. Create `book/SUMMARY.md` with:
   ```markdown
   # Summary

   [Introduction](./intro.md)

   ---

   # Part I: Foundations

   - [Chapter 1: What is Linux?](./part-1-foundations/chapter-01-philosophy.md)
   - [Chapter 2: Installation](./part-1-foundations/chapter-02-installation.md)
   - [Chapter 3: The GNOME Desktop](./part-1-foundations/chapter-03-gnome.md)

   # Part II: CLI Mastery

   - [Chapter 4: File System & Navigation](./part-2-cli/chapter-04-filesystem.md)
   - [Chapter 5: CLI Fundamentals](./part-2-cli/chapter-05-cli-basics.md)
   - [Chapter 6: Text Processing & Redirection](./part-2-cli/chapter-06-text-processing.md)
   - [Chapter 7: Permissions & Users](./part-2-cli/chapter-07-permissions.md)

   # Part III: System Administration

   - [Chapter 8: Package Management](./part-3-sysadmin/chapter-08-packages.md)
   - [Chapter 9: Processes & Services](./part-3-sysadmin/chapter-09-processes.md)
   - [Chapter 10: Shell Scripting Basics](./part-3-sysadmin/chapter-10-scripting.md)
   - [Chapter 11: Networking Basics](./part-3-sysadmin/chapter-11-networking.md)

   # Part IV: DevOps Introduction

   - [Chapter 12: Git Version Control](./part-4-devops/chapter-12-git.md)
   - [Chapter 13: Docker Containers](./part-4-devops/chapter-13-docker.md)

   # Capstone Project

   - [Personal Linux Server](./capstone.md)

   ---

   # Appendices

   - [Cheat Sheet](./appendices/cheat-sheet.md)
   - [Command Reference](./appendices/command-reference.md)
   - [Troubleshooting Guide](./appendices/troubleshooting.md)
   ```
2. Create all placeholder markdown files with basic headers
3. Create directory structure matching SUMMARY.md

**Test Requirements:**
- `mdbook build` completes without errors
- All chapters are accessible in navigation
- No broken links in table of contents

**Integration Notes:**
Skeleton is now complete. Each step will fill in one or more chapters.

**Demo Description:** Full table of contents visible, all chapters show placeholder content.

---

## Step 3: Write Part I - Foundations (Chapters 1-3)

**Objective:** Create content for the foundational chapters covering philosophy, installation, and GNOME desktop.

**Implementation Guidance:**

### Chapter 1: What is Linux?
- Learning objectives: Understand open-source, Linux history, why it matters
- Content: Philosophy, kernel vs distributions, Linux ecosystem
- "Wow factors": Customization, privacy, development power
- Exercises: Research a distro, identify use cases

### Chapter 2: Installation
- Learning objectives: Dual-boot or VM installation
- Content:
  - Creating bootable USB
  - Partitioning basics
  - Fedora (Anaconda) vs Debian (Calamares) installers
  - Post-installation setup
- Screenshots: Each major step
- Exercises: VM lab, partition planning worksheet

### Chapter 3: The GNOME Desktop
- Learning objectives: Navigate and customize GNOME
- Content:
  - Activities overview, workspaces
  - Settings, GNOME Tweaks
  - Extensions, themes
  - Customization "wow moment"
- Screenshots: Before/after customization
- Exercises: Personalize desktop, install extensions

**Test Requirements:**
- Each chapter has learning objectives, content, examples, summary, exercises
- Commands work on both Fedora and Debian
- Screenshots are clear and current
- Exercises have expected outputs

**Integration Notes:**
Students can now install Linux and navigate the desktop. Ready for CLI introduction.

**Demo Description:** Complete first 3 chapters with working installation guide and GNOME customization tutorial.

---

## Step 4: Write Part II - CLI Mastery (Chapters 4-7)

**Objective:** Create content for CLI fundamentals, the core of the course.

**Implementation Guidance:**

### Chapter 4: File System & Navigation
- Learning objectives: Understand Linux filesystem, navigate directories
- Content:
  - Filesystem hierarchy (/home, /etc, /var, etc.)
  - `pwd`, `ls`, `cd` with all options
  - Tab completion
  - Relative vs absolute paths
- Visual: ASCII filesystem tree
- Exercises: Navigate to specific locations, create practice directory

### Chapter 5: CLI Fundamentals
- Learning objectives: Master essential file operations
- Content:
  - `mkdir`, `rmdir`, `touch`, `rm`, `cp`, `mv`
  - Wildcards: `*`, `?`, `[]`
  - `man` pages and `--help`
- Visual: Before/after terminal screenshots
- Exercises: Create directory structure, move files, practice wildcards

### Chapter 6: Text Processing & Redirection
- Learning objectives: Read and manipulate text, chain commands
- Content:
  - `cat`, `less`, `head`, `tail`
  - Pipes `|`, redirection `>`, `>>`
  - `grep` basics
  - `find` introduction
- Visual: Data flow diagrams
- Exercises: Filter logs, combine commands, search files

### Chapter 7: Permissions & Users
- Learning objectives: Understand and manage permissions
- Content:
  - `ls -l` output explained
  - `chmod` (symbolic and octal)
  - `chown`, `chgrp`
  - `sudo` and root
  - `useradd`, `usermod`, `passwd`
- Visual: Permission table, user types
- Exercises: Fix permission errors, create users, practice sudo

**Test Requirements:**
- All 20 core commands covered with examples
- DNF/APT equivalence noted where relevant
- Each command has syntax, description, example
- Exercises build on each other

**Integration Notes:**
Students are now CLI proficient. Can handle system administration tasks.

**Demo Description:** Complete CLI mastery section with 49 cumulative commands.

---

## Step 5: Write Part III - System Administration (Chapters 8-11)

**Objective:** Create content for system administration topics.

**Implementation Guidance:**

### Chapter 8: Package Management
- Learning objectives: Install and manage software
- Content:
  - DNF/APT equivalence table (prominently displayed)
  - `search`, `install`, `remove`, `update`, `upgrade`
  - Repository concept
  - RPM Fusion / contrib non-free
- Visual: Package manager comparison table
- Exercises: Install software, remove it, search for packages

### Chapter 9: Processes & Services
- Learning objectives: Understand and control running processes
- Content:
  - `ps`, `top`, `htop`
  - `kill`, `pkill`
  - `systemctl` (start, stop, enable, status, journalctl)
  - Service concept
- Visual: Process state diagram
- Exercises: Kill a process, enable a service, view logs

### Chapter 10: Shell Scripting Basics
- Learning objectives: Automate tasks with scripts
- Content:
  - Shebang, variables
  - Input/output (`read`, `echo`)
  - Conditionals (`if`, `test`)
  - Loops (`for`, `while`)
  - Functions
- Visual: Script template
- Exercises: Write backup script, create menu system

### Chapter 11: Networking Basics
- Learning objectives: Understand basic networking
- Content:
  - IP addresses, ports
  - `ping`, `ss` or `netstat`
  - SSH basics
  - Firewall concept (firewalld/ufw mention)
- Visual: Network diagram
- Exercises: Ping a server, connect via SSH

**Test Requirements:**
- System administration concepts clearly explained
- Commands are safe for beginners to practice
- Scripts are functional and well-commented
- Networking basics are practical

**Integration Notes:**
Students can now administer their systems. Ready for development tools.

**Demo Description:** Complete sysadmin section with practical automation tasks.

---

## Step 6: Write Part IV - DevOps Introduction (Chapters 12-13)

**Objective:** Create introductory content for Git and Docker.

**Implementation Guidance:**

### Chapter 12: Git Version Control
- Learning objectives: Use Git for basic version control
- Content:
  - What is version control
  - 10 essential commands:
    - `git init`, `clone`, `status`, `add`, `commit`
    - `log`, `diff`, `branch`, `checkout`, `pull`, `push`
  - Basic workflow diagram
  - `.gitignore` introduction
- Visual: Git workflow (Mermaid diagram)
- Exercises: Create repo, commit changes, view history

### Chapter 13: Docker Containers
- Learning objectives: Understand and run Docker containers
- Content:
  - Containers vs VMs
  - 6 essential commands:
    - `docker run`, `ps`, `images`, `stop`, `rm`, `pull`
  - Images, containers, registry concepts
  - Basic nginx example
- Visual: Docker concepts diagram
- Exercises: Run nginx container, serve custom HTML

**Test Requirements:**
- Git commands are clearly explained with examples
- Docker concepts are simplified for beginners
- Both tools connect to Linux CLI skills
- Exercises are doable in one session

**Integration Notes:**
Students have complete toolset for capstone project.

**Demo Description:** Complete DevOps introduction with practical exercises.

---

## Step 7: Create Capstone Chapter

**Objective:** Create the integrating final project chapter.

**Implementation Guidance:**

### Personal Linux Server Project

**Structure:**
1. **Project Overview**
   - Goal: Set up a personal web server with automation
   - Skills demonstrated: All course topics
   - Estimated time: 2-3 hours

2. **Phase 1: Web Server Setup**
   - Install Apache/nginx
   - Start and enable service
   - Configure firewall
   - Create custom HTML page
   - Test from another device

3. **Phase 2: Automation**
   - Create backup script for web files
   - Set up automated backup (cron or systemd timer)
   - Test restoration

4. **Phase 3: Version Control**
   - Initialize Git repo
   - Commit web files and scripts
   - Create README documenting the setup

5. **Deliverables**
   - Working web server
   - Automated backups
   - Git repository with documentation
   - Presentation/demo

6. **Assessment Rubric**
   - Installation (10 points)
   - Services (20 points)
   - Automation (20 points)
   - Documentation (20 points)
   - CLI skills (20 points)
   - Problem-solving (10 points)

**Test Requirements:**
- Project uses skills from all chapters
- Steps are clear and achievable
- Rubric is objective
- Troubleshooting hints included

**Integration Notes:**
Capstone integrates all learning. Students demonstrate mastery.

**Demo Description:** Complete project guide with rubric and example submission.

---

## Step 8: Create Appendices

**Objective:** Create reference materials for ongoing use.

**Implementation Guidance:**

### Cheat Sheet (appendices/cheat-sheet.md)
- Single-page reference organized by topic
- Navigation, Files, Permissions, Packages, Processes, Git, Docker
- Include both DNF and APT commands
- Visual: Color-coded sections

### Command Reference (appendices/command-reference.md)
- Alphabetical list of all commands
- Each with: syntax, description, common flags, example
- Cross-references to chapters

### Troubleshooting Guide (appendices/troubleshooting.md)
- Common errors by category
- Debug commands
- Where to get help
- Community resources

**Test Requirements:**
- Cheat sheet fits on one page (printed)
- Command reference is complete
- Troubleshooting covers common issues

**Integration Notes:**
Reference materials support ongoing learning after course.

**Demo Description:** Complete appendices for student reference.

---

## Step 9: Add Visual Elements

**Objective:** Enhance content with screenshots, diagrams, and terminal outputs.

**Implementation Guidance:**

1. **Screenshots**
   - Create `images/session01/`, `images/session02/`, etc.
   - Capture: Installation steps, GNOME before/after, terminal outputs
   - Use consistent resolution and styling
   - Add descriptive alt text

2. **Diagrams**
   - Mermaid diagrams for:
     - Filesystem hierarchy
     - Git workflow
     - Docker concepts
     - Process states
   - ASCII art for simple structures

3. **Terminal Output Examples**
   - Real output from commands
   - Before/after comparisons
   - Error examples with solutions

4. **Tables**
   - DNF vs APT command equivalence
   - Permission modes (rwx)
   - File types
   - Process states

**Test Requirements:**
- All images load correctly
- Diagrams render in mdBook
- Terminal output is accurate
- Visuals enhance understanding

**Integration Notes:**
Visual elements make content accessible and engaging.

**Demo Description:** Fully illustrated course with professional visual elements.

---

## Step 10: Build and Verify mdBook Output

**Objective:** Generate and test the complete mdBook.

**Implementation Guidance:**

1. **Build the book**
   ```bash
   mdbook build
   ```

2. **Verify output**
   - Check `dist/index.html` in browser
   - Test navigation between chapters
   - Verify code highlighting
   - Check search functionality
   - Test print/PDF export

3. **Quality checks**
   - No broken internal links
   - All images load
   - Tables render correctly
   - Mermaid diagrams display
   - Code blocks have syntax highlighting

4. **Theme customization** (optional)
   - Create `theme/` directory
   - Override CSS for exercise boxes
   - Add custom colors for DNF/APT distinction

**Test Requirements:**
- Book builds without errors
- All chapters are accessible
- Navigation works correctly
- Print layout is readable
- Search returns relevant results

**Integration Notes:**
Course is now deliverable as HTML, PDF, or via Git hosting.

**Demo Description:** Complete, built mdBook ready for distribution.

---

## Step 11: Create Instructor Materials

**Objective:** Create supporting materials for course delivery.

**Implementation Guidance:**

1. **Quizzes** (instructor/quizzes/)
   - 10 quiz files, one per session
   - Mix of multiple choice, true/false, short answer
   - Answer keys included

2. **Exercise Solutions** (instructor/solutions/)
   - Complete solutions for all exercises
   - Alternative approaches where applicable
   - Common mistakes and how to avoid them

3. **Session Slides** (instructor/slides/)
   - One slide deck per session (optional)
   - Key concepts, commands, examples
   - Exportable as PDF

4. **Setup Guide** (instructor/setup.md)
   - Classroom preparation checklist
   - Demo machine setup
   - Common student issues and fixes

**Test Requirements:**
- Quiz answers are correct
- Solutions are reproducible
- Slides cover key points
- Setup guide is comprehensive

**Integration Notes:**
Instructor materials enable effective course delivery.

**Demo Description:** Complete instructor resource package.

---

## Step 12: Final Review and Polish

**Objective:** Ensure quality and completeness.

**Implementation Guidance:**

1. **Content review**
   - Check all commands work on both Fedora and Debian
   - Verify all exercises have expected outputs
   - Ensure consistent tone and style
   - Check for typos and errors

2. **Cross-reference check**
   - All chapter links work
   - Command reference is complete
   - Appendices reference main chapters

3. **Accessibility check**
   - All images have alt text
   - Code is readable (contrast)
   - Tables have headers

4. **Final build**
   - Clean build: `rm -rf dist/ && mdbook build`
   - Test in multiple browsers
   - Verify PDF export

5. **Documentation**
   - Update README.md with:
     - Course overview
     - Build instructions
     - Usage guidelines
     - Contribution guide

**Test Requirements:**
- Zero build errors
- Zero broken links
- All content proofread
- Multiple browser compatibility
- Professional appearance

**Integration Notes:**
Course is production-ready.

**Demo Description:** Final, polished Linux course mdBook.

---

## Completion Checklist

- [ ] All 13 chapters written and reviewed
- [ ] Capstone project complete with rubric
- [ ] All 3 appendices complete
- [ ] Screenshots captured and integrated
- [ ] Diagrams created and rendering
- [ ] Instructor materials complete
- [ ] mdBook builds successfully
- [ ] Tested on Fedora
- [ ] Tested on Debian
- [ ] Documentation complete

## Next Steps After Implementation

1. **Host the book**
   - GitHub Pages
   - GitLab Pages
   - Self-hosted web server

2. **Distribute**
   - Link for students
   - PDF for offline use
   - Print-on-demand option

3. **Iterate**
   - Gather student feedback
   - Update based on issues
   - Add advanced topics for future version
