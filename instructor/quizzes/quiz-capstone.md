# Capstone Quiz: Personal Linux Server Project

## Quiz Questions

### Multiple Choice

**1. What is the default port for HTTP web traffic?**
- A) 22
- B) 80
- C) 443
- D) 8080

**Answer:** B

**2. Which command schedules a script to run daily at 2 AM?**
- A) at 2:00
- B) batch 2:00
- C) cron (add: 0 2 * * *)
- D) systemd timer

**Answer:** C

**3. What command backs up files while preserving permissions?**
- A) cp -p
- B) cp -v
- C) cp -r
- D) tar -czf

**Answer:** A

**4. In the capstone project, what is the purpose of tracking configuration with Git?**
- A) To automatically deploy to production
- B) Version control and change tracking
- C) To share on GitHub
- D) To automatically fix errors

**Answer:** B

**5. What does the -z flag do in the tar command?**
- A) Extract files
- B) Compress with gzip
- C) List contents
- D) Preserve permissions

**Answer:** B

### True/False

**6. True or False: nginx can serve static HTML files from /var/www/html.**

**Answer:** True

**7. True or False: The capstone backup script uses cron to automate daily backups.**

**Answer:** True

**8. True or False: Systemd can automatically start Docker containers at boot.**

**Answer:** True (with custom service files)

**9. True or False: The verification checklist is optional in the capstone.**

