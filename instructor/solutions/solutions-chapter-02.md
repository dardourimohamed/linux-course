# Chapter 2 Solutions: Installation

## Exercise Solutions

### Exercise 1: Verify ISO Download

**Task:** Verify the integrity of your downloaded Linux ISO using checksums.

**Solution:**

```bash
# First, navigate to your Downloads directory
cd ~/Downloads

# Check the checksum (example for Fedora)
sha256sum Fedora-Workstation-Live-x86_64-39-1.5.iso
```

**Expected Output:**
```
a1b2c3d4e5f6...  Fedora-Workstation-Live-x86_64-39-1.5.iso
```

```bash
# Compare with the official checksum (usually in CHECKSUM file)
cat CHECKSUM | grep Fedora-Workstation-Live-x86_64-39-1.5.iso
```

**Explanation:**
- `sha256sum` calculates the SHA-256 hash of the file
- Compare the output with the official checksum from the distribution's website
- If they match exactly, your download is complete and uncorrupted
- If they differ, you need to re-download the ISO

**Why this matters:**
- Downloads can be corrupted or incomplete
- Malicious actors could serve modified ISOs
- A checksum verifies you have the exact file the distributor intended

---

### Exercise 2: Create Bootable USB

**Task:** Create a bootable USB drive using appropriate tools.

**Solution:**

**On Linux:**

```bash
# List all disks to identify your USB
lsblk
```

**Expected Output:**
```
NAME        MAJ:MIN RM   SIZE RO TYPE MOUNTPOINTS
sda           8:0    1  28.9G  0 disk
└─sda1        8:1    1  28.9G  0 disk /run/media/user/USB
```

```bash
# Unmount the USB device
sudo umount /dev/sda1

# Write the ISO to USB (BE VERY CAREFUL with the device name!)
sudo dd if=Fedora-Workstation-Live-x86_64-39-1.5.iso of=/dev/sda bs=4M status=progress conv=fsync
```

**Expected Output:**
```
1024+0 records in
1024+0 records out
4294967296 bytes (4.3 GB, 4.0 GiB) copied, 120 s, 35.8 MB/s
```

**On Windows with Rufus:**
1. Download Rufus from https://rufus.ie/
2. Insert USB drive
3. Open Rufus, select your USB drive
4. Select the ISO file
5. Click "Start" and confirm
6. Wait for completion

**On macOS:**

```bash
# Convert ISO and write to disk
sudo dd if=~/Downloads/Fedora-Workstation-Live-x86_64-39-1.5.iso of=/dev/rdisk2 bs=1m
```

**Explanation:**
- `dd` is a low-level copy command - be extremely careful with device names
- `bs=4M` sets block size for faster copying
- `status=progress` shows copy progress
- `conv=fsync` ensures data is written to disk before completion
- **WARNING**: `dd` will destroy data on the target device without asking!

---

### Exercise 3: Try Linux Live

**Task:** Boot from the USB and explore the live environment without installing.

**Solution:**

**Steps:**
1. Insert the bootable USB
2. Restart your computer
3. Enter boot menu (typically F12, F10, or Del key)
4. Select the USB drive from boot menu
5. Choose "Try Fedora" (or "Try without installing")

**What you can do in live mode:**

```bash
# Open terminal and explore
cat /etc/os-release

# Check hardware compatibility
lspci          # List PCI devices
lsusb          # List USB devices
inxi -Fz       # Full system information

# Test internet connection
ping -c 4 google.com

# Browse files
nautilus       # File manager

# Check available disk space
df -h
```

**Expected Output from various commands:**
```
$ cat /etc/os-release
NAME="Fedora Linux"
VERSION="39 (Workstation Edition)"

$ df -h
Filesystem      Size  Used Avail Use% Mounted on
overlay         50G   15G   33G  32% /

$ ping -c 4 google.com
PING google.com (142.250.x.x): 56 data bytes
64 bytes from 142.250.x.x: icmp_seq=0 ttl=115 time=12.3 ms
```

**Explanation:**
- Live mode runs entirely from RAM and USB
- Changes made in live mode are not saved
- Perfect for testing hardware compatibility
- No risk to your existing system
- Performance may be slower than installed system

---

### Exercise 4: Plan Partitions

**Task:** Design a partitioning scheme for your installation.

**Solution:**

**Basic Partition Scheme (beginner):**

| Partition | Mount Point | Size | Type | Purpose |
|-----------|-------------|------|------|---------|
| `/dev/sda1` | `/boot/efi` | 512M | EFI System Partition | UEFI boot |
| `/dev/sda2` | `/` | Remainder | ext4 | Root filesystem |

**Intermediate Partition Scheme:**

