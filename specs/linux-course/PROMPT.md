# Linux Course Implementation Prompt

## Objective

Create a comprehensive, well-structured mdBook course that teaches Linux from beginner to intermediate level. The course should make students love Linux through its development power, privacy/open-source philosophy, and customization capabilities.

## Course Specifications

### Target Audience
- University students, complete beginners to Linux
- Familiar with basic programming concepts
- Currently using Windows, no CLI experience

### Course Format
- **Duration:** 10 sessions × 1.5 hours (15 hours total)
- **Students:** ~15 per class
- **Distribution:** Fedora/Debian with GNOME desktop
- **Installation:** Cover dual-boot (preferred) and Virtual Machine options

### Learning Outcomes
By the end, students must:
- Comfortably use Linux as their daily driver
- Master the CLI (Command Line Interface)
- Perform system administration tasks
- Use Git and Docker at an introductory level

### Teaching Approach
- Real-time in-session coding/CLI exercises
- Theory/practice balance based on content
- Optional homework with bonus points
- Session-based exercises that merge into a capstone project
- Include a "wow moment" session showcasing Linux capabilities
- Assessment via quizzes

### Capstone Project
Complete Linux setup + system administration task

## mdBook Requirements

### Structure
- Organize by **topic** (not strictly by session), fitting 10-12 sessions
- Each chapter must include:
  - Learning objectives
  - Prerequisites
  - Clear explanations with examples
  - Code blocks with syntax highlighting (bash, markdown, yaml, etc.)
  - Visual elements (diagrams, screenshots, terminal outputs)
  - Summary
  - Exercises (with clear expected output)

### Style Guidelines
- Mix tone based on content: formal for technical topics, conversational for concepts
- Clean, well-structured, easy to understand and explain
- Include a cheat sheet section at the end

### Core Topics to Cover
1. **Introduction & Philosophy** — What is Linux, open-source, why it matters
2. **Installation** — Dual-boot, VM setup, Fedora/Debian specifics
3. **GNOME Desktop** — Basic navigation, customization, settings
4. **File System & Navigation** — Directory structure, paths, permissions
5. **CLI Fundamentals** — Commands, pipes, redirection, wildcards
6. **Package Management** — dnf/apt, installing software, repositories
7. **Users & Permissions** — sudo, useradd, chmod, chown
8. **Processes & Services** — systemd, monitoring, troubleshooting
9. **Shell Scripting Basics** — Variables, loops, functions, automation
10. **Git for Development** — Basic version control workflows
11. **Docker Introduction** — Containers, basic commands
12. **System Administration Capstone** — Combining all skills

### Visual Elements Required
- ASCII diagrams for file system structure
- Mermaid diagrams for workflows (e.g., Git workflow)
- Terminal screenshots showing before/after states
- Code comparison tables (GUI vs CLI equivalents)

## Acceptance Criteria (Given-When-Then)

**GIVEN** a complete beginner university student
**WHEN** they complete all 10 sessions and exercises
**THEN** they can:
- Navigate and control Linux entirely from CLI
- Install and manage software independently
- Troubleshoot common system issues
- Write basic shell scripts for automation
- Use Git for version control
- Understand and run Docker containers
- Be excited to use Linux as their daily OS

## Reference Materials

All detailed requirements and research are in: `specs/linux-course/`

---

## Suggested Ralph Commands

Full pipeline with research integration:
```bash
ralph run --config presets/pdd-to-code-assist.yml
```

Simpler spec-driven flow:
```bash
ralph run --config presets/spec-driven.yml
```
