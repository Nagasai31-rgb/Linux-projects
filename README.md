# Linux
 ## Linux is an open-source operating system (OS) used to run servers, computers, mobile devices, cloud platforms, networking systems, and even supercomputers.

  ## It is known for being fast, secure, stable, and free. 

## 🐧 What is Linux?

Linux is an operating system just like:

> Windows

 > macOS

> Android

But Linux is open-source, meaning its source code is free and can be modified by anyone

 ## ⭐ Why DevOps Engineers Prefer Linux

 > Free and open-source

 > Extremely stable

 > Very secure

 > Easy automation (shell scripts, cron, systemd)

 > Works perfectly with cloud tools

 > Excellent for performance monitoring & troubleshooting

 ## **🐧 Linux – Real-Time Use Case: Server Setup & Automation **

# **  ⭐ Level 1 – Basic (Foundational Skills) **

 ###  ✅ Set up users, groups for dev team
 ````bash
sudo groupadd devteam
sudo useradd -m -s /bin/bash alice
sudo useradd -m -s /bin/bash bob
sudo usermod -aG devteam alice
sudo usermod -aG devteam bob
````
![alt text](evidence/{C0FA7AC9-AA13-4333-9E91-1F3EE11DD747}.png)

![alt text](evidence/{65697E71-00B6-423E-9854-0579C919A6F2}.png)
### Set passwords
````bash
sudo passwd alice
sudo passwd bob
````
![alt text](image.png)




## ✅  Manage permissions for project directories ##
````bash
sudo mkdir -p /opt/app
sudo chown :devteam /opt/app
sudo chmod 770 /opt/app
````
![alt text](evidence/{EDFED68E-476E-4E5D-8B85-AD048C56C877}.png)

 ###  ✅ Install required packages (git, nginx, java)
 ````bash
 sudo yum  update -y
 sudo yum  install -y git
 sudo yum  install -y nginx
 sudo yum  install -y java*
````
![alt text](evidence/{5E4335DC-6625-416C-BD2E-5FFE58C05FC2}.png)
![alt text](evidence/{0C6CD40B-7B1C-4BF3-B031-1B99DD2E504B}.png)
![alt text](evidence/{3D26B01D-5836-41F5-B100-A9A3E515F965}.png)
###  ✅ start & enable service
```bash
sudo systemctl start  nginx
sudo systemctl enable nginx
sudo systemctl status nginx
```
![alt text](evidence/{ACA33091-A57B-4D16-B7AC-FDEB7E9F9F93}.png)
### ✅  Check system info (memory, CPU, disks)
````bash
free -h         # RAM
lscpu           # CPU
df -h           # Disk usage
lsblk           # Block devices
````
![alt text](evidence/{DA584E53-995D-4CB6-AFFC-7BA00302C11D}.png)
![alt text](evidence/{CDD06C1D-3851-45FE-8386-37B0015CE7BD}.png)
# ** ⭐ Level 2 – Intermediate (Daily DevOps Tasks) **

 ###  ✅ Automate backups with Cron
 ### Backup /opt/app every day at 2 AM
 ### Create a script
 ````bash
 sudo vi /usr/local/bin/backup_app.sh
 ````
 ![alt text](evidence/{73B12F00-1402-4591-A121-E71AD3C57C54}.png)
 ### paste the script
 ````bash
#!/bin/bash


# Create /backup if it doesn't exist
mkdir -p /backup

# Create backup file with today’s date
tar -czf /backup/app-$(date +%F).tar.gz /opt/app

# Delete backups older than 7 days
find /backup -type f -mtime +7 -delete

````
![alt text](evidence/{0CD80795-6680-4696-8451-A64BEA0429A3}.png)
### Make it executable
````bash
sudo chmod +x /usr/local/bin/backup_app.sh
````
### Add cron job
 ````bash
 crontab -e
````
##
 Add:
````bash
0 2 * * * /usr/local/bin/backup_app.sh

````

