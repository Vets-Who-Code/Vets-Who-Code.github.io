# Command Line Utilities SOP

## 1. Introduction

This Standard Operating Procedure (SOP) outlines essential command line utilities that enhance productivity and efficiency in various tech-related tasks. These tools are crucial for data transfer, file compression, network management, package management, system monitoring, and user administration. Mastering these utilities will significantly improve your command line proficiency and overall system management capabilities.

## 2. Data Transfer

### 2.1 scp (Secure Copy Protocol)
Securely transfer files between systems using SSH.

```bash
scp -P 2222 user@remote:/path/to/file /local/directory
scp -r user@remote:/directory/ /local/directory
```

### 2.2 rsync (Remote Sync)
Efficiently synchronize files and directories between systems.

```bash
rsync -avz user@remote:/source /local/destination
rsync -av --exclude 'path/to/exclude' /source /destination
rsync --bwlimit=1000 /source /destination
```

### 2.3 wget (Web Get)
Retrieve files from web servers.

```bash
wget -b url
wget --retry-connrefused --waitretry=10 --timeout=60 url
```

### 2.4 ftp (File Transfer Protocol)
Basic file transfer for non-secure data movements.

```bash
ftp -p host
ftp -s:script.txt host
```

## 3. File Compression

### 3.1 zip
Package and compress files for easy sharing and storage.

```bash
zip archive.zip -r target_folder/ -x \*exclude_pattern\*
zip -u archive.zip updated_file.txt
```

### 3.2 tar
Bundle files and directories efficiently.

```bash
tar --listed-incremental=snapshot.file -cvzf backup.tar.gz target_directory/
tar czf - target_directory/ | ssh user@remote "cat > remote_backup.tar.gz"
```

### 3.3 gzip
Maximize compression ratios for faster transfers.

```bash
gzip -S .custom_suffix large_file
cat archive_part1.gz archive_part2.gz > combined_archive.gz
```

### 3.4 bzip2
Provide superior compression efficiency for large-scale archival.

```bash
bzip2 -dc archive.bz2 > output_file
pbzip2 -p4 massive_file
```

## 4. Network Tools

### 4.1 ping
Test connectivity and measure round-trip time to a host.

```bash
ping -c 4 google.com
```

### 4.2 ssh (Secure Shell)
Securely access and execute commands on remote machines.

```bash
ssh -p port user@hostname
ssh -i ~/.ssh/mykey user@192.168.1.100
```

## 5. Package Management

### 5.1 apt (Advanced Package Tool)
Manage software on Debian-based systems.

```bash
sudo apt update && sudo apt upgrade -y
echo "package_name hold" | sudo dpkg --set-selections
```

### 5.2 brew (Homebrew)
Package management for macOS.

```bash
brew tap custom/repo
brew services start service_name
```

### 5.3 yum (Yellowdog Updater, Modified)
Manage RPM packages on Red Hat-based systems.

```bash
yum history
yum --enablerepo=repo_name install package_name
```

### 5.4 dpkg (Debian Package)
Low-level package management for Debian-based systems.

```bash
dpkg-query -l
dpkg-reconfigure package_name
```

## 6. System Information Tools

### 6.1 uname
Display system information.
```bash
uname -a  # Display all information
uname -s  # Display kernel name
uname -r  # Display kernel release
```

### 6.2 lscpu
Display CPU architecture information.
```bash
lscpu
lscpu | grep Virtualization  # Check virtualization support
```

### 6.3 lshw
Display detailed hardware information.
```bash
sudo lshw -short  # Display brief hardware summary
sudo lshw -C network  # Display network hardware details
```

### 6.4 dmesg
Display kernel messages.
```bash
dmesg -Tw  # Display kernel messages in real-time with timestamps
```

## 7. System Monitoring

### 7.1 top
Monitor system processes and resource usage in real-time.
```bash
top -b -n 1 > top_output.txt  # Capture a snapshot of system state
top -p PID1,PID2  # Monitor specific processes
```

### 7.2 htop
Interactive process viewer and system monitor.
```bash
htop -u username  # Monitor processes for a specific user
htop --output-setup-json > setup.json  # Export htop configuration
```

### 7.3 iostat
Report CPU statistics and I/O statistics for devices and partitions.
```bash
iostat -d /dev/sda  # Display I/O statistics for a specific device
iostat -e  # Display extended statistics
```

### 7.4 vmstat
Report virtual memory statistics.
```bash
vmstat -d  # Display disk statistics
vmstat -e  # Display event counter information
```

## 8. User and Group Management

### 8.1 useradd
Create a new user account.
```bash
useradd -m -d /home/jdoe -e 2023-12-31 jdoe  # Create user with home directory and expiration date
```

### 8.2 groupadd
Create a new group.
```bash
groupadd -r -g 101 developers  # Create a system group with specific GID
```

### 8.3 sudo
Execute a command as another user, typically with administrative privileges.
```bash
sudo -u jdoe ls /home/jdoe  # Execute command as another user
```

### 8.4 passwd
Change user password and manage account locks.
```bash
passwd -l jdoe  # Lock user account
passwd -u jdoe  # Unlock user account
```

## 9. Best Practices

1. Always verify the integrity of transferred data.
2. Use compression to reduce bandwidth usage and storage requirements.
3. Regularly update package lists and installed software to ensure security.
4. Exercise caution when removing packages to avoid unintended consequences.
5. Use SSH keys for secure, passwordless authentication when accessing remote systems.
6. Implement rate limiting and throttling for large data transfers to avoid network congestion.
7. Choose the appropriate compression algorithm based on the specific requirements of speed, size, and computational resources.
8. Regularly review and update SSH keys to maintain security.
9. Regularly monitor system performance and resource usage to identify and address issues proactively.
10. Use system information tools to gather detailed hardware and software information for troubleshooting and inventory management.
11. Implement proper user and group management practices to enhance system security and organization.
12. Utilize sudo instead of switching to the root user for better security and accountability.
13. Regularly review and update user accounts, removing or disabling unnecessary accounts to maintain system security.

## 10. Summary

Mastering these command line utilities provides a comprehensive toolkit for efficient system management, data handling, network operations, system monitoring, and user management. From secure file transfers and compression to network diagnostics, package management, and system performance analysis, these tools form the backbone of effective command line operations. 

The ability to gather detailed system information, monitor performance in real-time, and manage users and groups effectively are crucial skills for system administrators and power users. By incorporating these utilities into your workflow, you can significantly enhance your ability to manage, troubleshoot, and optimize Linux systems.

Regular practice and application of these utilities will significantly enhance your proficiency in various tech-related roles, from system administration to software development and cybersecurity.