# Research 5: Capstone Project Examples

## Sources Analyzed
- Linux Training Academy project ideas
- GeeksforGeeks Linux projects
- Reddit sysadmin project discussions
- DevDojo junior admin projects
- University capstone examples

## Project Categories for Beginners

### Category 1: Complete Setup (Daily Driver)

**Objective:** Configure Linux for personal daily use

**Components:**
1. **Installation** — Dual-boot or VM setup
2. **Desktop customization** — GNOME Tweaks, extensions, themes
3. **Essential software** — Browser, media player, office suite
4. **Development tools** — VS Code, Git, Docker
5. **Backup strategy** — Automated backup script
6. **Security basics** — Firewall, updates

**Deliverables:**
- Screenshots of custom desktop
- List of installed software with justification
- Backup script (shell)
- Firewall configuration file

**Estimated time:** 4-5 sessions spread across course

---

### Category 2: System Administration Task

Based on research, appropriate beginner projects:

#### Option A: Personal Web Server (LAMP Stack)

**Tasks:**
1. Install Apache web server
2. Configure basic HTML page
3. Set up permissions correctly
4. Start/enable service (systemd)
5. Configure firewall to allow traffic
6. Test from another device

**Skills demonstrated:**
- Package management
- Service management
- File permissions
- Basic networking
- Firewall configuration

#### Option B: Automated Backup System

**Tasks:**
1. Create backup directory structure
2. Write backup script (shell)
3. Set up cron job for automation
4. Configure log rotation
5. Test restoration
6. Document the process

**Skills demonstrated:**
- Shell scripting
- Cron/systemd timers
- File operations
- Documentation

#### Option C: User Management System

**Tasks:**
1. Create multiple user accounts
2. Configure sudo access
3. Set up user groups
4. Configure permissions for shared directory
5. Create welcome script for new users
6. Document user policies

**Skills demonstrated:**
- User/group management
- Permissions (chmod, chown)
- sudo configuration
- Shell scripting

---

### Category 3: Mini-Projects (Build to Capstone)

**Session-based exercises that accumulate:**

| Session | Exercise | Builds To |
|---------|----------|-----------|
| 1 | Installation | Base system |
| 2 | Desktop setup | Daily driver |
| 3 | File navigation | File management |
| 4 | Basic scripts | Automation |
| 5 | User creation | Multi-user setup |
| 6 | Permissions | Security |
| 7 | Package management | Software setup |
| 8 | Service management | Server setup |
| 9 | Git usage | Version control |
| 10 | Final integration | Capstone complete |

---

## Recommended Capstone for This Course

Given the requirements (15 hours, beginners, university students):

### "Personal Linux Server"

**Scenario:** Set up a Linux system that serves a personal website and automates backups.

**Phase 1: Setup (Sessions 1-4)**
- [x] Install Linux (Fedora/Debian)
- [x] Configure GNOME desktop
- [x] Install development tools
- [x] Master CLI navigation

**Phase 2: Services (Sessions 5-7)**
- [x] Install Apache/nginx
- [x] Create and serve HTML page
- [x] Configure firewall
- [x] Enable auto-start

**Phase 3: Automation (Session 8)**
- [x] Write backup script for web files
- [x] Set up automated backup (cron/systemd)
- [x] Test restoration

**Phase 4: Version Control (Session 9)**
- [x] Initialize Git repo for web files
- [x] Commit changes
- [x] Document in README

**Phase 5: Integration & Presentation (Session 10)**
- [x] Combine all components
- [x] Create documentation
- [x] Demo to class
- [x] Quiz assessment

### Assessment Rubric

| Criterion | Excellent | Good | Needs Work |
|-----------|-----------|------|------------|
| **Installation** | Dual-boot, correct partitioning | VM only | Requires reinstall |
| **Services** | Working, secured, auto-start | Working, manual start | Not functional |
| **Automation** | Cron/systemd working, logs correct | Script works, manual run | Script incomplete |
| **Documentation** | Clear README, screenshots | Basic notes | Minimal |
| **CLI Skills** | Commands correct, efficient | Commands work | Frequent errors |
| **Problem-solving** | Independent troubleshooting | Some help needed | Heavy guidance |

## Sources

- Linux Training Academy: https://www.linuxtrainingacademy.com/linux-projects/
- GeeksforGeeks: https://www.geeksforgeeks.org/linux-unix/linux-project-ideas-for-beginners/
- DevDojo: https://devdojo.com/post/bobbyiliev/5-project-ideas-for-junior-linux-system-administrators
- Reddit Discussion: https://www.reddit.com/r/linuxquestions/comments/mlnzir/
