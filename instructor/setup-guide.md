# Instructor Setup Guide

## Course Overview

**Course:** Linux for Everyone
**Duration:** 10 sessions × 1.5 hours (15 hours total)
**Target Audience:** University students, complete beginners to Linux
**Class Size:** ~15 students per class

## Prerequisites for Students

### Before Day 1
Students should be informed to:
1. **Backup important data** - If installing Linux on their main machine
2. **Bring their laptop** - With at least 20GB free space and 4GB RAM (8GB recommended)
3. **Know their admin password** - For Windows dual-boot setup

### Technical Requirements
- **Processor:** 64-bit x86_64
- **RAM:** 4GB minimum (8GB+ recommended for Docker)
- **Storage:** 20GB minimum (50GB+ recommended)
- **Internet:** Required for package installation

## Instructor Environment Setup

### Required Software

1. **mdBook** (for course materials)
   ```bash
   cargo install mdbook
   cargo install mdbook-mermaid
   ```

2. **Linux Distribution Options**
   - **Fedora 41+** (recommended for teaching)
   - **Debian 12+** (stable alternative)
   - Both with GNOME desktop

3. **Virtual Machine Software** (for students without spare hardware)
   - **VirtualBox** (free, cross-platform)
   - **VMware Workstation Player** (free for personal use)
   - **GNOME Boxes** (built-in to GNOME, simplest option)

### Classroom Setup

#### Option 1: Computer Lab
- All machines have Linux pre-installed
- Students have sudo access
- Network with internet connectivity
- Projector for demonstrations

#### Option 2: BYOD with Dual-Boot
- Students install Linux alongside Windows
- **Risk:** Potential data loss if not careful
- **Benefit:** Students get daily driver experience

#### Option 3: Virtual Machines
- Each student runs Linux in VM
- **Recommended for:** First-time teaching
- Minimum 4GB RAM allocated to VM

### Installation Media

**For Fedora:**
```bash
# Download Fedora Workstation 41+
# Verify checksum
sha256sum Fedora-Workstation-Live-x86_64-41.iso
# Create bootable USB
sudo dd if=Fedora-Workstation-Live-x86_64-41.iso of=/dev/sdX bs=4M status=progress
```

**For Debian:**
```bash
# Download Debian 12 Bookworm
# Verify with signed checksums
# Create bootable USB with Ventoy or Rufus (Windows)
```

## Pre-Course Checklist

### 2 Weeks Before
- [ ] Announce course requirements to students
- [ ] Schedule computer lab (if applicable)
- [ ] Download latest Linux ISOs
- [ ] Prepare installation USB drives (at least 3-4 per class)

### 1 Week Before
- [ ] Test mdBook build and navigation
- [ ] Prepare demo environment
- [ ] Create test user accounts (lab environment)
- [ ] Set up any network requirements

### Day of Class
- [ ] Bring spare USB drives
- [ ] Have offline package mirror (if network unreliable)
- [ ] Prepare emergency live USB environment
- [ ] Print cheat sheet handouts (optional)

## Teaching Environment

### Recommended Terminal Setup
```bash
# Set up a clean teaching environment
mkdir -p ~/linux-course
cd ~/linux-course

# Color-coded prompts for clarity
# Add to ~/.bashrc for demo user:
export PS1='\[\033[01;32m\]\u@\h\[\033[00m\]:\[\033[01;34m\]\w\[\033[00m\]\$ '
```

### Demo User Account
Create a dedicated demo user without sudo access for demonstrating permission errors:
```bash
sudo useradd -m -s /bin/bash demo
sudo passwd demo
```

## Common Issues and Solutions

### Installation Issues

#### "No bootable device found"
- **Cause:** UEFI/Legacy boot mismatch
- **Fix:** Enter BIOS, enable UEFI boot, disable Secure Boot

#### "Black screen after installation"
- **Cause:** Graphics driver issue
- **Fix:** Add `nomodeset` to kernel parameters, install NVIDIA/AMD drivers post-install

#### Windows not appearing after dual-boot
- **Cause:** Windows Fast Startup enabled
- **Fix:** Boot Windows, disable Fast Startup in Power Options

