# Command Line Cheat Sheet

## Navigation
```bash
pwd                 # Print working directory
ls                  # List directory contents
ls -la              # List detailed contents, including hidden files
cd <directory>      # Change directory
cd ..               # Move up one directory
cd ~                # Go to home directory
```

## File Operations
```bash
touch <file>        # Create a new file
mkdir <directory>   # Create a new directory
cp <source> <dest>  # Copy file or directory
mv <source> <dest>  # Move or rename file or directory
rm <file>           # Remove a file
rm -r <directory>   # Remove a directory and its contents
```

## File Viewing and Editing
```bash
cat <file>          # Display file contents
less <file>         # View file contents with pagination
head <file>         # Show first 10 lines of file
tail <file>         # Show last 10 lines of file
nano <file>         # Open file in nano text editor
vim <file>          # Open file in vim text editor
```

## File Permissions
```bash
chmod +x <file>     # Make file executable
chmod 755 <file>    # Set read, write, execute permissions
chown <user> <file> # Change file owner
```

## System Information
```bash
uname -a            # Display system information
df -h               # Show disk usage
free -h             # Display free and used memory
top                 # Show running processes (interactive)
ps aux              # List all running processes
```

## Network
```bash
ping <host>         # Ping a host
ssh <user>@<host>   # SSH into a remote machine
scp <file> <user>@<host>:<path> # Secure copy file to remote host
wget <url>          # Download file from web
curl <url>          # Send HTTP request
```

## Package Management (Debian/Ubuntu)
```bash
apt update          # Update package list
apt upgrade         # Upgrade installed packages
apt install <pkg>   # Install a package
apt remove <pkg>    # Remove a package
```

## Text Processing
```bash
grep <pattern> <file>    # Search for pattern in file
sed 's/old/new/g' <file> # Replace text in file
awk '{print $1}' <file>  # Print first column of file
```

## Process Management
```bash
<command> &         # Run command in background
jobs                # List background jobs
fg                  # Bring most recent job to foreground
kill <pid>          # Terminate process by ID
```

## Shortcuts
```bash
Ctrl + C            # Interrupt current process
Ctrl + Z            # Suspend current process
Ctrl + D            # Exit current shell
Ctrl + L            # Clear screen
Ctrl + R            # Search command history
```

## Miscellaneous
```bash
man <command>       # Display manual for command
history             # Show command history
echo $PATH          # Display PATH environment variable
which <command>     # Show full path of command
```

Remember, these commands may vary slightly depending on your specific operating system and shell. Always refer to the man pages (`man <command>`) for detailed information about each command.