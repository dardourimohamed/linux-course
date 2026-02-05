# Chapter 8: Package Management

## Learning Objectives

By the end of this chapter, you will be able to:

- [x] Understand what package managers are and why they matter
- [x] Install software using DNF (Fedora) or APT (Debian)
- [x] Search for and discover new packages
- [x] Update and upgrade your system safely
- [x] Remove unwanted packages and clean up
- [x] Manage repositories and software sources

## Prerequisites

- Completed Chapter 7: Permissions & Users
- Comfortable with sudo for elevated privileges
- Basic understanding of the file system

---

## What is Package Management?

In Windows, you typically download `.exe` installers from websites. On Linux, **package managers** handle software installation, updates, and removal from centralized repositories.

### Why Package Managers Matter

| Advantage | Description |
|-----------|-------------|
| **Security** | Packages are signed and verified |
| **Dependencies** | Automatically handles required libraries |
| **Updates** | One command updates everything |
| **Removal** | Clean uninstallation |
| **Centralized** | No hunting across websites |

### The Repository Concept

```
┌─────────────────────────────────────────────────┐
│             Package Repository                   │
│  ┌──────────┐ ┌──────────┐ ┌──────────┐        │
│  │ Firefox  │ │ VLC      │ │ GIMP     │        │
│  │ v120.0   │ │ v3.0.18  │ │ v2.10    │        │
│  └──────────┘ └──────────┘ └──────────┘        │
└─────────────────────────────────────────────────┘
                    ↕
┌─────────────────────────────────────────────────┐
│           Package Manager (DNF/APT)             │
│  dnf install firefox  → Downloads + Installs    │
└─────────────────────────────────────────────────┘
                    ↕
┌─────────────────────────────────────────────────┐
│              Your System                        │
│  /usr/bin/firefox  ← Executable                │
│  /usr/lib/firefox  ← Libraries                  │
│  /usr/share/doc     ← Documentation             │
└─────────────────────────────────────────────────┘
```

---

## DNF: Fedora's Package Manager

**DNF** (Dandified YUM) is the default package manager for Fedora.

### Basic DNF Commands

```bash
sudo dnf install firefox       # Install Firefox
sudo dnf remove firefox        # Remove Firefox
sudo dnf update                # Update all packages
sudo dnf upgrade               # Upgrade distribution (major version)
dnf search python              # Search for Python packages
dnf info ffmpeg                # Get package information
```

### Installing Software

```bash
$ sudo dnf install vlc
[sudo] password for user:
Last metadata expiration check: 0:45:32 ago on Wed 07 Feb 2025 10:15:00 AM CET.
Dependencies resolved.
================================================================================
 Package         Architecture   Version               Repository         Size
================================================================================
Installing:
 vlc             x86_64         3.0.18-4.fc39         fedora             27 M
Installing dependencies:
 ... (dependencies listed) ...
Transaction Summary
================================================================================
Install  15 Packages

Total download size: 27 M
Installed size: 98 M
Is this ok [y/N]: y
```

### Updating Your System

```bash
# Check for available updates
sudo dnf check-update

# Update all packages
sudo dnf update

# Update only security patches
sudo dnf update --security

# Upgrade to next Fedora release
sudo dnf system-upgrade download --release=40
sudo dnf system-upgrade reboot
```

### Searching for Packages

```bash
# Search by name
dnf search libreoffice

# Search specifically for installed packages
dnf list installed

# List all available packages
dnf list available

# Get detailed package info
dnf info neovim

# Find which package provides a file
dnf provides ifconfig
```

### Removing Packages

```bash
# Remove package (keeps config)
sudo dnf remove vlc

# Remove package + dependencies not needed
sudo dnf remove vlc --remove-leaves

# Remove orphaned packages
sudo dnf autoremove

# Clean cached package files
sudo dnf clean all
```

---

## APT: Debian's Package Manager

**APT** (Advanced Package Tool) is the default package manager for Debian and Ubuntu.

### Basic APT Commands

