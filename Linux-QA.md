

---

### **2. What Are Processes? How to Check Zombie Processes?**  
A process is an executing instance of a program.  

- **Zombie Process** → A process that has completed execution but still has an entry in the process table because its parent has not read its exit status.  
- **Check Zombie Processes:**  
  ```bash
  ps aux | awk '{ if ($8 == "Z") print $0 }'
  ```

---

### **3. What is IO Wait? How to Troubleshoot High IO Wait?**  
- **IO Wait:** The percentage of time the CPU is waiting for disk or network IO operations to complete.  
- **Troubleshoot High IO Wait:**  
  - Check IO stats: `iostat -x 1 5`  
  - Identify heavy disk usage: `iotop`  
  - Check running processes: `ps aux --sort=-%io`  
  - Tune disk scheduler: `echo deadline > /sys/block/sda/queue/scheduler`  

---

### **4. Commands to Check IO Wait**  
```bash
top  # Check %wa (IO wait)
iostat -x 1 5
vmstat 1 5
iotop
```

---

### **5. Command to Change File Permissions**  
```bash
chmod 644 filename
```

---

### **6. Different File Systems**  
- **EXT4** → Standard Linux FS  
- **XFS** → High-performance FS  
- **Btrfs** → Supports snapshots  
- **NTFS** → Windows FS  
- **VFAT** → USB drives  

---

### **7. Create Directories a1, a2, a3 Under abc**  
```bash
mkdir -p abc/{a1,a2,a3}
```

---

### **8. Replace "apple" With "banana" in Vim**  
In Vim, press `Esc`, then:  
```vim
:%s/apple/banana/g
```

---

### **9. Command to Check Process ID (PID)**  
```bash
pgrep <process-name>
pidof <process-name>
ps aux | grep <process-name>
```

---

### **10. Check File System of All Mount Points**  
```bash
df -Th
```

---

### **11. See Processes of All Users on All Terminals**  
```bash
ps aux
```

---

### **12. What is init.d and systemd?**  
- `init.d` → Old service management system.  
- `systemd` → Modern service manager, faster boot.  

---

### **13. Change Shell of a User**  
```bash
chsh -s /bin/bash username
```

---

### **14. Make a User a Primary Member of a Group**  
```bash
usermod -g groupname username
```

---

### **15. What is an Inode Number?**  
- A unique number assigned to each file.  
- Contains metadata like owner, permissions, timestamps.  
- Check inode:  
  ```bash
  ls -i filename
  ```

---

### **16. Password-Based vs Passwordless SSH**  
- **Password-Based SSH:** Requires a password on each login.  
- **Passwordless SSH:** Uses SSH keys instead.  

Password-based SSH login:  
```bash
ssh user@remote-server
```

---

### **17. If We Share SSH Keys, Will the File Remain the Same?**  
Yes, the file will remain the same. SSH keys only authenticate the user, they don’t modify files.

---

### **18. What is Ansible? Port Used?**  
- Ansible is an automation tool that runs on **port 22 (SSH)**.  

---

### **19. What Are Magic Variables in Ansible?**  
Predefined variables like:  
- `ansible_play_hosts`
- `hostvars`
- `groups`  

Used for dynamic inventory management.

---

### **20. What Are Ansible Roles?**  
- Predefined structures to organize playbooks.  
- Located in `/etc/ansible/roles/`.  
- Example:  
  ```bash
  ansible-galaxy init myrole
  ```

---

### **21. What is an Ad-hoc Command?**  
A one-time Ansible command without a playbook.  
Example:  
```bash
ansible all -m ping
```

---

### **22. Change Password of Encrypted File in Ansible Vault**  
```bash
ansible-vault rekey secrets.yml
```

---

### **23. Difference Between `du` and `df`**  
| Command | Usage |
|---------|-------|
| `du` | Check directory size |
| `df` | Check disk usage |

---

### **25. What is a Container? What is Podman?**  
- A **container** is a lightweight, isolated environment for applications.  
- **Podman** is a container runtime, similar to Docker but rootless.  
- Check running containers:  
  ```bash
  podman ps
  ```

---

### **26. What is Kubernetes and OpenShift?**  
- **Kubernetes**: Container orchestration platform.  
- **OpenShift**: Enterprise Kubernetes with security and additional features.

---

### **27. What is DNS? How Does It Work?**  
- DNS translates domain names to IP addresses.  
- Works in a hierarchy:  
  **Root DNS → TLD → Authoritative DNS**  

---

### **28. Monitoring CPU, RAM, and Disk**  
Commands:  
```bash
top
vmstat
free -m
df -h
```

---

### **29. Nagios XI Monitoring & Port**  
- Nagios XI monitors servers, network devices, and applications.  
- Works on **port 5666 (NRPE)**.

---

### **30. What is SNMP, HTTP, and HTTPS?**  
- **SNMP**: Monitors network devices (port 161).  
- **HTTP**: Web protocol (port 80).  
- **HTTPS**: Secure HTTP (port 443).  

---

### **31. Why Was `init.d` Replaced by `systemd`?**  
- **Faster boot time**  
- **Parallel execution of services**  
- **Better dependency management**  

