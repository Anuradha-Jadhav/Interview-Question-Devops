---

## **Linux Basics**  

### **Cron Job**  
A cron job is a scheduled task in Linux that runs automatically at specified times. It is defined using the `crontab` file.  

**Example:** Run a backup script daily at midnight  
```bash
0 0 * * * /home/user/backup.sh
```  

### **Logs in Linux**  
Logs are stored in `/var/log/` and help with troubleshooting.  
- `syslog` - General system logs.  
- `auth.log` - Authentication logs.  
- `dmesg` - Kernel logs.  

**View logs:**  
```bash
tail -f /var/log/syslog
journalctl -xe
```  

### **What is `echo $?`?**  
It prints the exit status of the last executed command.  
- `0` - Success  
- `1` - General error  
- `127` - Command not found  

Example:  
```bash
ls /nonexistent
echo $?   # Output: 2 (File not found)
```  

### **Environmental Variables**  
- System variables like `$PATH`, `$HOME`, `$USER`.  
- Set a variable: `export MYVAR="Hello"`  
- View a variable: `echo $MYVAR`  
- Persist variables in `~/.bashrc` or `~/.bash_profile`.  

### **User Management**  
- Add user: `useradd username`  
- Set password: `passwd username`  
- Delete user: `userdel -r username`  
- View users: `cat /etc/passwd`  

### **How to Set Expiry Date for a User**  
```bash
chage -E 2025-12-31 username
```
To check expiry:  
```bash
chage -l username
```  

### **SSH (Secure Shell)**  
Used for remote login.  
- Connect to a server: `ssh user@server`  
- Copy files: `scp file.txt user@server:/path/`  
- Enable passwordless SSH:  
  ```bash
  ssh-keygen -t rsa
  ssh-copy-id user@server
  ```  

---

## **Linux Troubleshooting & Scenarios**  

### **Monitoring CPU, RAM, Disk Usage**  
- **CPU:** `top`, `htop`, `mpstat`  
- **Memory:** `free -m`, `vmstat`  
- **Disk:** `df -h`, `iostat`, `du -sh /path/`  

### **High CPU Utilization Troubleshooting**  
1. Identify process:  
   ```bash
   top
   ps aux --sort=-%cpu | head
   ```  
2. Kill high-usage process:  
   ```bash
   kill -9 PID
   ```  
3. Optimize services or restart necessary ones.  

### **What is IO Wait?**  
`%iowait` shows how much time the CPU spends waiting for disk I/O operations to complete. High I/O wait means slow disk operations.  
- Check: `iostat -x`  

---

## **Booting Process in Linux**  
1. **BIOS/UEFI:** Hardware initialization.  
2. **Bootloader (GRUB):** Loads the OS kernel.  
3. **Kernel Initialization:** Loads drivers and mounts the filesystem.  
4. **Init/Systemd:** Starts services (`systemctl list-units --type=service`).  

---

## **YUM and Repo Files**  
- YUM (`yum install package`) manages RPM packages.  
- Repo files (`/etc/yum.repos.d/`) define package sources.  

Example repo file:  
```ini
[myrepo]
name=My Custom Repo
baseurl=http://myrepo.com/repo/
enabled=1
gpgcheck=0
```  

---

## **Networking Concepts**  

### **OSI Model Layers**  
1. **Physical** (Cables, NICs)  
2. **Data Link** (MAC, Switches)  
3. **Network** (IP, Routers)  
4. **Transport** (TCP, UDP)  
5. **Session** (Maintains connections)  
6. **Presentation** (Encryption, Compression)  
7. **Application** (HTTP, FTP)  

### **Types of IP Addresses**  
- **Public:** Accessible over the internet (e.g., 8.8.8.8).  
- **Private:** Used within local networks (e.g., 192.168.x.x, 10.x.x.x).  

### **What is NAT (Network Address Translation)?**  
NAT maps private IPs to a public IP for internet access.  
- **Static NAT:** One-to-one mapping.  
- **Dynamic NAT:** Many-to-one mapping.  

### **Difference Between Public & Private IP**  
- **Public:** Globally routable, assigned by ISPs.  
- **Private:** Used inside a network, not routable on the internet.  

### **Which is the Private IP?**  
- `192.168.0.1` → Private IP  
- `169.20.24.4` → Public IP  

### **What is a Network Adapter?**  
A network adapter (NIC) is hardware that connects a device to a network. It has a MAC address and supports different network types (Ethernet, Wi-Fi).  

---

## **AWS Concepts**  

### **Amazon S3 (Simple Storage Service)**  
- Object storage with high availability.  
- Used for backups, static websites, and logs.  
- Lifecycle policies for cost optimization.  

### **AMI (Amazon Machine Image)**  
- Pre-configured VM templates used to launch EC2 instances.  
- Contains OS, applications, and configurations.  

### **EC2 (Elastic Compute Cloud)**  
- Virtual servers in AWS.  
- Supports different instance types (e.g., t2.micro for small workloads).  

---

## **Shell Scripting**  

### **Shebang (`#!`)**  
Defines the script interpreter:  
```bash
#!/bin/bash
```  

### **Loops in Shell**  
**For Loop:**  
```bash
for i in {1..5}; do echo "Iteration $i"; done
```  
**While Loop:**  
```bash
while true; do echo "Running"; sleep 1; done
```  

### **Environment Variables in Scripts**  
```bash
#!/bin/bash
export MY_VAR="Hello World"
echo $MY_VAR
```  

### **Shell Script to Check Filesystem Full**  
```bash
#!/bin/bash
df -h | awk '$5 ~ /[8-9][0-9]%|100%/{print $0}'
```  