```bash
sudo apt update               # Update package lists
sudo apt install firefox      # Install Firefox
sudo apt remove firefox       # Remove Firefox (keeps config)
sudo apt purge firefox        # Remove Firefox + config
sudo apt upgrade              # Upgrade installed packages
sudo apt full-upgrade         # Upgrade with dependency changes
apt search python             # Search for packages
apt show neovim               # Show package details
```

### The Essential `apt update`

Before installing on Debian, **always run** `apt update` first:

```bash
$ sudo apt update
Hit:1 http://deb.debian.org/debian bookworm InRelease
Get:2 http://security.debian.org/debian-security bookworm-security InRelease
Reading package lists... Done
```

This refreshes the list of available packages from the repositories.

### Installing Software

```bash
$ sudo apt install vlc
Reading package lists... Done
Building dependency tree... Done
The following additional packages will be installed:
  ... (dependencies) ...
Suggested packages:
  ... (optional packages) ...
The following NEW packages will be installed:
  vlc
0 upgraded, 1 newly installed, 0 to remove and 12 not upgraded.
Need to get 2,456 kB of archives.
After this operation, 8,920 kB of additional disk space will be used.
Do you want to continue? [Y/n] y
```

### Updating Your System

```bash
# Always update package lists first
sudo apt update

# Upgrade all installed packages
sudo apt upgrade

# Full upgrade (may remove packages for dependency)
sudo apt full-upgrade

# Clean up unnecessary packages
sudo apt autoremove

# Remove downloaded package files
sudo apt clean
```

### Searching for Packages

```bash
# Search by name or description
apt search libreoffice

# Show package details
apt show neovim

# List installed packages
apt list --installed

# List upgradable packages
apt list --upgradable
```

### Removing Packages

```bash
# Remove package (keeps config files)
sudo apt remove vlc

# Remove package + config files
sudo apt purge vlc

# Remove dependencies no longer needed
sudo apt autoremove

# Clean package cache
sudo apt clean
```

---

## DNF vs APT: Quick Reference

| Task | Fedora (DNF) | Debian (APT) |
|------|--------------|--------------|
| Refresh repos | Automatic | `sudo apt update` |
| Install | `sudo dnf install pkg` | `sudo apt install pkg` |
| Remove | `sudo dnf remove pkg` | `sudo apt remove pkg` |
| Remove + config | `sudo dnf remove pkg` | `sudo apt purge pkg` |
| Update packages | `sudo dnf upgrade` | `sudo apt upgrade` |
| Search | `dnf search query` | `apt search query` |
| Package info | `dnf info pkg` | `apt show pkg` |
| List installed | `dnf list installed` | `apt list --installed` |
| Clean cache | `sudo dnf clean all` | `sudo apt clean` |

---

## Managing Repositories

Repositories (repos) are sources of packages. Linux distributions maintain official repos.

### Viewing Repositories

```bash
# Fedora: List enabled repos
dnf repolist

# Fedora: Show all repos (enabled/disabled)
dnf repolist all

# Debian: View sources (in file)
cat /etc/apt/sources.list
cat /etc/apt/sources.list.d/*.list
```

### Adding Repositories (Fedora)

```bash
# Enable RPM Fusion (popular third-party repo)
sudo dnf install \
  https://download1.rpmfusion.org/free/fedora/rpmfusion-free-release-$(rpm -E %fedora).noarch.rpm

# Add a COPR repo
sudo dnf copr enable user/repo
```

### Adding Repositories (Debian)

```bash
# Add a PPA (Ubuntu only)
sudo add-apt-repository ppa:user/repo
sudo apt update

# Manually add repo (Debian)
echo "deb https://example.com/debian stable main" | \
  sudo tee /etc/apt/sources.list.d/custom.list
sudo apt update
```

### Third-Party Repositories: Caution!

⚠️ **Only add trusted repositories**. Third-party repos can:
- Break your system
- Contain malicious software
- Conflict with official packages

**Stick to official repos unless you know what you're doing.**

---

## Practical Examples