**Answer:** False (it's essential)

**10. True or False: The Docker challenge is required for capstone completion.**

**Answer:** False (it's optional/extension)

### Short Answer

**11. What are the four main components of the capstone project, and what does each demonstrate?**

**Answer:**
1. **Web Server (nginx)**: System administration, service management, networking, understanding of web architecture
2. **Custom Webpage**: File management, basic HTML/CSS, documentation, ownership of a working system
3. **Automated Backup Script**: Shell scripting, automation, cron scheduling, data management, system maintenance
4. **Git Version Control**: Professional development practices, change tracking, documentation, rollback capability
5. **(Optional) Docker**: Containerization, modern deployment practices, advanced system administration

**12. Explain the role of systemd in the Docker challenge portion of the capstone.**

**Answer:**
In the Docker challenge, systemd is used to:
- **Start containers automatically at boot**: Without this, containers must be manually started after each reboot
- **Manage container lifecycle**: Start, stop, restart containers like any system service
- **Provide monitoring**: systemctl status shows container status
- **Enable standard service management**: Use familiar commands (start, stop, enable) instead of docker commands

**How it works:**
```bash
# Create systemd service file
sudo nano /etc/systemd/system/docker-nginx.service

# Enable and start
sudo systemctl enable docker-nginx
sudo systemctl start docker-nginx
```

This bridges the gap between Docker containers and traditional system administration, making containers behave like native services.

**13. What is the purpose of the verification checklist in the capstone?**

**Answer:**
The verification checklist ensures that:
1. **All components work correctly**: Web server accessible, backups running, Git tracking changes
2. **Quality standards met**: Each part of the project meets requirements
3. **Documentation is complete**: Student can explain what they built and why
4. **Ready for presentation**: System is demo-ready and can be shown to others
5. **Learning outcomes achieved**: Demonstrates mastery of course material

The checklist transforms a "mostly working" system into a polished, professional demonstration. It teaches students to validate their work systematically - a critical skill in real-world system administration.

### Discussion Question

**14. The capstone project integrates skills from all 13 chapters of the course. Choose three different chapters and explain how the capstone project applies the skills learned in each. Discuss how this integrated project demonstrates that you have achieved the learning objectives of the course.**

**Sample Answer:**

**Chapter 4: File System & Navigation**
- **Skills**: Understanding directories, file paths, permissions
- **Capstone Application**:
  - Creating project structure: `mkdir -p ~/server-project/{config,docs,scripts}`
  - Navigating to system directories: `/var/www/html`, `/etc/nginx`
  - Understanding where files live and how to organize them
  - Using proper paths in backup script
- **Demonstrates**: Can navigate and organize the Linux file system effectively

**Chapter 7: Permissions & Users**
- **Skills**: chmod, chown, understanding permission model
- **Capstone Application**:
  - Setting correct permissions: `chmod +x backup-script.sh`
  - Managing nginx web root: `sudo chown -R $USER:www-data /var/www/html`
  - Understanding why web files need specific permissions
  - Troubleshooting "permission denied" errors
- **Demonstrates**: Can secure a system and manage access controls

**Chapter 10: Shell Scripting**
- **Skills**: Writing scripts, variables, conditionals, cron
- **Capstone Application**:
  - Created complete backup script with logging and error handling
  - Using variables for configuration: `BACKUP_DIR`, `DATE`, `LOG_FILE`
  - Automated with cron: `0 2 * * * /home/user/backup-script.sh`
  - Bash functions for clean, reusable code
- **Demonstrates**: Can automate system administration tasks

**Chapter 9: Processes & Services**
- **Skills**: systemctl, service management, journalctl
- **Capstone Application**:
  - Managing nginx: `systemctl enable --now nginx`
  - Checking service status: `systemctl status nginx`
  - Viewing logs: `journalctl -u nginx -f`
  - Troubleshooting failed services
- **Demonstrates**: Can manage system services and diagnose problems

**Chapter 12: Git Version Control**
- **Skills**: Git workflow, commits, documentation
- **Capstone Application**:
  - Tracking configuration changes over time
  - Documenting setup with README.md
  - Creating proper commit messages
  - Maintaining history of system changes
- **Demonstrates**: Uses professional version control practices

**Chapter 13: Docker (Optional Challenge)**
- **Skills**: Containers, images, volumes, docker-compose
- **Capstone Application**:
  - Running nginx in container: `docker run -d -p 80:80 nginx`
  - Understanding port mapping and volumes
  - Bridging containers with systemd
  - Modern deployment practices
- **Demonstrates**: Can work with containerized applications

**Course Achievement:**

This integrated project proves the student can:
1. **Apply multiple skills simultaneously**: Real-world tasks require combining knowledge
2. **Troubleshoot independently**: Problems arise that require chapter-spanning solutions
3. **Document technical systems**: Professional documentation is essential
4. **Take ownership of a system**: From installation to automation to maintenance
5. **Think like a sysadmin**: Proactive management, not reactive fixing

**Conclusion**: The capstone is more than sum of its parts - it transforms isolated skills into practical system administration ability. Students who complete the capstone are ready to manage real Linux systems.

## Answer Key

| Question | Answer | Points |
|----------|--------|--------|
| 1 | B | 1 |
| 2 | C | 1 |
| 3 | A | 1 |
| 4 | B | 1 |
| 5 | B | 1 |
| 6 | True | 1 |
| 7 | True | 1 |
| 8 | True | 1 |
| 9 | False | 1 |
| 10 | False | 1 |
| 11 | Four components explained | 4 |
| 12 | Systemd role explained | 3 |
| 13 | Checklist purpose explained | 3 |
| 14 | Three chapters integrated | 5 |

**Total Points:** 27

**Passing Score:** 18/27 (67%)

---

## Instructor Notes

### Common Capstone Issues

1. **"nginx won't start"** - Usually firewall, SELinux, or port conflicts
2. **"Cron isn't running"** - Check crontab with `crontab -l`, verify script has shebang
3. **"Git permission denied"** - Configure files properly before committing
4. **"Docker container stops on reboot"** - Need systemd service (optional challenge)

### Teaching Tips

- **Live demo week**: Walk through entire capstone setup during class
- **Incremental verification**: Check each component as students complete it
- **Peer review**: Have students present to each other before final presentation
- **Troubleshooting practice**: Intentionally break something and have students fix it
- **Documentation emphasis**: Good docs are as important as working system

### Extension Activities

- Add SSL/TLS with Let's Encrypt
- Set up monitoring with GoAccess or Prometheus
- Create multiple virtual hosts
- Add database backend (PostgreSQL or MySQL)
- Set up automated deployment from Git
- Configure firewall rules for security
- Create a dashboard for server monitoring
