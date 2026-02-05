# Chapter 8 Solutions: Package Management

## Exercise Solutions

### Exercise 1: Install a Text Editor

**Task:** Search for, view information, and install neovim.

**Solution:**

```bash
# Search for the package
dnf search neovim
```

**Expected Output:**
```
========= Name & Summary Matched: neovim =========
neovim.x86_64 : A fork of Vim focused on extensibility and usability
```

```bash
# View package information
dnf info neovim
```

**Expected Output:**
```
Available Packages
Name         : neovim
Version      : 0.9.5
Release      : 1.fc39
Architecture : x86_64
Size         : 3.2 M
Source       : neovim-0.9.5-1.fc39.src.rpm
Repository   : fedora
Summary      : A fork of Vim focused on extensibility and usability
URL          : https://neovim.io/
License      : Vim
Description  : Neovim is a fork of Vim focused on extensibility and
             : usability. It aims to be a drop-in replacement for Vim.
```

```bash
# Install neovim
sudo dnf install neovim
```

**Expected Output:**
```
Last metadata expiration check: 0:45:32 ago on Wed 07 Feb 2025 10:15:00.
Dependencies resolved.
================================================================================
 Package         Architecture   Version               Repository         Size
================================================================================
Installing:
 neovim         x86_64         0.9.5-1.fc39          fedora             3.2 M
Installing dependencies:
 libuv          x86_64         1:1.44.2-1.fc39       fedora             156 k
 lua-luv        x86_64         1.44.2-1.fc39         fedora              41 k
...
Transaction Summary
================================================================================
Install  5 Packages

Total download size: 3.4 M
Installed size: 12 M
Is this ok [y/N]: y
...
Complete!
```

```bash
# Verify installation
nvim --version
```

**Expected Output:**
```
NVIM v0.9.5
Build type: Release
LuaJIT 2.1.1703358377
```

**Explanation:**
- `dnf search`: Find packages by name/description
- `dnf info`: Show detailed package information
- `dnf install`: Download and install package (and dependencies)
- DNF automatically handles dependency resolution

---

### Exercise 2: System Update

**Task:** Check for and install updates.

**Solution:**

```bash
# Check for available updates
sudo dnf check-update
```

**Expected Output:**
```
Last metadata expiration check: 1:00:00 ago on Wed 07 Feb 2025 10:00:00.
bash.x86_64                    5.2.15-1.fc39          fedora
kernel-core.x86_64             6.5.6-300.fc39         updates
systemd.x86_64                 254.10-1.fc39          updates
```

```bash
# Update all packages
sudo dnf upgrade
```

**Expected Output:**
```
Last metadata expiration check: 1:05:00 ago on Wed 07 Feb 2025 10:00:00.
Dependencies resolved.
================================================================================
 Package           Architecture   Version              Repository       Size
================================================================================
Upgrading:
 bash              x86_64         5.2.15-1.fc39        fedora           1.2 M
 kernel-core       x86_64         6.5.6-300.fc39       updates          25 M
 systemd           x86_64         254.10-1.fc39        updates          3.2 M
...
Transaction Summary
================================================================================
Upgrade  12 Packages

Total download size: 35 M
Is this ok [y/N]: y
...
Complete!
```

```bash
# Review what was updated
sudo dnf history info | head -30
```

**Expected Output:**
```
Transaction ID: 45
Begin time: Wed Feb  7 10:30:00 2025
Begin rpmdb: 4523:ef456789...
Released by: Command line
Performances:
  Packages upgraded: 12
  Packages removed: 0
  Packages installed: 0
...
Upgraded:
  bash-5.2.15-1.fc39.x86_64
  kernel-core-6.5.6-300.fc39.x86_64
  systemd-254.10-1.fc39.x86_64
  ...
```

**Explanation:**
- `check-update`: Show available updates without installing
- `upgrade`: Update all installed packages to latest versions
- `history`: View transaction history to see what changed

---

### Exercise 3: Package Discovery

**Task:** Search for and install image manipulation software.

**Solution:**

```bash
# Search for image manipulation software
dnf search "image editor"
```

**Expected Output:**
```
========= Name & Summary Matched: =========
gimp.x86_64 : GNU Image Manipulation Program
krita.x86_64 : Digital Painting Application
inkscape.x86_64 : Vector Graphics Editor
```

```bash
# Find and install gimp
sudo dnf install gimp
```

**Expected Output:**
```
Dependencies resolved.
================================================================================
 Package      Architecture   Version               Repository         Size
================================================================================
Installing:
 gimp         x86_64         2:2.10.34-1.fc39      fedora             58 M
Installing dependencies:
... (long list of dependencies) ...
Transaction Summary
================================================================================
Install  85 Packages

Total download size: 158 M
Installed size: 512 M
Is this ok [y/N]: y
...
Complete!
```

```bash
# Find which package provides ffmpeg
dnf provides /usr/bin/ffmpeg
```

**Expected Output:**
```
Last metadata expiration check: 0:30:00 ago on ...
ffmpeg-6.1.1-2.fc39.x86_64 : Multimedia framework
Repo        : fedora
Matched from:
Provide    : /usr/bin/ffmpeg
```

```bash
# Or search by command name
dnf provides ffmpeg
```

**Expected Output:**
```
ffmpeg-6.1.1-2.fc39.x86_64 : Multimedia framework
Repo        : fedora
```

