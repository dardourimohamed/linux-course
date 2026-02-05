# Chapter 8 Quiz: Package Management

## Quiz Questions

### Multiple Choice

**1. What is DNF?**
- A) A desktop environment
- B) Fedora's package manager
- C) A text editor
- D) A system service

**Answer:** B

**2. Which command installs a package on Debian?**
- A) dnf install
- B) apt install
- C) package install
- D) yum install

**Answer:** B

**3. Why must you run `apt update` before installing packages on Debian?**
- A) To upgrade installed packages
- B) To refresh the list of available packages
- C) To clean the package cache
- D) To fix broken dependencies

**Answer:** B

**4. What is Flathub?**
- A) A package repository for Flatpak applications
- B) A file compression tool
- C) A package manager for Fedora
- D) A system backup utility

**Answer:** A

**5. Which option removes a package AND its configuration files in APT?**
- A) remove
- B) purge
- C) autoremove
- D) clean

**Answer:** B

### True/False

**6. True or False: Package managers automatically handle dependencies.**

**Answer:** True

**7. True or False: You need to be root to install packages.**

**Answer:** True

**8. True or False: Flatpak applications work on any Linux distribution.**

**Answer:** True

**9. True or False: The same package name works on both Fedora and Debian.**

**Answer:** False

**10. True or False: Package repositories are curated and signed for security.**

**Answer:** True

### Short Answer

**11. What are the key differences between DNF (Fedora) and APT (Debian) package managers?**

**Answer:**
| Aspect | DNF (Fedora) | APT (Debian) |
|--------|--------------|--------------|
| **Refresh repos** | Automatic | Manual (apt update) |
| **Install** | dnf install pkg | apt install pkg |
| **Remove** | dnf remove pkg | apt remove/pkg purge pkg |
| **Update** | dnf upgrade | apt upgrade |
| **Search** | dnf search query | apt search query |
| **Info** | dnf info pkg | apt show pkg |
| **Config files** | Kept on remove | Removed with purge |

**12. Why are Flatpak and Snap considered "universal" package formats?**

**Answer:**
Flatpak and Snap are universal because:
- **Distribution-independent**: Same package works on any distro that supports them
- **Include dependencies**: Bundle all libraries needed, avoiding version conflicts
- **Sandboxed**: Run in containers for security
- **Updated independently**: Not tied to system package updates
- **Consistent versions**: Same app version across all distributions

This solves the problem of apps not being available or being outdated in some distro repositories.

**13. What is the difference between `apt remove` and `apt purge`, and when would you use each?**

**Answer:**
- **`apt remove package`**: Removes the package but keeps configuration files in /etc and user data in /home. Use when you might reinstall later and want to preserve settings.

- **`apt purge package`**: Removes the package AND all configuration files. Use when you want a complete removal and won't reinstall, or when troubleshooting configuration issues.

### Discussion Question

**14. Linux package management is often cited as one of the advantages of Linux over Windows. Compare and contrast the Linux package management approach with Windows' method of downloading installers from websites. What are the advantages and disadvantages of each?**

**Sample Answer:**

**Linux Package Management:**

**Advantages:**
- **Centralized**: All software from trusted, signed repositories
- **Automatic updates**: One command updates everything
- **Dependency management**: Automatically handles required libraries
- **Clean uninstall**: Removes all files, no leftover junk
- **Security**: Packages are verified and signed
- **No hunting**: Don't need to search websites for installers
- **Consistent**: Same installation process for all software

**Disadvantages:**
- **Limited selection**: Not all software is in repositories
- **Older versions**: Repositories may lag behind latest releases
- **System-wide updates**: Updates can sometimes break things (rare)
- **Multiple package managers**: Different formats (rpm, deb, flatpak, snap)

**Windows Method:**

**Advantages:**
- **Latest versions**: Always get newest software from vendor
- **Wide selection**: Any software available for download
- **Familiar**: Traditional method most people know
- **Per-application control**: Update what you want, when you want

**Disadvantages:**
- **Security risk**: Downloading from unknown websites
- **No updates**: Each app handles updates differently
- **Dependency hell**: Applications bundle their own libraries (bloat)
- **Uninstall issues**: Leaves registry entries and files behind
- **Fragmented**: Each installer has different interface
- **Manual process**: Must find, download, and install each application separately

**Conclusion**: Linux's centralized package management is more secure, efficient, and maintainable. Windows' method offers more choice but at the cost of security and consistency. Flatpak/Snap bring Linux-like benefits to cross-distribution packaging.

## Answer Key

| Question | Answer | Points |
|----------|--------|--------|
| 1 | B | 1 |
| 2 | B | 1 |
| 3 | B | 1 |
| 4 | A | 1 |
| 5 | B | 1 |
| 6 | True | 1 |
| 7 | True | 1 |
| 8 | True | 1 |
| 9 | False | 1 |
| 10 | True | 1 |
| 11 | Key differences listed | 4 |
| 12 | Universal format explained | 3 |
| 13 | Both commands explained | 3 |
| 14 | Both approaches compared | 5 |

**Total Points:** 27

**Passing Score:** 18/27 (67%)

---

## Instructor Notes

### Common Misconceptions to Address

1. **"I can just download .exe files in Linux"** - Linux doesn't use .exe files. Explain that packages are the equivalent.

2. **"Flatpak is just another package manager"** - Emphasize that Flatpak is a different format that works across distros.

3. **"apt update upgrades packages"** - Clarify that update refreshes the package list, upgrade actually installs updates.

4. **"All software is in repositories"** - Many apps require Flatpak/Snap or manual installation.

### Teaching Tips

- **Live installation**: Install a package during class to show the process
- **Compare output**: Show the difference between dnf and apt output for similar operations
- **Dependency visualization**: Show how package managers resolve dependencies
- **Flatpak demo**: Install a Flatpak to show the alternative installation method
- **Troubleshooting**: Demonstrate fixing common package management issues

### Extension Activities

- Compare software availability in Fedora vs Debian repositories
- Set up a third-party repository (like RPM Fusion)
- Create a list of essential software for a specific use case (web development, gaming)
- Research how software gets into repositories
- Explore AppImage as another universal format
- Compare package managers across different Linux distributions
