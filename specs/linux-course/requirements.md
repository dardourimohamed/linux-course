# Requirements Clarification

This document records the Q&A process for refining the Linux course requirements.

## Questions and Answers

### Q1: Target Audience and Background

**Question:** Who is the primary target audience for this Linux course? Specifically:
- What is their current technical background? (Complete beginners, some programming experience, IT professionals, etc.)
- What is their goal for learning Linux? (System administration, development, DevOps, personal interest, certification preparation, etc.)
- Are there any age groups or educational contexts to consider? (University students, professionals, self-learners, etc.)

**Answer:**
- **Current technical background:** Complete beginners
- **Goal:** Make them love Linux and be able to use it smoothly for personal use, development, and some system administration
- **Educational context:** University students

### Q2: Prior Knowledge Assumptions

**Question:** Since they are complete beginners to Linux, what existing knowledge can we assume?
- Do they have any prior experience with command-line interfaces (CLI)?
- Are they familiar with basic programming concepts?
- What operating system are they currently using? (Windows, macOS, mobile-only, etc.)
- Do they understand fundamental computing concepts (files, directories, processes, etc.)?

**Answer:**
- **CLI experience:** None
- **Programming concepts:** Familiar with basic programming concepts
- **Current OS:** Windows
- **Computing fundamentals:** Basic understanding (files, directories, processes)

### Q3: Learning Outcomes by Session

**Question:** Given 10 sessions of 1.5 hours (15 total), what should students be able to do by the end of the course? Specifically:
- Should they be comfortable using Linux as their daily driver OS by the end?
- What specific "hard skills" are must-haves? (e.g., navigate CLI, manage users, write bash scripts, install software, troubleshooting)
- Should the course cover installation/dual-boot setup, or assume they have access to a Linux environment (WSL, VM, etc.)?
- Are there any specific tools or technologies you want to ensure are covered? (git, docker, vim/nano, specific DEs, etc.)

**Answer:**
- **Daily driver comfort:** Yes, they should be comfortable using Linux as their daily driver by the end
- **Must-have skills:** Master the CLI (Command Line Interface)
- **Installation:** Course should cover Linux installation
- **Specific tools:** Git and Docker (at an introductory level)

### Q4: Session Structure and Hands-on Approach

**Question:** To design effective 1.5-hour sessions, I need to understand the teaching format:
- What should be the balance between theory and hands-on practice? (e.g., 30 min theory + 60 min practice?)
- Should each session have homework or follow-along exercises?
- Do you want in-session coding/CLI exercises where students follow along in real-time?
- Should there be a capstone project across sessions, or individual session-based exercises?
- What's the expected class size? (This affects how hands-on we can make it)

**Answer:**
- **Theory/practice balance:** Smartly split based on content (flexible)
- **Homework:** Optional exercises with bonus points
- **In-session practice:** Yes, real-time coding/CLI exercises
- **Project structure:** Individual session-based exercises that merge into a capstone project
- **Class size:** ~15 students

### Q5: Distribution and Linux Environment

**Question:** For the installation and practical sessions:
- Which Linux distribution should we focus on? (Ubuntu, Fedora, Debian, Arch, distro-agnostic approach?)
- Should we cover different desktop environments (GNOME, KDE) or stick to one?
- For the installation session, should we cover: dual-boot, virtual machine (VirtualBox/VMware), WSL, or all options?
- Should students be required to have Linux installed before the course, or will Session 1 cover installation?
- What's the recommended minimum hardware specifications we should communicate?

**Answer:**
- **Distribution:** Fedora or Debian (course should be applicable to both)
- **Desktop Environment:** Stick to GNOME
- **Installation methods:** Dual-boot (preferred) or Virtual Machine
- **Prerequisites:** Linux installation not required before course
- **Hardware specs:** None specified (no minimum requirements to communicate)

### Q6: Assessment, Capstone, and "Love Linux" Factor

**Question:** You mentioned making students "love Linux" — this is important for engagement:
- What aspects of Linux do you think will most excite this audience? (Customization, privacy/open-source, development power, gaming, community, etc.)
- For the capstone project, what should it demonstrate? (A complete setup? A deployed application? A system administration task?)
- How will we measure success beyond "they can use the CLI"? (Presentations, final project showcase, quiz scores?)
- Should there be a "wow moment" session where we show off Linux capabilities to build enthusiasm?
- Any specific success criteria or attendance policy we should plan around?

**Answer:**
- **Excitement factors:** Development power, privacy/open-source, and customization
- **Capstone:** A complete setup AND a system administration task
- **Success measurement:** Quiz scores
- **Wow moment:** Yes, include a session showcasing Linux capabilities
- **Success criteria:** None specified

### Q7: mdBook Format and Course Materials

**Question:** For the mdBook course materials:
- Should the book be structured exactly like the 10 sessions, or organized differently (e.g., by topic, progressive difficulty)?
- Do you want code blocks with syntax highlighting? If so, what languages? (bash, markdown, yaml, etc.)
- Should each chapter/session have: Learning objectives, prerequisites, summary, exercises?
- Do you want visual elements included? (Diagrams, screenshots, terminal output examples)
- Should there be a "quick reference" or "cheat sheet" section at the end?
- Any preference for the tone/style? (Formal, conversational, humorous, concise?)

**Answer:**
- **Structure:** Organized by topic, but fits into 10-12 sessions
- **Code blocks:** Yes, with syntax highlighting
- **Chapter structure:** Yes — learning objectives, prerequisites, summary, exercises (with clear output)
- **Visual elements:** Yes — diagrams, screenshots, terminal output examples
- **Cheat sheet:** Yes, include at the end
- **Tone/style:** Mixed based on content (flexible)

---

## Requirements Summary

### Target Audience
- University students, complete beginners to Linux
- Familiar with basic programming concepts
- Currently using Windows
- No CLI experience

### Course Goals
- Make them love Linux (via: development power, privacy/open-source, customization)
- Enable comfortable daily driver use
- Master the CLI
- Cover installation
- Introduce Git and Docker

### Course Format
- 10 sessions × 1.5 hours (15 total hours)
- ~15 students
- Fedora/Debian with GNOME
- Theory/practice split based on content
- Real-time in-session coding/CLI exercises
- Optional homework with bonus points
- Session-based exercises merging into capstone project
- Success measured via quiz scores
- Include a "wow moment" showcase session

### Capstone
- Complete setup + system administration task

### mdBook Structure
- Topic-based organization (fits 10-12 sessions)
- Code blocks with syntax highlighting
- Chapters: objectives, prerequisites, summary, exercises, clear output
- Visual elements: diagrams, screenshots, terminal outputs
- Cheat sheet section
- Flexible tone/style