![alt text](evidence/{EE445FAF-B401-4818-9E72-B4BC50F2E9ED}.png)
![alt text](evidence/{A3EA55CB-AB52-4DDB-8DB7-DE5FEE3CB119}.png)
##  Create shell scripts: Log cleanup, service restart, health checks
###  ✅ a) Log cleanup
````bash
#!/bin/bash
find /var/log/app/ -type f -mtime +7 -exec rm -f {} \;
````
![alt text](evidence/{C8BE3620-D6BB-45A7-87DC-615399BAB986}.png)
###  ✅ b) service restart
``` bash
#!/bin/bash
systemctl restart nginx
systemctl status nginx
```
![alt text](evidence/{BD97C636-2700-4ED0-B3A5-E78B7CC0EFEE}.png)
### ✅  c) Health checks
```bash
#!/bin/bash
curl -I http://localhost:80 
```
![alt text](evidence/{D5A1E7F5-6CC7-4ACD-84FB-0BD6A77E0632}.png)

### ✅ Manage logs under /var/log
````bash
tail -f /var/log/nginx/access.log
tail -f /var/log/nginx/error.log
journalctl -u nginx
du -sh /var/log/*
````
![alt text](evidence/{3E833D8D-BB74-47DC-820D-F8B3644068B0}.png)
![alt text](evidence/{CB576A16-9ED7-47C0-BC18-A0EB3BF807E0}.png)


### ✅  Monitor System Performance
````bash
top
htop
iostat
vmstat 5
````
![alt text](evidence/{1A35ACB0-43F6-4C25-A843-CC14CE09169E}.png)
![alt text](evidence/{C049762A-AEAA-4D24-8656-5F7345C9D4E7}.png)
##  ✅ Troubleshoot services
###  1. Check service status
This tells you if a service is running, stopped, or failed.
````bash
systemctl status nginx
````
Look for:

- active (running) → OK

- inactive (dead) → not running

- failed → something broke
![alt text](evidence/{EA10349E-964E-432F-AB67-99FD76E5C06E}.png)
### ✅ 2. Start or restart the service
````bash
sudo systemctl start nginx
sudo systemctl restart nginx
````
![alt text](evidence/{98F6DA66-65BA-4688-B3D6-25E6BCA3DBA5}.png)
### ✅ 3. Check service logs (journalctl)
````bash
sudo journalctl -u nginx -f
````
![alt text](evidence/{C143B807-B4F1-4CED-A93F-6BAFFFCA4F77}.png)
# ** ⭐ Level 3 – Advanced (Production-Ready Linux Admin) **
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
sudo systemctl enable nginx
sudo systemctl start nginx
````
![alt text](evidence/{74219922-7BE3-45E7-82E4-E6FA314B0FD6}.png)
### ✅ 2. SSH Hardening
### Edit
````bash
sudo vi  /etc/ssh/sshd_config
````
### Apply
````bash
PermitRootLogin no
PasswordAuthentication no
AllowUsers alice bob 
````
![alt text](evidence/{51FEDA18-8987-4A25-B21B-D7E0993C7268}.png)
![alt text](evidence/{FE20BC5A-8EDF-4FD5-8391-396722D9EA79}.png)

### Restart SSH:
````bash
sudo systemctl restart sshd
````
![alt text](evidence/{B9235A91-D9C1-4AF3-BD7F-134782700CA0}.png)
## ✅ 3. LVM Setup for Storage Scaling
### Install LVM tools
````bash
sudo yum install -y lvm2
````
![alt text](evidence/{77D5AB75-3BFF-4CAE-8642-D23A53017947}.png)

To create an LV, first you must create:

## 1️⃣ physical volume (PV)
## 2️⃣ volume group (VG)
## 3️⃣ Logical volume (LV)


### ✅ STEP 1 — Identify your disk**

Run:

```bash
lsblk
```
![alt text](evidence/{C56AA7AE-4A76-44E6-884F-FA092F039425}.png)
````bash
nvme0n1        8G   (main disk)
├─nvme0n1p1    8G   mounted on /
├─nvme0n1p127  1M
└─nvme0n1p128 10M   mounted on /boot/efi
````
✅ If you want to learn or practice LVM → You must add a new disk
🚀 Next Steps (Do This)
STEP 1: Add a New Disk in AWS

Go to:
EC2 → Instances → Select instance →
Actions → Storage → Attach EBS volume

Create a new volume:

Size: 5GB / 10GB

Type: gp3

Availability Zone: Same as instance

Attach → it will appear as:
````bash
/dev/nvme1n1
````
Look for a free disk like `/dev/nvme1n1`.

---

## ✅ STEP 2 — Create Physical Volume

Assume your free disk is `/dev/nvme1n1`
(If your disk is different, replace it!)

```bash
sudo pvcreate /dev/nvme1n1
```

---

## ✅ STEP 3 — Create Volume Group

```bash
sudo vgcreate vgdata /dev/nvme1n1
```

Now check:

```bash
vgs
```

You should see `vgdata`.

---

## ✅ STEP 4 — Create Logical Volume

Now your earlier command will work:

```bash
sudo lvcreate -L 10G -n lvapp vgdata
```

---

## ✅ STEP 5 — Format the LV

```bash
sudo mkfs.ext4 /dev/vgdata/lvapp
```

---

## ✅ STEP 6 — Mount it

```bash
sudo mkdir /mnt/appdata
sudo mount /dev/vgdata/lvapp /mnt/appdata
````


## ✅ Install and Enable Firewalld on Amazon Linux 2023

Run these commands:

### 1. Install firewalld

```bash
sudo dnf install firewalld -y
```
![alt text](evidence/{A0926D93-C25B-46C8-A9B7-54D3145B180D}.png)

## 2. Enable firewalld to start at boot

```bash
sudo systemctl enable firewalld
```

## 3. Start firewalld

```bash
sudo systemctl start firewalld
```

## 4. Check the status

```bash
sudo systemctl status firewalld
```
![alt text](evidence/{5E207CBD-DD35-49AD-A921-FC182DA6E7F5}.png)

Here is a **perfect logrotate configuration for Nginx**, including rotation, compression, permissions, and automatic Nginx reload.

---

## ✅ Nginx Logrotate Configuration (Recommended)

Create or edit the config file:

```bash
sudo vi  /etc/logrotate.d/nginx
```

Paste this full configuration:

```bash
/var/log/nginx/*.log {
    daily                  # rotate logs daily
    missingok              # ignore if log file is missing
    rotate 14              # keep 14 days of logs
    compress               # compress rotated logs (.gz)
    delaycompress          # compress on next cycle
    notifempty             # skip rotating empty logs
    sharedscripts

    postrotate
        # Only reload nginx if it is running
        if pidof nginx > /dev/null; then
            systemctl reload nginx > /dev/null 2>&1;
        fi
    endscript
}
```

---

##  What this config does

| Setting         | Meaning                                                    |
| --------------- | ---------------------------------------------------------- |
| `daily`         | Rotate logs every day                                      |
| `rotate 14`     | Keep logs for 14 days                                      |
| `compress`      | Old logs become `.gz` (saves disk space)                   |
| `delaycompress` | Compress next day (keeps 1 day uncompressed for debugging) |
| `notifempty`    | Skip empty logs                                            |
| `missingok`     | No error if log not found                                  |
| `sharedscripts` | Run postrotate only once                                   |
| `postrotate`    | Reload Nginx so it reopens fresh log files                 |

---

## 🔧 Test the config

## 1. Debug mode (no rotation, safe test) ##

```bash
sudo logrotate -dv /etc/logrotate.d/nginx
```

## 2. Force rotate now (for checking)

```bash
sudo logrotate -fv /etc/logrotate.d/nginx
```

Your logs will become:

``` bash 
access.log
access.log-20251201.gz
error.log
error.log-20251201.
````
![alt text](evidence/{9D16E513-FE92-4FC1-8675-630BAC1001C2}.png)

































