1. How to Check Disk Space Usage
Answer: 
Use `df -h` to view disk usage by mounted filesystems. For directory-level details, run `du -sh /path/to/directory`. Clean up old logs, unused packages, or large files with tools like `ncdu` for interactive analysis.

2. Service Fails to Start
Answer: 
Check status with `systemctl status service-name`. View logs via `journalctl -u service-name --since "10 minutes ago"`. Ensure dependencies are installed and configuration files (e.g., `/etc/service-name/config.conf`) are correct.

3. Network Connectivity Issues
Answer:
- Test connectivity: `ping 8.8.8.8` (Google DNS). 
- Check routes: `ip route` or `traceroute google.com`. 
- Verify DNS: `dig google.com` or `nslookup google.com`. 
- Inspect interfaces: `ip a` or `ifconfig`. Restart networking with `systemctl restart NetworkManager` (or `networkd`).

4. "Permission Denied" Errors
Answer: 
- Check permissions: `ls -l /path/to/file`. 
- Modify permissions: `chmod 755 file` or `chown user:group file`. 
- If SELinux/AppArmor blocks access, check logs (`/var/log/audit/audit.log`) or temporarily disable with `setenforce 0`.

5. Terminating Unresponsive Processes
Answer: 
- Find PID: `ps aux | grep process-name` or `top`. 
- Kill process: `kill -9 PID` or `pkill process-name`. 
- Force-kill all instances: `killall -9 process-name`.

6. System Fails to Boot
Answer:
- Boot into recovery mode (GRUB menu) and check logs (`/var/log/boot.log` or `journalctl -b`). 
- Repair filesystems: `fsck /dev/sdX`. 
- Reinstall bootloader (GRUB): `grub-install /dev/sdX`.

7. "No Space Left on Device" Despite Free Space
Answer: 
- Check inode usage: `df -i`. 
- Delete small, numerous files (e.g., temporary files) to free inodes.

8. DNS Resolution Failures
Answer: 
- Verify DNS config: `cat /etc/resolv.conf`. 
- Test DNS server: `dig @8.8.8.8 google.com`. 
- Restart DNS resolver: `systemctl restart systemd-resolved`.

9. High CPU/Memory Usage
Answer: 
- Identify resource hogs: `top`, `htop`, or `vmstat 2`. 
- Kill problematic processes or optimize applications. Check for memory leaks with `free -h`.

10. SSH Connection Refused
Answer:
- Ensure SSH service runs: `systemctl status sshd`. 
- Check firewall rules: `ufw status` or `iptables -L`. 
- Verify SSH port (default: 22) is open and accessible.

11. Filesystem Corruption
Answer:
- Unmount the disk: `umount /dev/sdX`. 
- Run `fsck /dev/sdX` to repair. Backup critical data first. For root FS, boot from a live USB.

12. Cron Jobs Not Executing
Answer: 
- Check cron logs: `grep CRON /var/log/syslog`. 
- Ensure the cron service is running: `systemctl status cron`. 
- Validate syntax and user permissions in `/etc/crontab` or user crontabs (`crontab -e`).

13. Time Synchronization Issues
Answer: 
- Check NTP status: `timedatectl` or `ntpq -p`. 
- Sync manually: `ntpdate pool.ntp.org` or restart `chronyd`/`systemd-timesyncd`.
