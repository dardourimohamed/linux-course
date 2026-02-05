# Capstone Solutions: Personal Linux Server Project

## Complete Capstone Solutions

This document provides complete solutions for all parts of the capstone project, with troubleshooting tips and verification commands.

---

## Part 1: Web Server Setup

### Task 1.1: Install nginx

**Fedora Solution:**

```bash
# Install nginx
sudo dnf install nginx

# Start and enable nginx service
sudo systemctl enable --now nginx

# Configure firewall
sudo firewall-cmd --permanent --add-service=http
sudo firewall-cmd --permanent --add-service=https
sudo firewall-cmd --reload
```

**Debian Solution:**

```bash
# Update package index
sudo apt update

# Install nginx
sudo apt install nginx
# Note: nginx starts automatically on Debian
```

**Expected Output (Fedora):**
```
Last metadata expiration check: 0:45:00 ago on Wed 07 Feb 2025 10:00:00
Dependencies resolved.
================================================================================
 Package         Architecture   Version               Repository         Size
================================================================================
Installing:
 nginx           x86_64         1:1.24.0-1.fc39       fedora             1.2 M
...
Complete!
Created symlink /etc/systemd/system/multi-user.target.wants/nginx.service → /usr/lib/systemd/system/nginx.service.
success
success
```

---

### Task 1.2: Verify Installation

**Solution:**

```bash
# Check service status
sudo systemctl status nginx
```

**Expected Output:**
```
● nginx.service - The nginx HTTP and reverse proxy server
     Loaded: loaded (/usr/lib/systemd/system/nginx.service; enabled; preset: enabled)
     Active: active (running) since Wed 2025-02-07 10:15:23 CET; 5s ago
       Docs: man:nginx(8)
   Main PID: 1234 (nginx)
      Tasks: 2 (limit: 38212)
     Memory: 3.2M (peak: 4.5M)
        CPU: 15ms
     CGroup: /system.slice/nginx.service
             ├─1234 "nginx: master process"
             └─1235 "nginx: worker process"
```

```bash
# Check if port 80 is listening
sudo ss -tlnp | grep :80
```

**Expected Output:**
```
LISTEN 0  128          0.0.0.0:80          0.0.0.0:*   users:(("nginx",pid=1234,fd=6))
LISTEN 0  128             [::]:80             [::]:*   users:(("nginx",pid=1234,fd=7))
```

```bash
# Test locally
curl -I http://localhost
```

**Expected Output:**
```
HTTP/1.1 200 OK
Server: nginx/1.24.0
Date: Wed, 07 Feb 2025 10:15:23 GMT
Content-Type: text/html
Connection: keep-alive
```

```bash
# Open in browser
# Navigate to: http://localhost
# Should see default nginx welcome page
```

---

### Task 1.3: Understand nginx Directory Structure

**Directory locations:**

```bash
# Configuration files
ls -la /etc/nginx/
```

**Expected Output:**
```
drwxr-xr-x  2 root root 4096 Feb  7 10:00 .
drwxr-xr-x  3 root root 4096 Feb  1 00:00 ..
-rw-r--r--  1 root root 2345 Feb  7 10:00 nginx.conf
drwxr-xr-x  2 root root 4096 Feb  7 10:00 conf.d
```

```bash
# Web content directory
ls -la /var/www/html/
```

**Expected Output:**
```
total 4
-rw-r--r-- 1 root root 612 Feb  7 10:00 index.html
-rw-r--r-- 1 root root 234 Feb  7 10:00 index.html.backup
```

```bash
# Log files
ls -la /var/log/nginx/
```

**Expected Output:**
```
total 8
-rw-r--r-- 1 nginx adm    0 Feb  7 10:00 access.log
-rw-r--r-- 1 nginx adm    0 Feb  7 10:00 error.log
```

---

## Part 2: Custom Webpage

### Task 2.1: Create Custom Page

**Solution:**

```bash
# Backup the original
sudo cp /var/www/html/index.html /var/www/html/index.html.backup

# Create your custom page
sudo nano /var/www/html/index.html
```