### During Class

#### Package installation fails
```bash
# Clear DNF cache
sudo dnf clean all
sudo dnf makecache

# For APT
sudo apt update
sudo apt --fix-broken install
```

#### Network not working
```bash
# Check NetworkManager status
nmcli device status
# Restart network
sudo systemctl restart NetworkManager
```

#### Disk space issues
```bash
# Check space
df -h
# Clean package cache
sudo dnf clean all
```

## Session Timing Guidelines

### Typical 1.5-Hour Session Breakdown
- **5 min:** Recap of previous session
- **10 min:** Introduction to new concepts
- **30 min:** Live demonstration
- **30 min:** Guided exercises
- **10 min:** Individual practice
- **5 min:** Quiz/Q&A

### Pacing Tips
- Plan for 20% more time than estimated
- Have "bonus" content ready for fast learners
- Know which topics can be skipped if time runs short
- Always leave 5 minutes for cleanup/questions

## Assessment Strategy

### Formative Assessment
- **In-class exercises:** Immediate feedback
- **Exit tickets:** One-question checks at end of session
- **Peer review:** Students explain concepts to each other

### Summative Assessment
- **Chapter quizzes:** Multiple choice and short answer
- **Capstone project:** Demonstrates all learned skills
- **Practical exam:** Complete a task from command line only

### Grading Rubric (Suggested)
- **Participation:** 20%
- **Exercises:** 30%
- **Quizzes:** 20%
- **Capstone:** 30%

## Accessibility Considerations

### Visual Impairments
- Use GNOME's accessibility features
- High contrast themes available
- Screen reader (Orca) pre-installed

### Motor Impairments
- Sticky keys enabled in Settings → Universal Access
- On-screen keyboard available
- Voice control tools available

### Learning Differences
- All course materials available in digital format
- Keyboard shortcuts reduce mouse dependency
- Self-paced exercises accommodate different speeds

## Resources for Instructors

### Official Documentation
- **Fedora:** https://docs.fedoraproject.org/
- **Debian:** https://www.debian.org/doc/
- **GNOME:** https://help.gnome.org/

### Community Support
- **Fedora Discussion:** https://discussion.fedoraproject.org/
- **Debian Forums:** https://forums.debian.net/
- **Reddit:** r/linux4noobs

### Continuous Learning
- Subscribe to Linux distribution newsletters
- Follow Linux blogs (OMG! Ubuntu, Fedora Magazine)
- Join local Linux User Groups (LUGs)

## Troubleshooting Student Issues

### "I messed up my system"
1. Don't panic - almost everything is fixable
2. Use live USB to boot and chroot in
3. Have students document what they did
4. Use it as a learning opportunity

### "I don't understand the command line"
1. Emphasize it's like learning any language
2. Break commands into components
3. Use `man` pages together
4. Practice with simple commands first

### "I'm afraid of breaking something"
1. Reassure: Linux is resilient
2. Show how to find help
3. Emphasize the power of backups
4. Start with non-destructive commands

## Safety Guidelines

### Always Emphasize
- **Read before you type** - Don't run commands blindly
- **Understand sudo** - It gives you power to break things
- **Backup first** - Before major changes
- **Ask for help** - The Linux community is friendly

### Commands to Avoid in Early Sessions
- `rm -rf /` - Explain why this is dangerous
- `dd` - Data destroyer if used incorrectly
- `mkfs` - Formats filesystems
- `:(){ :|:& };:` - Fork bomb

## Feedback and Improvement

After teaching the course:
1. Collect student feedback
2. Note which topics were confusing
3. Update exercises based on common issues
4. Share improvements with other instructors

## Emergency Procedures

### Complete System Failure
1. Boot from live USB
2. Mount partitions and backup important data
3. Reinstall if necessary
4. Restore from backup

### Network Outage
1. Have offline package cache ready
2. Use local documentation
3. Focus on CLI exercises that don't require internet

### Time Running Short
1. Skip advanced topics
2. Move exercises to homework
3. Focus on core concepts
4. Provide written materials for skipped content