### Example 1: Installing a Development Environment

**Fedora:**
```bash
sudo dnf install git vim python3 python3-pip nodejs npm code
```

**Debian:**
```bash
sudo apt update
sudo apt install git vim python3 python3-pip nodejs npm code
```

### Example 2: Installing Media Software

**Fedora:**
```bash
# First enable RPM Fusion for codecs
sudo dnf install https://download1.rpmfusion.org/free/fedora/rpmfusion-free-release-$(rpm -E %fedora).noarch.rpm
sudo dnf install vlc gimp audacity
```

**Debian:**
```bash
sudo apt update
sudo apt install vlc gimp audacity
```

### Example 3: Finding What Provides a Command

```bash
# You want to run 'ifconfig' but it's missing
# Fedora:
$ dnf provides ifconfig
net-tools-2.0-0.fc39.x86_64 : Basic networking tools
Repo        : fedora
Matched from:
Filename    : /usr/sbin/ifconfig

$ sudo dnf install net-tools

# Debian:
$ apt provides ifconfig
net-tools-old: /sbin/ifconfig
$ sudo apt install net-tools-old
```

### Example 4: System Maintenance

```bash
# Fedora:
sudo dnf upgrade --refresh          # Refresh metadata + upgrade
sudo dnf autoremove                 # Remove unused packages
sudo dnf clean all                  # Free disk space

# Debian:
sudo apt update                     # Refresh package lists
sudo apt full-upgrade               # Upgrade everything
sudo apt autoremove                 # Remove unused packages
sudo apt autoclean                  # Clean old package files
```

---

## Understanding Package Names

Packages often have multiple versions or variants:

```bash
# Package naming patterns
python3              # Python interpreter
python3-devel        # Development headers (Fedora)
python3-dev          # Development headers (Debian)
python3-pip          # Pip package manager
python3-venv         # Virtual environment support

# Library packages
libffmpeg-dev        # FFmpeg development files
ffmpeg               # FFmpeg tools

# Architecture-specific
bash.x86_64          # Intel/AMD 64-bit
bash.aarch64         # ARM 64-bit
bash.noarch          # Architecture-independent
```

---

## Flatpak and Snap: Universal Packages

Some apps are distributed as **Flatpak** (Fedora-friendly) or **Snap** (Ubuntu-focused). These work across distributions.

### Flatpak (Recommended for Fedora)

```bash
# Install Flatpak support
sudo dnf install flatpak

# Add Flathub repository
flatpak remote-add --if-not-exists flathub \
  https://flathub.org/repo/flathub.flatpakrepo

# Install an app
flatpak install flathub com.spotify.Client

# Run an app
flatpak run com.spotify.Client

# List installed
flatpak list

# Update all
flatpak update
```

### Snap (Available on both)

```bash
# Install Snap support
# Fedora: sudo dnf install snapd
# Debian: sudo apt install snapd

# Install an app
sudo snap install spotify

# List installed
snap list

# Update all
sudo snap refresh
```

---

## Troubleshooting

### Broken Packages

```bash
# Fedora: Fix dependency issues
sudo dnf install --repair
sudo dnf clean all
sudo dnf distro-sync

# Debian: Fix broken packages
sudo apt --fix-broken install
sudo apt update
sudo apt full-upgrade
```

### Lock Errors

If you get "Could not get lock /var/lib/dpkg/lock-frontend":

```bash
# Another process is using apt
# Wait a minute, or find the process:
ps aux | grep apt

# If stuck, remove lock (carefully!)
sudo rm /var/lib/dpkg/lock-frontend
sudo rm /var/lib/dpkg/lock
```

### Package Not Found

```bash
# Check package name spelling
dnf search similar-name

# Package might be in a disabled repo
dnf repolist all

# Package might not exist for your distro
# Search online or use Flatpak/Snap instead
```

---

## Summary

In this chapter, you learned:

