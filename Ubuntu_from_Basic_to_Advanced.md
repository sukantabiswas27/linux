# Ubuntu from Basic to Advanced

## 🔹 Level 1: Ubuntu Basics (Beginner)

### 1. **Understanding Ubuntu**
- Ubuntu = Linux distribution (based on Debian).
- Everything is **file + process**.
- Root (`/`) is the base directory.

### 2. **Basic Commands**
```bash
pwd           # show current directory
ls            # list files
ls -l         # detailed list
cd /path      # change directory
touch file    # create empty file
mkdir dir     # create directory
rm file       # remove file
rm -r dir     # remove directory
```

### 3. **File Viewing & Editing**
```bash
cat file      # view file content
less file     # scroll file
head -n 10 file  # first 10 lines
tail -n 10 file  # last 10 lines
nano file     # edit with nano
vim file      # edit with vim
```

### 4. **User & Permissions**
```bash
whoami        # current user
id            # user + groups
sudo command  # run as root
chmod 755 file  # permissions
chown user:group file  # change ownership
```

---

## 🔹 Level 2: System Management (Intermediate)

### 1. **Package Management (APT)**
```bash
sudo apt update              # update package list
sudo apt upgrade             # upgrade packages
sudo apt install nginx       # install a package
sudo apt remove nginx        # remove a package
dpkg -l | grep nginx         # check installed packages
```

### 2. **Processes**
```bash
ps aux          # list processes
top             # live process monitor
htop            # (better top, install first)
kill -9 PID     # kill a process
```

### 3. **Services (systemd)**
```bash
systemctl status nginx   # check service status
systemctl start nginx    # start service
systemctl stop nginx     # stop service
systemctl enable nginx   # start on boot
```

### 4. **Networking**
```bash
ip a              # show IP addresses
ping google.com   # test connectivity
curl ifconfig.me  # check public IP
netstat -tulnp    # listening ports
```

---

## 🔹 Level 3: Advanced Ubuntu (Pro/Admin)

### 1. **Disk Management**
```bash
df -h             # check disk usage
du -sh *          # size of files/folders
lsblk             # list block devices
mount /dev/sdb1 /mnt   # mount disk
umount /mnt
```

### 2. **Users & Groups**
```bash
sudo adduser newuser
sudo usermod -aG sudo newuser
sudo deluser newuser
```

### 3. **SSH & Remote Access**
```bash
sudo apt install openssh-server
systemctl enable ssh
ssh user@server-ip
```

### 4. **Firewall (UFW)**
```bash
sudo apt install ufw
sudo ufw enable
sudo ufw allow 22/tcp      # allow SSH
sudo ufw allow 80/tcp      # allow HTTP
sudo ufw status
```

### 5. **Logs & Monitoring**
```bash
journalctl -xe            # system logs
tail -f /var/log/syslog   # real-time logs
dmesg                     # kernel logs
```

### 6. **Shell Scripting**
```bash
#!/bin/bash
echo "Hello, Ubuntu"
date
uptime
```
Save as `myscript.sh`, then:
```bash
chmod +x myscript.sh
./myscript.sh
```

---

## 🔹 Level 4: Expert Topics (Server Admin)

- **LAMP/LEMP Stack** → Apache/Nginx + MySQL + PHP
- **Docker & Containers** → Deploy apps
- **Virtualization** → KVM, VirtualBox
- **Automation** → Cron jobs, Ansible
- **Security** → Fail2ban, SSL certificates
- **Performance** → systemctl tuning, caching
- **Cloud** → Ubuntu on AWS/Azure/Google Cloud