---

### **32. Ansible Programming Language & Python Dependencies**  
- Written in **Python**.  
- Dependencies:  
  - `pyyaml`
  - `jinja2`
  - `cryptography`

---

### **33. How Do You Practice Linux? How to Create VMs?**  
- Use **VirtualBox, VMware, or KVM**.  
- Create a VM in VirtualBox:  
  ```bash
  VBoxManage createvm --name "LinuxVM" --register
  ```

---

### **34. What is LVM? Create a Partition**  
- **LVM (Logical Volume Manager)** is used for flexible disk management.  
- Create LVM partition:  
  ```bash
  pvcreate /dev/sdb
  vgcreate myvg /dev/sdb
  lvcreate -L 10G -n mylv myvg
  mkfs.ext4 /dev/myvg/mylv
  ```

---

### **35. How to Use the `/data` Partition?**  
```bash
mount /dev/sdb1 /data
```

---

### **36. How to Change File System?**  
```bash
mkfs.ext4 /dev/sdb1
```

---

### **37. What is Logrotate? Rotate Logs for `httpd`**  
A utility for managing log files.  

Configure rotation for `httpd`:  
```bash
vi /etc/logrotate.d/httpd
```
Add:  
```
/var/log/httpd/*.log {
    weekly
    rotate 4
    compress
}
```

---

### **38. Troubleshooting Server Reboot Without SAR**  
Check logs:  
```bash
journalctl -b -1
cat /var/log/messages | grep -i reboot
```

---

### **39. Cron Job to Run Every 30 Seconds**  
Edit crontab:  
```bash
crontab -e
```
Add:  
```bash
* * * * * /path/to/script.sh
* * * * * sleep 30; /path/to/script.sh
```
---
Here are the answers to your new set of questions:  

---

### **Process**  
A process is an instance of a running program.  
Types of processes:  
- **Foreground** (`fg`) – Requires user input.  
- **Background** (`&`) – Runs in the background.  
- **Daemon** – System process that runs continuously.  
- **Zombie** – Terminated but not removed from the process table.  
- **Orphan** – Parent process has terminated.  

Check running processes:  
```bash
ps aux
top
htop
```

---

### **Archiving in Linux**  
- **Tar (Tape Archive)**  
  ```bash
  tar -cvf archive.tar /path/to/files
  ```
- **Zip**  
  ```bash
  zip -r archive.zip /path/to/files
  ```
- **Gzip**  
  ```bash
  tar -czvf archive.tar.gz /path/to/files
  ```

---

### **NFS (Network File System)**  
- Allows file sharing between Linux systems over a network.  
- Works on **port 2049**.  
- To mount an NFS share:  
  ```bash
  mount -t nfs server:/sharedfolder /mnt
  ```

---

### **Booting Process in Linux**  
1. **BIOS/UEFI** – Power-on self-test (POST).  
2. **Bootloader (GRUB)** – Loads the OS kernel.  
3. **Kernel Initialization** – Loads drivers.  
4. **Init/Systemd** – Manages services and targets.  
5. **Login Prompt**  

---

### **TCP vs. UDP**  
| Feature | TCP | UDP |
|---------|-----|-----|
| Connection-oriented | ✅ | ❌ |
| Reliable | ✅ | ❌ |
| Faster | ❌ | ✅ |
| Uses Acknowledgment | ✅ | ❌ |
| Common Use Cases | HTTP, SSH | DNS, Streaming |

---

### **Run Modes in Linux**  
Also called **Runlevels** in SysVinit or **Targets** in systemd:  
| Runlevel | Mode |
|----------|------|
| 0 | Halt (Shutdown) |
| 1 | Single-user mode (Rescue) |
| 3 | Multi-user mode (CLI) |
| 5 | Multi-user mode (GUI) |
| 6 | Reboot |

---

### **Reboot System Without `reboot` Command**  
```bash
shutdown -r now
init 6
systemctl reboot
echo b > /proc/sysrq-trigger  # Force reboot
```

---

### **DNS (Domain Name System)**  
- Converts domain names to IP addresses.  
- Works on **port 53** (TCP/UDP).  
- Check DNS resolution:  
  ```bash
  nslookup google.com
  dig google.com
  ```

---

### **Switch to Single User Mode**  
For **SysVinit** systems:  
1. At GRUB menu, press `e` to edit boot parameters.  
2. Add `single` or `init 1` at the end of the kernel line.  
3. Press `Ctrl+X` to boot.  

For **Systemd** systems:  
```bash
systemctl rescue
```

---

### **LVM (Logical Volume Manager)**  
1. **Create Physical Volume:**  
   ```bash
   pvcreate /dev/sdb
   ```
2. **Create Volume Group:**  
   ```bash
   vgcreate myvg /dev/sdb
   ```
3. **Create Logical Volume:**  
   ```bash
   lvcreate -L 10G -n mylv myvg
   ```
4. **Format and Mount:**  
   ```bash
   mkfs.ext4 /dev/myvg/mylv
   mount /dev/myvg/mylv /mnt
   ```

---

