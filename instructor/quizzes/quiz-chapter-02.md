# Chapter 2 Quiz: Installation

## Quiz Questions

### Multiple Choice

**1. What is the minimum RAM requirement for a comfortable Linux installation?**
- A) 2 GB
- B) 4 GB
- C) 8 GB
- D) 16 GB

**Answer:** C

**2. Which partition type is used for boot files in a UEFI system?**
- A) Root partition (/)
- B) EFI System Partition
- C) Swap partition
- D) /home partition

**Answer:** B

**3. What does dual-booting allow you to do?**
- A) Run two operating systems simultaneously
- B) Choose between two operating systems at startup
- C) Run Windows inside Linux
- D) Install two copies of the same Linux distribution

**Answer:** B

**4. Which tool is recommended for creating a bootable USB on Windows?**
- A) Rufus
- B) BalenaEtcher
- C) dd command
- D) All of the above

**Answer:** D

**5. What is the purpose of the swap partition?**
- A) Store user documents
- B) Store system logs
- C) Act as virtual memory when RAM is full
- D) Boot the operating system

**Answer:** C

### True/False

**6. True or False: You must shrink your Windows partition before installing Linux in a dual-boot configuration.**

**Answer:** True

**7. True or False: A virtual machine installation provides the same performance as a native installation.**

**Answer:** False

**8. True or False: The btrfs file system supports snapshots, which can be useful for system rollback.**

**Answer:** True

**9. True or False: GRUB is the boot loader that manages which operating system to boot.**

**Answer:** True

**10. True or False: You should always verify the ISO checksum after downloading a Linux distribution.**

**Answer:** True

### Short Answer

**11. What are the three main installation methods for Linux, and which is recommended for beginners?**

**Answer:**
1. Dual-boot - Run Linux alongside Windows (recommended)
2. Virtual Machine - Safest option, runs within your current OS
3. Replace Windows - Complete commitment to Linux

Virtual Machine is safest for beginners, while dual-boot offers the best balance of performance and safety.

**12. Why is it important to disable Windows Fast Startup before installing Linux in a dual-boot configuration?**

**Answer:**
Windows Fast Startup can cause problems with dual-boot because it doesn't fully shut down Windows - it saves the system state to disk and hibernates. This can cause disk corruption and prevent Linux from properly accessing Windows partitions or seeing Windows in the boot menu.

**13. What is the difference between an absolute path and a relative path?**

**Answer:**
- Absolute path: Starts from root (/) and specifies the complete path to a file or directory (e.g., /home/user/Documents)
- Relative path: Starts from the current directory and specifies the path relative to your current location (e.g., Documents or ../Pictures)

### Discussion Question

**14. Why might someone choose to install Linux in a virtual machine rather than dual-boot? Discuss at least three reasons and their trade-offs.**

**Sample Answer:**
1. **Safety**: VMs are completely isolated from the host system. If something goes wrong, it doesn't affect your main OS. Trade-off: Reduced performance.

2. **Testing/Learning**: VMs are perfect for experimentation. You can snapshot the VM, try something, and roll back if needed. Trade-off: Uses more disk space.

3. **Multiple Distros**: You can run multiple different Linux distributions simultaneously to compare them. Trade-off: Each VM consumes RAM and CPU.

4. **No Risk to Hardware**: VMs don't modify your bootloader or partition table. Trade-off: Can't access full hardware capabilities.

5. **Easy Cleanup**: Deleting a VM is as simple as removing a file. Trade-off: No permanent installation if you need one.

## Answer Key

| Question | Answer | Points |
|----------|--------|--------|
| 1 | C | 1 |
| 2 | B | 1 |
| 3 | B | 1 |
| 4 | D | 1 |
| 5 | C | 1 |
| 6 | True | 1 |
| 7 | False | 1 |
| 8 | True | 1 |
| 9 | True | 1 |
| 10 | True | 1 |
| 11 | Three methods listed | 4 |
| 12 | Fast Startup explanation | 3 |
| 13 | Correct distinction | 3 |
| 14 | Three valid reasons | 5 |

**Total Points:** 27

**Passing Score:** 18/27 (67%)

---

## Instructor Notes

### Common Misconceptions to Address

1. **"VMs are just as fast as native"** - VMs have overhead from virtualization. They're great for testing but not for performance-critical work.

2. **"I don't need to backup before installing"** - Disk operations can and do fail. Always backup before repartitioning.

3. **"All Linux distros install the same way"** - While similar, each distro has its own installer with unique features and options.

4. **"Dual-boot is permanent"** - You can remove Linux later if needed, though it's easier to plan correctly from the start.

### Teaching Tips

- **Live demo**: Show the installation process in a VM during class so students see what to expect
- **Partition visualization**: Use diagrams to show how partitions relate to physical disk space
- **Boot menu**: Emphasize that the boot menu (GRUB) is what makes dual-boot possible
- **Time management**: Installation takes 20-30 minutes - plan accordingly for lab sessions
- **Troubleshooting preparation**: Discuss common issues (WiFi not working, graphics issues) before installation

### Extension Activities

- Have students research different file systems (ext4 vs btrfs vs xfs) and present pros/cons
- Explore the Fedora/Debian installer in a VM without actually installing
- Calculate optimal partition sizes for different use cases (gaming, development, server)
- Compare live USB vs. installation - what can you do from the live environment?