### **Shell Script to Backup Logs with Date**  
```bash
#!/bin/bash
backup_dir="/var/logs/backup"
mkdir -p $backup_dir
tar -czf "$backup_dir/logs_$(date +%F).tar.gz" /var/logs/*.log
```  

---


---

### **12. Booting Process in Linux**  
1. **BIOS/UEFI** → Power-on self-test (POST), hardware checks.  
2. **Bootloader (GRUB)** → Loads the Linux kernel.  
3. **Kernel Initialization** → Loads drivers, mounts filesystems.  
4. **Systemd (or init)** → Manages services and system startup.  
5. **Login Prompt** → User can log in.  

---

### **13. What is Systemd?**  
`systemd` is the modern Linux service manager that controls system startup, services, and dependencies.  
- Use `systemctl` to manage services:  
  ```bash
  systemctl status sshd
  systemctl start nginx
  systemctl enable httpd
  ```  

---

### **14. Which is the Last Script Run by Systemd?**  
`/etc/rc.local` (if enabled) or the last service in `/etc/systemd/system/`.  

To find the last executed service:  
```bash
systemctl list-units --type=service --no-pager
```

---

### **15. Difference Between RAM and ROM**  
| Feature | RAM | ROM |
|---------|------|------|
| Type | Volatile (temporary) | Non-volatile (permanent) |
| Usage | Stores running programs | Stores firmware (BIOS) |
| Speed | Faster | Slower |

---

### **16. What is POST?**  
Power-On Self-Test (POST) is a hardware diagnostic test performed during system startup to check memory, CPU, and storage.  

---

### **17. Sync Server Time 2 Hours Delayed**  
```bash
timedatectl set-time "$(date -d '2 hours ago' '+%Y-%m-%d %H:%M:%S')"
```

For automatic drift management:  
```bash
timedatectl set-timezone UTC
timedatectl set-ntp off
```

---

### **18. Difference Between NTP and Chrony**  
| Feature | NTP | Chrony |
|---------|------|--------|
| Usage | Traditional time sync daemon | Faster, modern replacement |
| Best For | Static systems | Dynamic (VMs, cloud) |
| Sync Speed | Slower | Faster |
| Config File | `/etc/ntp.conf` | `/etc/chrony.conf` |

---

### **19. What is rc.local?**  
- A legacy startup script executed at the end of the boot process.  
- Located at `/etc/rc.local` (may need execution permission).  

Example:  
```bash
chmod +x /etc/rc.local
```

---

### **20. Difference Between ext and XFS**  
| Feature | EXT4 | XFS |
|---------|------|------|
| Performance | General-purpose | High-performance, large files |
| Journaling | Yes | Yes (better performance) |
| Best for | Small to medium systems | Large-scale systems |

---

### **21. Run Level for Server Restart**  
- **Runlevel 6** is used to reboot the system.  

Commands:  
```bash
init 6
reboot
```

---

### **22. Explain NFS Server**  
NFS (Network File System) allows file sharing across Linux machines.  
- To check mounted shares:  
  ```bash
  showmount -e <server_ip>
  ```
- Mount NFS share:  
  ```bash
  mount -t nfs <server_ip>:/nfs_share /mnt
  ```

---

### **23. Difference Between TCP and UDP**  
| Feature | TCP | UDP |
|---------|------|------|
| Type | Connection-oriented | Connectionless |
| Reliability | Reliable | Unreliable |
| Use Case | Web, SSH | DNS, Streaming |

---

### **24. What is the `/proc` Filesystem?**  
A virtual filesystem that provides information about system processes and kernel parameters.  

Examples:  
```bash
cat /proc/cpuinfo
cat /proc/meminfo
ls /proc
```

---

### **25. Important Port Numbers**  
- **DNS:** 53  
- **DHCP:** 67, 68  
- **HTTP:** 80  
- **HTTPS:** 443  

---

### **26. Why DNS Uses UDP and Not TCP?**  
- Faster query resolution.  
- Lower overhead.  
- Uses TCP only for large zone transfers.  

---

### **27. Monitoring Tool: Nagios**  
- Open-source monitoring tool.  
- Monitors servers, network devices, and applications.  
- Uses plugins to check services.  

---

### **28. Troubleshooting Client Server Access Issues**  
1. **Check network connectivity:**  
   ```bash
   ping <server-ip>
   telnet <server-ip> <port>
   ```
2. **Check SSH service:**  
   ```bash
   systemctl status sshd
   ```
3. **Verify firewall rules:**  
   ```bash
   iptables -L
   ```

---

### **29. Explain Passwordless SSH**  
- Uses SSH keys instead of passwords.  

**Steps:**  
```bash
ssh-keygen -t rsa
ssh-copy-id user@remote-server
ssh user@remote-server
```

---

### **30. How Ansible Works (Agentless Communication)**  
- Uses SSH for Linux and WinRM for Windows.  
- No agent needed on managed hosts.  
- Executes playbooks remotely.  

Example Inventory:  
```ini
[webservers]
server1 ansible_host=192.168.1.10 ansible_user=root
```

Run command:  
```bash
ansible webservers -m ping
```

---

### **31. What is Ansible Vault?**  
A tool for encrypting sensitive data in Ansible.  

Encrypt a file:  
```bash
ansible-vault encrypt secrets.yml
```

Edit encrypted file:  
```bash
ansible-vault edit secrets.yml
```

---

### **32. Schedule a Cron Job to Run at 1 AM Daily**  
Edit crontab:  
```bash
crontab -e
```
Add:  
```bash
0 1 * * * /path/to/script.sh
```

---
