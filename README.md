# Linux for Everyone

A comprehensive mdBook-based Linux course designed for university students with no prior Linux experience.

## Overview

This course takes students from complete beginners to confident daily Linux users through 10 sessions (15 hours total). The curriculum covers:

- **Part I: Foundations** - Philosophy, installation, GNOME desktop
- **Part II: CLI Mastery** - Filesystem, commands, text processing, permissions
- **Part III: System Administration** - Packages, processes, scripting, networking
- **Part IV: DevOps Introduction** - Git version control, Docker containers
- **Capstone Project** - Personal Linux server setup

## Target Audience

- University students with no Linux experience
- Familiar with basic programming concepts
- Currently using Windows, no CLI experience

## Distributions Covered

- **Fedora** (using DNF package manager)
- **Debian** (using APT package manager)

Both distributions use the GNOME desktop environment.

## Building the Book

### Prerequisites

1. Install Rust and Cargo
2. Install mdBook:
   ```bash
   cargo install mdbook
   ```
3. Install mdbook-mermaid (for diagrams):
   ```bash
   cargo install mdbook-mermaid
   ```

### Build

```bash
# Build the static HTML
mdbook build

# Or serve locally for live preview
mdbook serve
```

The built book will be in the `dist/` directory.

### View

Open `dist/index.html` in a web browser, or run:
```bash
mdbook serve --open
```

## Course Structure

| Session | Topic | Duration |
|---------|-------|----------|
| 1 | What is Linux? | 1.5 hours |
| 2 | Installation | 1.5 hours |
| 3 | GNOME Desktop | 1.5 hours |
| 4 | File System & Navigation | 1.5 hours |
| 5 | CLI Fundamentals | 1.5 hours |
| 6 | Text Processing & Redirection | 1.5 hours |
| 7 | Permissions & Users | 1.5 hours |
| 8 | Package Management | 1.5 hours |
| 9 | Processes & Services | 1.5 hours |
| 10 | Shell Scripting + DevOps (Git/Docker) | 1.5 hours |

## Learning Outcomes

By the end of this course, students will be able to:

- Install and use Linux as their daily operating system
- Navigate and control Linux entirely from the command line
- Install and manage software independently
- Troubleshoot common system issues
- Write basic shell scripts for automation
- Use Git for version control
- Understand and run Docker containers
- Complete a system administration capstone project

## License

MIT License - See LICENSE file for details

## Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

## Author

Mohamed Dardouri - [GitHub](https://github.com/mdardouri)
