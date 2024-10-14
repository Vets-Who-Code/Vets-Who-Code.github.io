# Command Line Basics SOP

## 1. Introduction

This Standard Operating Procedure (SOP) outlines the fundamental commands and techniques for effective command line usage. Mastering these basics is crucial for efficient system navigation, file management, and maintaining operational security in a digital environment.

## 2. Navigating Directories

### 2.1 Essential Navigation Commands

#### 2.1.1 pwd (Print Working Directory)
Confirms your current location in the file system.
```bash
pwd
```

#### 2.1.2 cd (Change Directory)
Primary command for moving between directories.
- Move to home directory: `cd` or `cd ~`
- Move to previous directory: `cd -`
- Move up one level: `cd ..`
- Move to specific directory: `cd /path/to/directory`

#### 2.1.3 ls (List)
Provides reconnaissance on directory contents.
- Basic listing: `ls`
- Detailed listing: `ls -l`
- Show hidden files: `ls -a`
- Detailed listing with hidden files: `ls -la`
- Sort by size: `ls -S`

#### 2.1.4 tree
Offers a hierarchical view of the directory structure.
- Limited depth view: `tree -L 2`
- Directory-only view: `tree -d`

### 2.2 Advanced Navigation Techniques

- Use tab completion to auto-complete file and directory names.
- Utilize the up and down arrow keys to navigate command history.
- Create aliases for frequently used commands to enhance efficiency.

## 3. File Operations

### 3.1 Basic File Management

#### 3.1.1 cp (Copy)
Copies files or directories.
```bash
cp source_file target_file
cp -R source_directory target_directory
```

#### 3.1.2 mv (Move)
Moves or renames files and directories.
```bash
mv old_filename new_filename
mv file_to_move target_directory/
```

#### 3.1.3 rm (Remove)
Deletes files or directories.
```bash
rm filename
rm -r directory_name
rm -i filename  # Interactive mode, asks for confirmation
```

#### 3.1.4 touch
Creates new empty files or updates file timestamps.
```bash
touch new_file
```

#### 3.1.5 mkdir (Make Directory)
Creates new directories.
```bash
mkdir new_directory
mkdir -p parent_directory/new_directory
```

### 3.2 Advanced File Operations

#### 3.2.1 find
Searches for files and directories based on various criteria.
```bash
find /path/to/search -name "filename"
find ~ -name "*.txt"  # Find all .txt files in home directory
```

#### 3.2.2 locate
Quickly searches for files using a pre-built database.
```bash
locate filename
sudo updatedb  # Update the database
```

## 4. Working with Wildcards

Wildcards enhance precision and efficiency in file operations.

### 4.1 Asterisk (*)
Matches zero or more characters.
```bash
ls *.jpg  # Lists all JPEG files
```

### 4.2 Question Mark (?)
Matches exactly one character.
```bash
ls report_?.txt  # Matches report_1.txt, report_2.txt, but not report_10.txt
```

### 4.3 Square Brackets ([])
Matches any single character within the brackets.
```bash
ls [a-c]*.txt  # Lists files starting with a, b, or c and ending with .txt
```

### 4.4 Curly Braces ({})
Specifies multiple discrete patterns.
```bash
ls {*.txt,*.pdf}  # Lists all text and PDF files
```

## 5. Managing Permissions

Proper permission management is crucial for system security.

### 5.1 chmod (Change Mode)
Adjusts file and directory permissions.
```bash
chmod u+x file.txt  # Adds execute permission for the owner
chmod 755 script.sh  # Sets rwxr-xr-x permissions
```

### 5.2 chown (Change Owner)
Transfers file or directory ownership.
```bash
chown user:group file.txt
```

### 5.3 chgrp (Change Group)
Modifies the group ownership of files or directories.
```bash
chgrp team project/
```

### 5.4 umask
Sets default creation permissions for new files and directories.
```bash
umask 022  # Sets default permissions to 644 for files and 755 for directories
```

## 6. Disk and Directory Usage

### 6.1 df (Disk Free)
Displays disk space usage.
```bash
df -h  # Human-readable format
```

### 6.2 du (Disk Usage)
Estimates space used by directories and files.
```bash
du -sh /path/to/directory
```

## 7. Summary

Mastering these command line basics provides a solid foundation for efficient system management and navigation. Regular practice and application of these commands will enhance your operational capabilities in a digital environment. Remember, the command line is your primary interface for precise control and management of your system resources.