**Explanation:**
- `dnf search`: Find packages by keyword in name or description
- `dnf provides`: Find which package provides a specific file or command
- Useful when you know the command but not the package name

---

### Exercise 4: Cleanup

**Task:** Remove unwanted packages and clean cache.

**Solution:**

```bash
# List installed packages
dnf list installed | grep gimp
```

**Expected Output:**
```
gimp.x86_64                    2:2.10.34-1.fc39                    @fedora
```

```bash
# Remove gimp (keeps config)
sudo dnf remove gimp
```

**Expected Output:**
```
Dependencies resolved.
================================================================================
 Package      Architecture   Version               Repository         Size
================================================================================
Removing:
 gimp         x86_64         2:2.10.34-1.fc39      @fedora             58 M
Removing unused dependencies:
... (dependencies that were only needed by gimp) ...
Transaction Summary
================================================================================
Remove  15 Packages

Freed space: 180 M
Is this ok [y/N]: y
...
Complete!
```

```bash
# Clean package cache
sudo dnf clean all
```

**Expected Output:**
```
25 files removed
```

```bash
# Remove orphaned dependencies (no longer needed)
sudo dnf autoremove
```

**Expected Output:**
```
Dependencies resolved.
================================================================================
 Package      Architecture   Version               Repository         Size
================================================================================
Removing:
... (packages that were installed as dependencies) ...
Transaction Summary
================================================================================
Remove  5 Packages

Freed space: 45 M
Is this ok [y/N]: y
...
Complete!
```

```bash
# Verify cleanup
df -h /var/cache/dnf
```

**Expected Output:**
```
Filesystem      Size  Used Avail Use% Mounted on
/dev/sda2        50G   22G   26G  46% /
```

**Explanation:**
- `dnf remove`: Remove package (keeps config files)
- `dnf clean all`: Remove cached package files to free disk space
- `dnf autoremove`: Remove packages that were installed as dependencies but are no longer needed

---

### Exercise 5: Flatpak (Optional)

**Task:** Install Flatpak and add Flathub.

**Solution:**

```bash
# Install Flatpak support
sudo dnf install flatpak
```

```bash
# Add Flathub repository
flatpak remote-add --if-not-exists flathub https://flathub.org/repo/flathub.flatpakrepo
```

**Expected Output:**
```
Flathub remote already exists
```

```bash
# List remotes
flatpak remote-list
```

**Expected Output:**
```
Name    Options
flathub system
```

```bash
# Search for an application
flatpak search spotify
```

```bash
# Install Spotify from Flathub
flatpak install flathub com.spotify.Client
```

**Expected Output:**
```
Looking for matches…
Remote 'flathub' found 'Spotify' (com.spotify.Client) in 'flathub' repo
Required runtime for com.spotify.Client/x86_64/stable (branch: stable) found in remote 'flathub'
com.spotify-client permissions: ipc, network, pulseaudio, wayland, x11, dbus
    dbus access: [org.mpris.MediaPlayer2.Player]
        ID                              Branch      Op    Remote    Download
 1. [ ] com.spotify.Client            stable      u     flathub   95.6 MB / 95.6 MB

Install? [Y/n]: y
```

```bash
# Run the installed app
flatpak run com.spotify.Client
```

```bash
# List installed Flatpaks
flatpak list
```

**Expected Output:**
```
Name                    Application ID                                    Branch    Installation
Spotify                 com.spotify.Client                               stable    system
```

**Explanation:**
- Flatpak provides distribution-independent packages
- Flathub is the main Flatpak repository
- Same app runs on any distro that supports Flatpak
- Useful for apps not available in distro repositories

---

## Common DNF/APT Operations

### Repository Management

```bash
# List enabled repositories
dnf repolist

# Show all repositories (enabled/disabled)
dnf repolist all

# Enable a repository
sudo dnf config-manager --set-enabled repo-name

# Disable a repository
sudo dnf config-manager --set-disabled repo-name
```

### Package Information

```bash
# List installed packages
dnf list installed

# List all available packages
dnf list available

# Get detailed package info
dnf info package-name

# Find which package owns a file
dnf provides /path/to/file
```

### Package Maintenance

```bash
# Security updates only
sudo dnf upgrade --security

# Download only (don't install)
sudo dnf upgrade --downloadonly

# Downgrade a package
sudo dnf downgrade package-name

# Reinstall a package
sudo dnf reinstall package-name
```

---

## Instructor Notes

### Teaching Tips

1. **Live installation**: Install a package during class to show the process
2. **Dependency visualization**: Show how DNF resolves dependencies
3. **Compare DNF vs APT**: Highlight differences for students using Debian
4. **Flatpak demo**: Install a Flatpak to show alternative installation method
5. **Troubleshooting**: Demonstrate fixing common package management issues

### Common Student Issues

| Issue | Solution |
|-------|----------|
| Package not found | Check spelling, try `dnf search similar-name` |
| GPG key errors | Import key: `sudo rpm --import /etc/pki/rpm-gpg/*` |
| Dependency conflicts | `sudo dnf install --best --allowerasing` |
| 404 errors | Run `sudo dnf clean all && sudo dnf upgrade` |
| Half-installed package | `sudo dnf install --repair` |

### Extension Activities

- Compare package availability in Fedora vs Debian repos
- Enable RPM Fusion or other third-party repos
- Create a list of essential development packages
- Research how packages get into repositories
- Compare Flatpak, Snap, and AppImage formats
- Set up automatic updates
- Explore the `/var/cache/dnf` directory structure