### **Find a Word in a File**  
```bash
grep "word" filename
```
Recursive search in all files:  
```bash
grep -r "word" /path/to/directory
```

---

### **File Permissions in Linux**  
| Symbol | Permission |
|--------|------------|
| r | Read |
| w | Write |
| x | Execute |

Check permissions:  
```bash
ls -l filename
```

Change permissions:  
```bash
chmod 755 filename
```

---

### **Umask (Default File Permissions)**  
Check current umask:  
```bash
umask
```
Set umask:  
```bash
umask 022
```

---

### **Common Port Numbers**  
| Service | Port |
|---------|------|
| SSH | 22 |
| FTP | 21 |
| Telnet | 23 |
| HTTPS | 443 |
| DNS | 53 |

---

### **Restore `/etc/passwd` If Deleted**  
```bash
cp /var/backups/passwd /etc/passwd
```
If backup is not available, restore using another system:  
```bash
scp root@backup-server:/etc/passwd /etc/passwd
```

---

### **Login If Root Password Is Changed**  
1. **Boot into Rescue Mode:**  
   - At GRUB, press `e`.  
   - Add `init=/bin/bash` at the end.  
   - Press `Ctrl+X`.  
2. **Change Password:**  
   ```bash
   mount -o remount,rw /
   passwd root
   ```

---

### **RHCSA Exam Questions**  
- User management (`useradd`, `passwd`)  
- File permissions (`chmod`, `chown`)  
- LVM (`pvcreate`, `vgcreate`, `lvcreate`)  
- Networking (`ip addr show`, `nmcli`)  
- Systemd services (`systemctl start/stop`)  
- SELinux (`getenforce`, `setenforce 0`)  

---

### **IoT (Internet of Things) Protocols**  
1. **MQTT (Message Queuing Telemetry Transport)** – Lightweight protocol for IoT.  
2. **CoAP (Constrained Application Protocol)** – Used in low-power devices.  
3. **AMQP (Advanced Message Queuing Protocol)** – Message-oriented middleware.  
4. **HTTP/HTTPS** – Standard web protocols used in IoT APIs.  

---

Here are the answers to your questions:

---

### **1. How Would You Add Users?**  
To add a new user in Linux:  
```bash
useradd username
passwd username
```
To add a user with a specific home directory:  
```bash
useradd -m -d /home/custom_user username
```
To create a user with a specific UID and shell:  
```bash
useradd -u 1002 -s /bin/bash username
```

---

### **2. How Would You Add a User to Another Primary Group of an Existing User?**  
By default, a user can have only one primary group. To change it:  
```bash
usermod -g new_primary_group username
```
To **add** the user to a secondary group without changing the primary group:  
```bash
usermod -aG secondary_group username
```
To check the groups of a user:  
```bash
id username
```

---

### **3. How Will You Schedule Jobs?**  

**Using Cron (Recurring Jobs):**  
Edit crontab:  
```bash
crontab -e
```
Example: Run a script daily at 1 AM:  
```bash
0 1 * * * /path/to/script.sh
```

**Using `at` (One-time Jobs):**  
```bash
echo "/path/to/script.sh" | at 2:30 PM
```

---

### **4. How Will You Replace a Word Within a File?**  

**Using Vim Editor:**  
- Open the file in Vim:  
  ```bash
  vim filename
  ```
- Run the following command in normal mode:  
  ```vim
  :%s/oldword/newword/g
  ```

**Without Vim (Using `sed`):**  
```bash
sed -i 's/oldword/newword/g' filename
```

---

### **5. How Are Pods Different from Containers?**  
| Feature | Pod | Container |
|---------|-----|----------|
| Definition | A pod is a group of one or more containers. | A container is an isolated runtime environment. |
| Scope | Smallest deployable unit in Kubernetes. | Runs an application process. |
| Networking | Shares the same network namespace and storage with other containers in the pod. | Has its own isolated network. |

---

### **6. What's the Use of Sticky Bit? How Will You Apply It?**  
The **sticky bit** prevents users from deleting files they don’t own in a shared directory.  
Example: Apply sticky bit on `/shared`:  
```bash
chmod +t /shared
```
Check if sticky bit is applied:  
```bash
ls -ld /shared
```
You’ll see **`t`** at the end of the permission string:  
```
drwxrwxrwt 2 root root 4096 Feb 13 10:30 /shared
```

---

### **7. Is NFS a Filesystem?**  
No, **NFS (Network File System)** is a protocol, not a filesystem. It allows remote systems to share files over a network.  
To mount an NFS share:  
```bash
mount -t nfs server:/shared_folder /mnt
```

---

### **8. What's Stored in `/proc`?**  
`/proc` is a **virtual filesystem** that provides information about running processes and system details.  
| Directory/File | Description |
|---------------|-------------|
| `/proc/cpuinfo` | CPU details |
| `/proc/meminfo` | Memory usage |
| `/proc/uptime` | System uptime |
| `/proc/[PID]` | Process-specific info |
| `/proc/mounts` | Mounted filesystems |

To check CPU details:  
```bash
cat /proc/cpuinfo
```
To check memory info:  
```bash
cat /proc/meminfo
```

---

