# Advanced Command Line Features SOP

## 1. Introduction

This Standard Operating Procedure (SOP) outlines advanced command line features and techniques that enhance productivity and efficiency for software engineers, writers, instructors, and other tech professionals. These advanced features include shell scripting basics, environmental variables, aliases and shortcuts, command history manipulation, job control, and task scheduling.

## 2. Shell Scripting Basics

Shell scripting allows for automation of complex tasks and repetitive operations.

### 2.1 Creating a Shell Script
1. Create a new file with a `.sh` extension.
2. Add the shebang line at the top: `#!/bin/bash`
3. Make the script executable: `chmod +x script.sh`

### 2.2 Syntax

#### Variables
```bash
variable_name=value
my_var="Hello, World"
```

#### Conditional Statements
```bash
if [ "$variable_one" -gt "$variable_two" ]; then
  echo "Variable one is greater"
elif [ "$variable_one" -eq "$variable_two" ]; then
  echo "Variables are equal"
else
  echo "Variable two is greater"
fi
```

#### Loops
For Loop:
```bash
for i in {1..10}; do
  echo $i
done
```

While Loop:
```bash
count=0
while [ $count -lt 10 ]; do
  echo $count
  ((count++))
done
```

### 2.3 Structure

#### Shebang
```bash
#!/bin/bash
```

#### Functions
```bash
my_function() {
  echo "Hello from my_function!"
}

# Call the function
my_function
```

### 2.4 Best Practices
- Use consistent indentation (2 or 4 spaces).
- Comment your code for clarity.
- Handle errors and check return codes.

### 2.5 Debugging
Basic debugging:
```bash
bash -x script.sh
```

Advanced debugging:
```bash
set -e  # Exit on first error
set -u  # Treat unset variables as errors
set +e  # Continue even if there is an error
```

## 3. Environmental Variables

Environmental variables store system-wide or user-specific configuration information.

### 3.1 Setting Variables
```bash
# Temporary (session only)
VARIABLE_NAME=value

# Permanent (add to ~/.bashrc or ~/.bash_profile)
echo 'export PERMANENT_VAR="I'm here to stay!"' >> ~/.bashrc
source ~/.bashrc
```

### 3.2 Retrieving Variables
```bash
echo $VARIABLE_NAME
env  # List all environment variables
```

### 3.3 Exporting Variables
```bash
export VARIABLE_NAME
# Or in one line
export VARIABLE_NAME=value
```

### 3.4 Unsetting Variables
```bash
unset VARIABLE_NAME
```

## 4. Aliases and Shortcuts

Aliases allow you to create custom shortcuts for frequently used commands.

### 4.1 Creating Aliases
```bash
# Temporary (session only)
alias myalias='my long command here'

# Permanent (add to ~/.bashrc or ~/.zshrc)
echo "alias persist='I will survive reboots!'" >> ~/.bashrc
source ~/.bashrc
```

### 4.2 Common Aliases
```bash
alias ll='ls -l'
alias la='ls -A'
alias ..='cd ..'
```

### 4.3 Functions
For more complex operations, use functions:
```bash
myfunc() {
  echo "Doing complex stuff!"
}
```

## 5. Command History

Command history allows you to recall and reuse previously executed commands.

### 5.1 Navigating History
- Use Up and Down arrow keys to navigate through history.
- `Ctrl+r`: Search backward through history.

### 5.2 Repeating Commands
```bash
!!  # Repeat the last command
!n  # Repeat the nth command in history
!-n  # Repeat the nth last command
```

### 5.3 Modifying History
```bash
history -c  # Clear current session's history
history -d n  # Delete the nth command from history
history -a  # Manually save session's history
```

## 6. Job Control

Job control allows management of multiple processes within a single terminal session.

### 6.1 Background and Foreground Jobs
```bash
command &  # Start a job in the background
Ctrl+Z  # Pause the current foreground job
fg %n  # Bring job n to the foreground
bg %n  # Continue job n in the background
```

### 6.2 Listing and Managing Jobs
```bash
jobs  # List all jobs
kill %n  # Terminate job n
```

### 6.3 Signals
```bash
kill -l  # List all available signals
kill -SIGSTOP %n  # Pause job n
kill -SIGCONT %n  # Resume job n
kill -SIGKILL %n  # Forcefully terminate job n
```

## 7. Scheduling Tasks

The `cron` utility allows scheduling of recurring tasks.

### 7.1 Editing the Crontab
```bash
crontab -e
```

### 7.2 Crontab Syntax
```
* * * * * command_to_execute
│ │ │ │ │
│ │ │ │ └─── day of week (0 - 7) (Sunday = 0 or 7)
│ │ │ └────── month (1 - 12)
│ │ └─────────── day of month (1 - 31)
│ └──────────────── hour (0 - 23)
└───────────────────── minute (0 - 59)
```

### 7.3 Example Cron Job
```
0 2 * * * /path/to/backup_script.sh
```
This runs the backup script every day at 2:00 AM.

## 8. Best Practices

1. Use meaningful names for variables, aliases, and functions.
2. Comment your scripts and complex commands for better readability.
3. Be cautious when modifying system-wide environmental variables.
4. Regularly review and clean up your command history and cron jobs.
5. Use job control judiciously to manage system resources effectively.
6. Test scripts and scheduled tasks thoroughly before implementation.
7. Keep your shell configuration files (e.g., .bashrc, .zshrc) organized and well-commented.
8. Use error handling in your scripts to make them more robust.
9. When working with environmental variables, consider using lowercase for local variables and uppercase for exported variables to maintain clarity.
10. Utilize shell script debugging tools like `set -x` for troubleshooting.

## 9. Summary

Mastering these advanced command line features significantly enhances your ability to work efficiently in a Unix-like environment. From automating tasks with shell scripts to managing complex job workflows, these tools provide powerful capabilities for system administration, software development, and general productivity. 

Environmental variables offer a flexible way to configure your system and applications, while shell scripting allows you to automate complex tasks and create powerful utilities. Aliases and shortcuts streamline your workflow, and command history manipulation helps you work more efficiently. Job control gives you fine-grained management of processes, and task scheduling with cron allows for automation of recurring tasks.

Regular practice and exploration of these features will continue to improve your command line proficiency. Remember to always consider security implications when working with sensitive data in scripts or environmental variables, and to document your work for future reference and collaboration.