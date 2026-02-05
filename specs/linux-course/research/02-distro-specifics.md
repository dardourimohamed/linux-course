# Research 2: Fedora & Debian Specifics

## Sources Analyzed
- Fedora official documentation (DNF vs APT)
- Debian package management guides
- Community comparisons (Reddit, forums, HN)
- Package manager comparison articles

## Key Findings

### Package Management Equivalence Table

| APT (Debian) | DNF (Fedora) | Notes |
|--------------|--------------|-------|
| `apt update` | `dnf check-update` | DNF updates cache automatically |
| `apt upgrade` | `dnf upgrade` | `dnf up` also works |
| `apt install` | `dnf install` | Package names may differ |
| `apt remove` | `dnf remove` |  |
| `apt search` | `dnf search` | `dnf repoquery` for advanced |
| `apt autoremove` | `dnf autoremove` | May remove wanted packages |
| `apt full-upgrade` | `dnf distro-sync` | Release upgrade: `dnf system-upgrade` |

### Key Differences

1. **Cache Handling**
   - Debian: Must run `apt update` before installs
   - Fedora: DNF updates cache automatically when stale

2. **Package Names**
   - `libc6-dev` (Debian) → `glibc-devel` (Fedora)
   - Teach students to search, not memorize

3. **Configuration Files**
   - Fedora packages don't treat configs like Debian
   - No direct equivalent to `apt purge`

### Installation Comparison

| Aspect | Fedora | Debian |
|--------|--------|--------|
| **Installer** | Anaconda (GUI, very polished) | Calamares (GUI) or text-based |
| **Live Session** | Available (Workstation edition) | Available |
| **Default Desktop** | GNOME | GNOME |
| **First-boot Setup** | Guided wizard | Guided wizard |
| **Partitioning** | Automatic + Custom | Automatic + Custom |
| **Third-party Codecs** | Requires RPM Fusion | Contrib/non-free repos |

### GNOME Configuration (Both Distros)

**Tools common to both:**
- `gnome-tweaks` — Advanced customization
- `gnome-extensions-app` — Manage extensions
- `settings` — System settings

**Customization hierarchy:**
1. Settings (built-in) → Wallpapers, dark mode, behavior
2. GNOME Tweaks → Fonts, top bar, startup apps
3. Extensions → Dash to Dock, Blur my Shell, etc.
4. Themes → GTK themes, icon themes (via user themes extension)

### Dual-boot Installation

**Both support similar dual-boot workflows:**
1. Shrink Windows partition (from Windows Disk Management)
2. Boot live USB
3. Install alongside Windows
4. GRUB detects Windows automatically

**Fedora-specific:**
- Btrfs filesystem by default (modern, snapshots)
- Silverblue available (immutable variant)

**Debian-specific:**
- ext4 by default (traditional, stable)
- More conservative software versions

## Recommendations

1. **Teach package managers together** — Show equivalence table early
2. **Use search-first approach** — `dnf search` or `apt search` over memorization
3. **Mention RPM Fusion for Fedora** — Codecs, proprietary drivers
4. **Mention contrib/non-free for Debian** — Similar purpose
5. **Highlight automatic cache** — Fedora students don't need `update` habit
6. **Cover both installers** — Screenshots for Anaconda and Calamares

## Sources

- Fedora DNF Docs: https://docs.fedoraproject.org/en-US/quick-docs/dnf-vs-apt/
- Package Manager Comparison: https://linuxcommunity.io/t/linux-package-managers-compared-apt-dnf-pacman-and-zypper/5760
- Opensource Comparison: https://opensource.com/article/21/7/dnf-vs-apt