**Paste this HTML:**

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>My Linux Server</title>
    <style>
        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            max-width: 800px;
            margin: 50px auto;
            padding: 20px;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            color: white;
        }
        .container {
            background: rgba(0,0,0,0.3);
            padding: 30px;
            border-radius: 10px;
        }
        h1 { color: #fff; }
        .info { background: rgba(255,255,255,0.1); padding: 15px; margin: 10px 0; border-radius: 5px; }
        .footer { margin-top: 30px; font-size: 0.9em; opacity: 0.8; }
    </style>
</head>
<body>
    <div class="container">
        <h1>Welcome to My Linux Server!</h1>
        <p>This server is proudly running on Linux.</p>

        <div class="info">
            <h3>System Information</h3>
            <ul>
                <li>Operating System: Linux</li>
                <li>Web Server: nginx</li>
                <li>Course: Linux for Everyone</li>
                <li>Status: <strong style="color: #4ade80;">Online</strong></li>
            </ul>
        </div>

        <div class="info">
            <h3>Skills Demonstrated</h3>
            <ul>
                <li>System Administration</li>
                <li>Web Server Configuration</li>
                <li>File Permissions</li>
                <li>Service Management</li>
            </ul>
        </div>

        <div class="footer">
            <p>Powered by Linux | Configured by [Your Name]</p>
        </div>
    </div>
</body>
</html>
```

### Task 2.2: Verify Custom Page

**Solution:**

```bash
# Test with curl
curl http://localhost
```

**Expected Output:**
```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
...
    <h1>Welcome to My Linux Server!</h1>
...
```

```bash
# Or reload in browser
# Navigate to: http://localhost
# Should see your custom page
```

---

## Part 3: Automated Backup Script

### Task 3.1 & 3.2: Create Backup Script

**Complete Solution:**

```bash
# Create backup directory
mkdir -p ~/backups

# Create the backup script
cat > ~/backup-script.sh << 'EOF'
#!/bin/bash

#==============================================
# Linux Server Backup Script
# Course: Linux for Everyone - Capstone Project
#==============================================

# Configuration
BACKUP_DIR="$HOME/backups"
WEB_DIR="/var/www/html"
NGINX_CONF="/etc/nginx"
DATE=$(date +"%Y%m%d_%H%M%S")
BACKUP_NAME="server_backup_$DATE"
LOG_FILE="$BACKUP_DIR/backup.log"

# Create backup directory if it doesn't exist
mkdir -p "$BACKUP_DIR"

# Log function
log() {
    echo "[$(date '+%Y-%m-%d %H:%M:%S')] $1" | tee -a "$LOG_FILE"
}

# Start backup
log "========== Starting Backup: $BACKUP_NAME =========="

# Create backup archive
log "Creating backup archive..."
sudo tar -czf "$BACKUP_DIR/$BACKUP_NAME.tar.gz" \
    -C / \
    "$WEB_DIR" \
    "$NGINX_CONF" \
    2>/dev/null

if [ $? -eq 0 ]; then
    log "Backup created successfully: $BACKUP_NAME.tar.gz"

    # Get file size
    SIZE=$(du -h "$BACKUP_DIR/$BACKUP_NAME.tar.gz" | cut -f1)
    log "Backup size: $SIZE"
else
    log "ERROR: Backup creation failed!"
    exit 1
fi

# Remove backups older than 30 days
log "Cleaning old backups (older than 30 days)..."
find "$BACKUP_DIR" -name "server_backup_*.tar.gz" -mtime +30 -delete
REMAINING=$(find "$BACKUP_DIR" -name "server_backup_*.tar.gz" | wc -l)
log "Backups remaining: $REMAINING"

# Display disk usage
log "Current disk usage of backup directory:"
du -sh "$BACKUP_DIR" | tail -1 | tee -a "$LOG_FILE"

log "========== Backup Complete =========="
echo ""

# List recent backups
echo "Recent backups:"
ls -lht "$BACKUP_DIR"/server_backup_*.tar.gz 2>/dev/null | head -5
EOF

# Make executable
chmod +x ~/backup-script.sh
```

---

### Task 3.3: Test Backup Script

**Solution:**

```bash
# Run manually to test
~/backup-script.sh
```

**Expected Output:**
```
========== Starting Backup: server_backup_20250207_103000 ==========

[2025-02-07 10:30:00] Creating backup archive...
[2025-02-07 10:30:02] Backup created successfully: server_backup_20250207_103000.tar.gz
[2025-02-07 10:30:02] Backup size: 45K
[2025-02-07 10:30:02] Cleaning old backups (older than 30 days)...
[2025-02-07 10:30:02] Backups remaining: 1
[2025-02-07 10:30:02] Current disk usage of backup directory:
45K    /home/user/backups
[2025-02-07 10:30:02] ========== Backup Complete ==========

Recent backups:
-rw-r--r-- 1 user user 45K Feb  7 10:30 server_backup_20250207_103000.tar.gz
```

```bash
# Verify the backup was created
ls -lh ~/backups/
```

**Expected Output:**
```
total 48K
-rw-r--r-- 1 user root  45K Feb  7 10:30 server_backup_20250207_103000.tar.gz
-rw-r--r-- 1 user user 234 Feb  7 10:30 backup.log
```

---

### Task 3.4: Schedule with Cron

**Solution:**

```bash
# Edit crontab
crontab -e

# Add this line to run daily at 2 AM
0 2 * * * /home/$(whoami)/backup-script.sh

# Save and exit
```

```bash
# Verify your crontab
crontab -l
```

**Expected Output:**
```
0 2 * * * /home/user/backup-script.sh
```

**Test cron entry (optional - for immediate testing):**

```bash
# Add test entry to run in 2 minutes
crontab -e
# Add: $(date +%M | awk '{print $1+2%60}') * * * * /home/user/backup-script.sh

# Wait and check that it ran
tail ~/backups/backup.log
```

---

## Part 4: Git Version Control

### Task 4.1: Initialize Git Repository

**Complete Solution:**

```bash
# Create project directory
mkdir -p ~/server-project
cd ~/server-project

# Initialize Git repository
git init
```

```bash
# Create project structure
mkdir -p config docs scripts

# Copy configuration files
sudo cat /etc/nginx/nginx.conf > config/nginx.conf
sudo cat /var/www/html/index.html > config/index.html

# Copy your backup script
cp ~/backup-script.sh scripts/

# Create README.md
cat > README.md << 'EOF'
# Personal Linux Server

## Overview
This is my capstone project for the Linux for Everyone course.

## System Specifications
- OS: Linux (Fedora/Debian)
- Web Server: nginx
- Automation: cron + bash script
- Version Control: Git

## Setup Instructions

### 1. Install nginx
**Fedora:**
\`\`\`bash
sudo dnf install nginx
sudo systemctl enable --now nginx
\`\`\`

**Debian:**
\`\`\`bash
sudo apt install nginx
\`\`\`

### 2. Deploy Web Content
\`\`\`bash
sudo cp config/index.html /var/www/html/index.html
sudo cp config/nginx.conf /etc/nginx/nginx.conf
sudo systemctl reload nginx
\`\`\`

### 3. Setup Backups
\`\`\`bash
chmod +x scripts/backup-script.sh
sudo cp scripts/backup-script.sh /usr/local/bin/
crontab -e  # Add: 0 2 * * * /usr/local/bin/backup-script.sh
\`\`\`

## Maintenance

- Check logs: \`sudo journalctl -u nginx -f\`
- View backups: \`ls -lh ~/backups/\`
- Test web server: \`curl http://localhost\`

## Author
[Your Name] - Linux for Everyone Course
EOF
```

---

### Task 4.2: Make Initial Commit

**Solution:**

```bash
# Configure Git if not already done
git config --global user.name "Your Name"
git config --global user.email "you@example.com"
```

```bash
# Add all files
git add .

# Check status
git status
```

**Expected Output:**
```
On branch main

No commits yet

Changes to be committed:
  (use "git rm --cached <file>..." to unstage)
        new file:   README.md
        new file:   config/index.html
        new file:   config/nginx.conf
        new file:   scripts/backup-script.sh
```

```bash
# Make initial commit
git commit -m "Initial commit: Server configuration and backup script"
```

**Expected Output:**
```
[main (root-commit)] Initial commit: Server configuration and backup script
 4 files changed, 234 insertions(+)
 create mode 100644 README.md
 create mode 100644 config/index.html
 create mode 100644 config/nginx.conf
 create mode 100644 scripts/backup-script.sh
```

```bash
# Verify
git log --oneline
```

**Expected Output:**
```
a1b2c3d Initial commit: Server configuration and backup script
```

---

## Part 5: Docker Challenge (Optional)

### Task 5.1-5.3: Run nginx in Docker

**Complete Solution:**

```bash
# Install Docker if not already installed
sudo dnf install docker
sudo systemctl enable --now docker
sudo usermod -aG docker $USER
newgrp docker  # or logout and login
```

```bash
# Stop nginx service
sudo systemctl stop nginx
sudo systemctl disable nginx
```

```bash
# Create data directory
mkdir -p ~/docker-nginx/html

# Copy your custom page
cp ~/server-project/config/index.html ~/docker-nginx/html/

# Run nginx container
docker run -d \
  --name my-nginx \
  -p 80:80 \
  -v ~/docker-nginx/html:/usr/share/nginx/html:ro \
  nginx:alpine
```

**Expected Output:**
```
a1b2c3d4e5f67890abc123def45678901234567890abcdef123456
```

```bash
# Verify
curl http://localhost
```

```bash
# View logs
docker logs my-nginx
```

**Expected Output:**
```
/docker-entrypoint.sh: /docker-entrypoint.d/ is not empty
...
2025/02/07 10:30:00 [notice] 1#1: start worker process 1
```

```bash
# Enable container on boot
sudo nano /etc/systemd/system/docker-nginx.service
```

**Add this content:**

```ini
[Unit]
Description=Docker nginx container
Requires=docker.service
After=docker.service

[Service]
Restart=always
ExecStart=/usr/bin/docker start -a my-nginx
ExecStop=/usr/bin/docker stop -t 2 my-nginx

[Install]
WantedBy=multi-user.target
```

```bash
# Enable the service
sudo systemctl daemon-reload
sudo systemctl enable docker-nginx
sudo systemctl start docker-nginx
```

```bash
# Verify
systemctl status docker-nginx
```

---

## Verification Checklist Solutions

### Web Server Verification

```bash
# [ ] nginx is running
sudo systemctl status nginx
# Expected: active (running)

# [ ] Website is accessible
curl -I http://localhost
# Expected: HTTP/1.1 200 OK

# [ ] Custom page displays correctly
curl http://localhost | grep "My Linux Server"
# Expected: Should find the text
```

### Backup System Verification

```bash
# [ ] Backup script exists and is executable
ls -l ~/backup-script.sh
# Expected: -rwxr-xr-x

# [ ] Backup directory exists
ls -ld ~/backups
# Expected: drwxr-xr-x

# [ ] Recent backup exists
ls -lht ~/backups/ | head -2
# Expected: Recent .tar.gz file

# [ ] Cron job is scheduled
crontab -l | grep backup-script
# Expected: 0 2 * * * /home/user/backup-script.sh
```

### Git Repository Verification

```bash
# [ ] Repository initialized
cd ~/server-project && git status
# Expected: On branch main, nothing to commit

# [ ] All files tracked
git ls-files
# Expected: README.md, config/, scripts/

# [ ] Commit history exists
git log --oneline
# Expected: At least one commit
```

### Docker Verification (if applicable)

```bash
# [ ] Docker container running
docker ps | grep nginx
# Expected: Container with name my-nginx

# [ ] Container accessible
curl http://localhost
# Expected: HTTP response

# [ ] Container restarts with system
systemctl status docker-nginx
# Expected: enabled and active
```

---

## Documentation Solution

### Task: Create System Documentation

**Complete Solution:**

```bash
cd ~/server-project/docs

cat > SETUP.md << 'EOF'
# Server Setup Documentation

## System Information
- **Hostname:** $(hostname)
- **Distribution:** $(cat /etc/os-release | grep PRETTY_NAME | cut -d'"' -f2)
- **Kernel:** $(uname -r)
- **IP Address:** $(hostname -I | awk '{print $1}')
- **Date Configured:** $(date)

## Web Server Configuration

### nginx Installation
- **Service:** nginx
- **Version:** $(nginx -v 2>&1 | cut -d'/' -f2)
- **Config Location:** /etc/nginx/nginx.conf
- **Web Root:** /var/www/html/
- **Port:** 80

### Management Commands
\`\`\`bash
# Check status
sudo systemctl status nginx

# Restart service
sudo systemctl restart nginx

# View logs
sudo journalctl -u nginx -f

# Test configuration
sudo nginx -t
\`\`\`

## Backup Configuration

### Backup Script
- **Location:** ~/backup-script.sh
- **Backup Directory:** ~/backups/
- **Schedule:** Daily at 2:00 AM
- **Retention:** 30 days

### Manual Backup
\`\`\`bash
~/backup-script.sh
\`\`\`

### Restore Procedure
\`\`\`bash
# Extract backup
tar -xzf backups/server_backup_YYYYMMDD_HHMMSS.tar.gz -C /

# Restart services
sudo systemctl restart nginx
\`\`\`

## Git Repository

### Repository Structure
\`\`\`
~/server-project/
├── config/          # Configuration files
├── docs/           # Documentation
├── scripts/        # Automation scripts
└── README.md       # Project overview
\`\`\`

### Commands
\`\`\`bash
# View status
cd ~/server-project && git status

# View log
git log --oneline

# Add changes
git add .
git commit -m "Update configuration"
\`\`\`

## Troubleshooting

### Web Server Issues
1. Check if nginx is running: \`sudo systemctl status nginx\`
2. Check firewall: \`sudo firewall-cmd --list-all\`
3. Check logs: \`sudo journalctl -u nginx -n 50\`
4. Test config: \`sudo nginx -t\`

### Backup Issues
1. Check script permissions: \`ls -l ~/backup-script.sh\`
2. Check cron: \`crontab -l\`
3. Check backup log: \`cat ~/backups/backup.log\`

### Docker Issues (if applicable)
1. Check container: \`docker ps -a\`
2. View logs: \`docker logs my-nginx\`
3. Restart: \`docker restart my-nginx\`
EOF
```

---

## Common Capstone Problems and Solutions

| Problem | Diagnosis | Solution |
|---------|-----------|----------|
| nginx won't start | Port 80 already in use | \`sudo netstat -tlnp | grep :80\` then stop conflicting service |
| Can't edit /var/www/html | Permission denied | \`sudo chown -R $USER:$USER /var/www/html\` |
| Backup script fails | Can't write to backup dir | \`mkdir -p ~/backups && chmod 755 ~/backups\` |
| cron not running | Check cron service | \`sudo systemctl status cron\` |
| Git permission errors | Wrong file ownership | \`sudo chown -R $USER:$USER ~/server-project\` |
| Docker permission denied | User not in docker group | \`sudo usermod -aG docker $USER\` then logout/login |
| Container can't access web files | Wrong volume mount | Check bind mount path and permissions |

---

## Presentation Tips

### Live Demonstration Structure

1. **Show your web server** (2 minutes)
   - Open localhost in browser
   - Show custom page
   - Explain nginx configuration

2. **Demonstrate Git workflow** (3 minutes)
   - \`git log --oneline\`
   - Make a small change
   - \`git add` and \`git commit\`

3. **Run backup manually** (2 minutes)
   - Execute ~/backup-script.sh
   - Show results
   - Check backup log

4. **Show automation** (2 minutes)
   - Display crontab
   - Explain cron schedule

5. **(Optional) Docker** (2 minutes)
   - Show running containers
   - Explain volume mapping

### Explanation Framework

**Why nginx?**
- Industry standard for web serving
- Lightweight and fast
- Easy to configure

**Why cron?**
- Built-in reliability
- Simple and well-tested
- No additional dependencies

**Why Git?**
- Version control for configuration
- Change tracking and rollback
- Professional documentation

---

## Extension Solutions

### Add SSL/TLS

```bash
# Install certbot
sudo dnf install certbot python3-certbot-nginx  # Fedora
sudo apt install certbot python3-certbot-nginx   # Debian

# Get certificate (requires domain)
sudo certbot --nginx -d yourdomain.com
```

### Add Monitoring

```bash
# Install htop
sudo dnf install htop  # Fedora
sudo apt install htop   # Debian

# Run htop for monitoring
htop
```

### Multiple Pages

```bash
# Create about page
sudo cp /var/www/html/index.html /var/www/html/about.html

# Edit about page
sudo nano /var/www/html/about.html
# Change title and content

# Access at http://localhost/about.html
```

---

## Instructor Notes

### Teaching Tips

1. **Live setup week**: Walk through entire capstone during class
2. **Incremental verification**: Check each component as students complete it
3. **Peer review**: Have students present to each other before final presentation
4. **Troubleshooting practice**: Intentionally break something and fix it
5. **Documentation emphasis**: Good docs are as important as working system

### Assessment Criteria

**Successful capstone should demonstrate:**
- [ ] Web server accessible and serving custom content
- [ ] Backup script runs successfully and creates archives
- [ ] Cron job is properly configured
- [ ] Git repository tracks all configuration files
- [ ] Student can explain their design choices
- [ ] System is documented and maintainable

### Common Issues Timeline

| Week | Common Issues | Solutions |
|------|---------------|------------|
| Week 1-2 | nginx installation fails | Firewall issues, SELinux, port conflicts |
| Week 2-3 | Can't edit web files | Permission issues with /var/www/html |
| Week 3-4 | Backup script fails | Sudo password prompts, path errors |
| Week 4-5 | Cron not running | Incorrect cron syntax, permissions |
| Week 5-6 | Git commit issues | Large files, binary files, merge conflicts |

### Final Presentation Checklist

**Before presenting, ensure:**
- [ ] Tested all verification commands
- [ ] Prepared demo environment (clean terminal, browser ready)
- [ ] Backup script tested recently
- [ ] Git repository is clean and up to date
- [ ] Documentation is complete and accurate
- [ ] Can answer questions about design choices
- [ ] Have fallback plan if demo fails (screenshots, logs)
