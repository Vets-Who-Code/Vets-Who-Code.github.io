# Comprehensive Bash Guide: From Basics to Advanced Techniques

## Table of Contents
- [Comprehensive Bash Guide: From Basics to Advanced Techniques](#comprehensive-bash-guide-from-basics-to-advanced-techniques)
  - [Table of Contents](#table-of-contents)
  - [1. Introduction to Bash](#1-introduction-to-bash)
  - [2. Basic Syntax](#2-basic-syntax)
    - [Comments](#comments)
    - [Commands](#commands)
  - [3. Variables](#3-variables)
    - [Declaring and Using Variables](#declaring-and-using-variables)
    - [Command Substitution](#command-substitution)
  - [4. Control Structures](#4-control-structures)
    - [If-Else Statement](#if-else-statement)
    - [Loops](#loops)
      - [For Loop](#for-loop)
      - [While Loop](#while-loop)
  - [5. Functions](#5-functions)
  - [6. Input and Output](#6-input-and-output)
    - [Reading User Input](#reading-user-input)
    - [Redirecting Output](#redirecting-output)
  - [7. Command Line Arguments](#7-command-line-arguments)
  - [8. Basic String Operations](#8-basic-string-operations)
  - [9. Basic Arithmetic](#9-basic-arithmetic)
  - [10. Useful Bash Commands](#10-useful-bash-commands)
  - [11. Intermediate Bash Techniques](#11-intermediate-bash-techniques)
    - [Arrays](#arrays)
    - [Case Statements](#case-statements)
    - [Here Documents](#here-documents)
    - [Subshells](#subshells)
  - [12. Advanced Bash Techniques](#12-advanced-bash-techniques)
    - [Regular Expressions](#regular-expressions)
    - [Process Substitution](#process-substitution)
    - [Traps](#traps)
    - [Debugging](#debugging)
    - [Parameter Expansion](#parameter-expansion)
  - [13. Best Practices and Tips](#13-best-practices-and-tips)

## 1. Introduction to Bash

Bash (Bourne Again SHell) is a command processor that typically runs in a text window where the user types commands that cause actions. Bash can also read and execute commands from a file, called a script.

## 2. Basic Syntax

### Comments
```bash
# This is a comment
```

### Commands
```bash
echo "Hello, World!"  # Prints "Hello, World!"
```

## 3. Variables

### Declaring and Using Variables
```bash
name="John"
echo "Hello, $name"  # Prints "Hello, John"
```

### Command Substitution
```bash
current_date=$(date)
echo "Today is $current_date"
```

## 4. Control Structures

### If-Else Statement
```bash
if [ "$name" == "John" ]; then
    echo "Hello, John!"
else
    echo "You're not John"
fi
```

### Loops

#### For Loop
```bash
for i in 1 2 3 4 5
do
   echo "Number: $i"
done
```

#### While Loop
```bash
count=0
while [ $count -lt 5 ]
do
   echo "Count: $count"
   ((count++))
done
```

## 5. Functions

```bash
greet() {
    echo "Hello, $1!"
}

greet "World"  # Calls the function, prints "Hello, World!"
```

## 6. Input and Output

### Reading User Input
```bash
echo "What's your name?"
read user_name
echo "Hello, $user_name!"
```

### Redirecting Output
```bash
echo "This goes to a file" > output.txt
echo "This is appended to the file" >> output.txt
```

## 7. Command Line Arguments

```bash
# Save as script.sh and run with: ./script.sh arg1 arg2
echo "First argument: $1"
echo "Second argument: $2"
echo "All arguments: $@"
echo "Number of arguments: $#"
```

## 8. Basic String Operations

```bash
string="Hello, World!"
echo "${string:0:5}"  # Prints "Hello"
echo "${string/World/Bash}"  # Prints "Hello, Bash!"
echo "${#string}"  # Prints the length of the string
```

## 9. Basic Arithmetic

```bash
a=5
b=3
echo $((a + b))  # Addition
echo $((a - b))  # Subtraction
echo $((a * b))  # Multiplication
echo $((a / b))  # Division
echo $((a % b))  # Modulus
```

## 10. Useful Bash Commands

- `source script.sh`: Execute commands from a file in the current shell
- `export VAR="value"`: Make a variable available to child processes
- `alias ls='ls -la'`: Create a shortcut for a command
- `set -e`: Exit immediately if a command exits with a non-zero status
- `set -x`: Print commands and their arguments as they are executed

## 11. Intermediate Bash Techniques

### Arrays
```bash
fruits=("apple" "banana" "cherry")
echo ${fruits[1]}  # Prints "banana"
echo ${fruits[@]}  # Prints all elements
echo ${#fruits[@]}  # Prints number of elements
```

### Case Statements
```bash
case $fruit in
    "apple")
        echo "It's an apple"
        ;;
    "banana"|"plantain")
        echo "It's a banana or plantain"
        ;;
    *)
        echo "Unknown fruit"
        ;;
esac
```

### Here Documents
```bash
cat << EOF > file.txt
This is a multi-line
text that will be written
to file.txt
EOF
```

### Subshells
```bash
(cd /tmp && echo "Current dir: $PWD")
echo "We're back in $PWD"
```

## 12. Advanced Bash Techniques

### Regular Expressions
```bash
if [[ "example@email.com" =~ ^[A-Za-z0-9._%+-]+@[A-Za-z0-9.-]+\.[A-Z|a-z]{2,}$ ]]; then
    echo "Valid email"
fi
```

### Process Substitution
```bash
diff <(ls dir1) <(ls dir2)
```

### Traps
```bash
trap 'echo "Ctrl+C is trapped"' INT
```

### Debugging
```bash
set -x  # Enable debugging
set -e  # Exit on error
set -u  # Treat unset variables as an error
```

### Parameter Expansion
```bash
name="John"
echo ${name:-"Default"}  # Use default if name is unset
echo ${name:="NewDefault"}  # Assign default if name is unset
echo ${name:+"Alternative"}  # Use alternative if name is set
echo ${name:?"Error message"}  # Display error if name is unset
```

## 13. Best Practices and Tips

1. Use shellcheck for script validation
2. Use functions for repeated code
3. Use meaningful variable names
4. Quote variables to prevent word splitting
5. Use `set -e` to exit on error
6. Use `set -u` to catch unset variables
7. Use `[[ ]]` for conditionals instead of `[ ]`
8. Use `$(command)` instead of backticks for command substitution
9. Use `$()` for arithmetic instead of `expr`
10. Use `trap` to handle signals and perform cleanup

Remember, this guide covers a wide range of Bash features, but Bash is incredibly rich and there's always more to learn. Always refer to the Bash manual (`man bash`) for the most up-to-date and detailed information.