| Partition | Mount Point | Size | Type | Purpose |
|-----------|-------------|------|------|---------|
| `/dev/sda1` | `/boot/efi` | 512M | EFI System Partition | UEFI boot |
| `/dev/sda2` | `/` | 50G | ext4 | System files |
| `/dev/sda3` | `/home` | Remainder | ext4 | User data |
| `/dev/sda4` | swap | 8G+ | swap | Virtual memory |

**Calculating Swap Size:**
- Traditional: 2x RAM
- Modern: Equal to RAM (for hibernation)
- Minimum: 4GB
- Formula: `max(4GB, RAM size)` for systems ≤ 32GB RAM

**Why separate /home:**
- Reinstall system without losing user data
- Easier backups (backup /home separately)
- Share home directory between dual-boot installations

**Explanation:**
- UEFI systems require an EFI System Partition (ESP)
- Root partition (/) holds the entire system
- Separate /home protects user data from system reinstalls
- Swap is used when RAM is full and for hibernation

---

### Exercise 5: Installation

**Task:** Install Linux on your system.

**Solution:**

**During Installation (Fedora Anaconda):**

1. **Language Selection**: Choose your language
2. **Installation Summary**:
   - Installation Destination: Select disk, choose "Automatic" or "Custom"
   - User Creation: Create your user account
   - Root Password: Set a secure root password

**If using Custom Partitioning:**

```
1. Select your disk (e.g., /dev/sda)
2. Click "Done"
3. Create partitions:
   - Click "+" → Mount point: /boot/efi, Size: 512 MiB, Type: EFI System Partition
   - Click "+" → Mount point: /, Size: 50 GiB, Type: ext4
   - Click "+" → Mount point: /home, Size: remaining, Type: ext4
   - Click "+" → Mount point: swap, Size: 8 GiB, Type: swap
4. Click "Done" → "Accept Changes"
```

**Post-Installation Verification:**

```bash
# Check disk usage
df -h
```

**Expected Output:**
```
Filesystem      Size  Used Avail Use% Mounted on
/dev/sda2        50G   15G   33G  32% /
/dev/sda3       200G  120G   70G  63% /home
```

```bash
# Check mounted filesystems
mount | grep "^/dev"
```

**Expected Output:**
```
/dev/sda2 on / type ext4 (rw,relatime,seclabel)
/dev/sda3 on /home type ext4 (rw,relatime,seclabel)
/dev/sda1 on /boot/efi type vfat (rw,relatime,fmask=0077,dmask=0077...)
```

**Explanation:**
- The installer (Anaconda for Fedora, Calamares for some others) guides you through steps
- Automatic partitioning is safest for beginners
- Custom partitioning gives more control
- After installation, verify everything is mounted correctly

---

## Dual-Boot Setup (Optional)

**Shrinking Windows for Dual-Boot:**

1. **In Windows**:
   ```
   - Right-click Start → Disk Management
   - Right-click C: drive → Shrink
   - Enter size to shrink (leave at least 50GB for Windows)
   - Click "Shrink"
   ```

2. **Boot Linux USB**:
   - Install in the unallocated space
   - Installer should detect Windows and configure dual-boot automatically

**Verification:**

```bash
# Check if Windows is detected
sudo os-prober
```

**Expected Output:**
```
/dev/sda1:Windows 10 (loader) on /dev/sda1
```

---

## Common Problems and Solutions

| Problem | Solution |
|---------|----------|
| "Reboot and Select proper Boot device" | Re-enter BIOS, change boot order to SSD/HDD first |
| Windows doesn't appear in boot menu | Run `sudo os-prober` then `sudo update-grub` (Debian) or `sudo grub2-mkconfig -o /boot/grub2/grub.cfg` (Fedora) |
| WiFi not working | You may need proprietary drivers - connect via ethernet first and install |
| Screen resolution wrong | Install graphics drivers: `sudo dnf install xorg-x11-drv-amdgpu` (AMD) or similar |
| Installation freezes | Try different USB port, rewrite USB with different tool |

---

## Instructor Notes

### Teaching Tips

1. **VM First**: Have students practice in a VM before touching real hardware
2. **Backup Emphasis**: Stress backing up data before any disk operations
3. **Live USB Time**: Give students 15-20 minutes to explore live environment during class
4. **Partition Visualization**: Use diagrams to show how partitions relate to physical disk
5. **Troubleshooting Prep**: Discuss common issues before they encounter them

### Safety Reminders

- **Always verify the target device** before running `dd`
- **Backup important data** before repartitioning
- **Have a recovery plan** (Windows recovery media, Linux live USB)
- **Take screenshots** of partition layout before modifying

### Extension Activities

- Compare file system types (ext4 vs btrfs vs xfs)
- Research LVM (Logical Volume Manager) and its advantages
- Explore full-disk encryption options during installation
- Set up automatic backups post-installation
- Create a dual-boot rescue plan