- **Package Managers**: Centralized software management (DNF for Fedora, APT for Debian)
- **Repositories**: Trusted sources of packages
- **Installation**: `dnf install` / `apt install`
- **Updates**: `dnf upgrade` / `apt upgrade` (plus `apt update` for Debian)
- **Removal**: `dnf remove` / `apt remove` (use `apt purge` to remove configs)
- **Searching**: `dnf search` / `apt search`
- **Universal Packages**: Flatpak and Snap for distribution-independent apps

---

## Chapter Quiz

Test your understanding of package management:

{{#quiz ../../quizzes/chapter-08.toml}}

---

## Exercises

### Exercise 1: Install a Text Editor

1. Search for the `neovim` package
2. View its information
3. Install it
4. Verify it's installed

### Exercise 2: System Update

1. Check for available updates
2. Update your system
3. Review what was updated

### Exercise 3: Package Discovery

1. Search for image manipulation software
2. Find and install `gimp`
3. Use `provides` or `apt-file` to find which package provides `ffmpeg`

### Exercise 4: Cleanup

1. List installed packages
2. Remove a package you don't need
3. Clean the package cache
4. Remove orphaned dependencies

### Exercise 5: Flatpak (Optional)

1. Install Flatpak support
2. Add Flathub repository
3. Install an app from Flathub (e.g., Spotify)
4. Run the installed app

---

## Expected Output

### Exercise 1 Solution

```bash
$ dnf search neovim
========= Name & Summary Matched: neovim =========
neovim.x86_64 : A fork of Vim focused on extensibility

$ dnf info neovim
Name         : neovim
Version      : 0.9.5
Release      : 1.fc39
Architecture : x86_64
Size         : 3.2 M
Source       : neovim-0.9.5-1.fc39.src.rpm
Repository   : fedora
Summary      : A fork of Vim focused on extensibility
...

$ sudo dnf install neovim
[... output ...]
Installed:
  neovim.x86_64 0.9.5-1.fc39
Complete!

$ nvim --version
NVIM v0.9.5
```

### Exercise 2 Solution

```bash
# Fedora:
$ sudo dnf upgrade --refresh
Last metadata expiration check: ...
Dependencies resolved.
================================================================================
 Package           Architecture   Version              Repository       Size
================================================================================
Upgrading:
 systemd           x86_64         254.10-1.fc39        updates         3.2 M
 systemd-libs      x86_64         254.10-1.fc39        updates         567 K
 ...
Transaction Summary
================================================================================
Upgrade  12 Packages

Total download size: 15 M
Is this ok [y/N]: y
...

# Debian:
$ sudo apt update
Hit:1 http://deb.debian.org/debian bookworm InRelease
Get:2 http://security.debian.org/debian-security bookworm-security InRelease
Reading package lists... Done

$ sudo apt upgrade
Reading package lists... Done
The following packages will be upgraded:
  curl python3 systemd ...
12 upgraded, 0 newly installed, 0 to remove and 0 not upgraded.
Need to get 8,921 kB of archives.
After this operation, 0 B of additional disk space will be used.
Do you want to continue? [Y/n] y
```

### Exercise 3 Solution

```bash
$ dnf search image editor
========= Name & Summary Matched: =========
gimp.x86_64 : GNU Image Manipulation Program
krita.x86_64 : Digital Painting Application

$ sudo dnf install gimp
[... installation output ...]
Complete!

$ which ffmpeg
/usr/bin/ffmpeg

$ dnf provides /usr/bin/ffmpeg
ffmpeg-6.1.1-2.fc39.x86_64 : Multimedia framework
Repo        : fedora
```

### Exercise 4 Solution

```bash
$ dnf list installed | grep -i test
test-package.x86_64      1.0-1      @fedora

$ sudo dnf remove test-package
Removed:
  test-package.x86_64 1.0-1
Complete!

$ sudo dnf clean all
24 files removed

$ sudo dnf autoremove
Removed:
  dependency-package.x86_64 2.0-1
Complete!
```

---

## Next Chapter

In Chapter 9, you'll learn about **Processes & Services** - viewing running processes, monitoring system resources, managing systemd services, and understanding system logs.
