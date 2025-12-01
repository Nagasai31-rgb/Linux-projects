# Linux-projects
## 🐧 1. Linux – Real-Time Use Case: Server Setup & Automation

 ### 🧩 Linux – Demo

##  ⭐ Level 1 – Basic (Foundational Skills)
 ###  ✅ Set up users, groups for dev team
 ````bash
sudo groupadd devteam
sudo useradd -m -s /bin/bash alice
sudo useradd -m -s /bin/bash bob
sudo usermod -aG devteam alice
sudo usermod -aG devteam bob
### ✅  Manage permissions for project directories
````bash
sudo mkdir -p /opt/app
sudo chown :devteam /opt/app
sudo chmod 770 /opt/app
````
 ###  ✅ Install required packages (git, nginx, java)
 ````bash
sudo apt update -y
sudo apt install -y git nginx openjdk-17-jdk
````

### ✅  Check system info (memory, CPU, disks)
````bash
free -h         # RAM
lscpu           # CPU
df -h           # Disk usage
lsblk           # Block devices
````
## ⭐ Level 2 – Intermediate (Daily DevOps Tasks)


 ###  ✅ Automate backups with Cron
 ###
 Backup /opt/app every day at 2 AM
 ````bash
 crontab -e
````
##
 Add:
````bash
0 2 * * * tar -czf /backup/app-$(date +\%F).tar.gz /opt/app
````
##  Create shell scripts: Log cleanup, service restart, health checks
###  ✅ a) Log cleanup
````bash
#!/bin/bash
find /var/log/app/ -type f -mtime +7 -exec rm -f {} \;
````
###  ✅ b) service restart
``` bash
#!/bin/bash
systemctl restart myapp
systemctl status myapp
```
### ✅  c) Health checks
```bash
#!/bin/bash
curl -I http://localhost:8080 || systemctl restart myapp

```
##  ✅ Manage logs under /var/log
````bash
cd /var/log
tail -f syslog
journalctl -u nginx
du -sh /var/log/*
````
## ✅  Monitor System Performance
````bash
top
htop
iostat
vmstat 5
````
##  ✅ Troubleshoot services
````bash
systemctl status nginx
journalctl -xe
````
## ⭐ Level 3 – Advanced (Production-Ready Linux Admin)
### ✅ 1. Create Custom systemd Service
File: /etc/systemd/system/myapp.service
````bash
[Unit]
Description=My Application
After=network.target

[Service]
User=dev1
WorkingDirectory=/opt/myapp
ExecStart=/usr/bin/java -jar app.jar
Restart=always

[Install]
WantedBy=multi-user.target
````
## Enable & start:
```bash
sudo systemctl daemon-reload
sudo systemctl enable myapp
sudo systemctl start myapp
````
### ✅ 2. SSH Hardening
### Edit
````bash
sudo nano /etc/ssh/sshd_config
````
### Apply
````bash
PermitRootLogin no
PasswordAuthentication no
AllowUsers dev1 dev2
````
### Restart SSH:
````bash
sudo systemctl restart sshd
````
## ✅ 3. LVM Setup for Storage Scaling












