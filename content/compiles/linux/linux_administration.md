---
title: Linux Administration - Overview
draft: false
date: 2025-12-06
tags: [linux]
---
# Module 01 \- Files and Directories

### Working with Directories

- **pwd** (Print Working Directory)
  - **Syntax:** *pwd*
  - **Example Output:** */home/jetlee*
- **cd** (Change Directory)
  - **Syntax:** *cd \&lt;absolute / relative path\&gt;*
  - **Usage Examples:**
    - *cd /etc* (Absolute path)
    - If *pwd* is */home*, then *cd jetlee* (Relative path)
  - **Notes:**
    - Used alone (*$cd*), navigates to the current user\&apos;s home directory.
    - *$cd \~*: Navigates to the user\&apos;s home directory (Example: */home/jetlee*).
    - *$cd ..*: Navigates to the parent directory.
    - **Absolute Path:** Starts with */* (root directory, the top of the file hierarchy).
    - **Relative Path:** Starts without a */* (starting point is the current directory).
- **ls** (List contents of a directory)
  - **Syntax:** *ls \&lt;path\&gt;*
  - **Usage:**
    - If no path is given, lists files and folders of the current directory.
    - *ls /etc*
  - **Options (Arguments):**
    - *\-l*: Long list; displays additional information like permissions, owners, dates.
    - *\-h*: Human-readable form, especially for content size.
    - *\-a*: Lists all contents, including hidden files and directories (those starting with a dot *.*).
- **mkdir** (Create a directory)
  - **Syntax:** *mkdir directory\_name*
  - **Requirement:** Requires at least one argument.
  - **Option:** *\-p*: Used to create parent directories.
    - **Example:** *mkdir \-p /parent1/parent2/directory*
- **rmdir** (Remove directories)
  - **Syntax:** *rmdir directory\_name*
  - **Action:** Removes the specified directory.

### Working with Files

- **file** (Know the type of files)
  - **Syntax:** *file \&lt;filepath\&gt;*
  - **Notes:** Linux does not use extensions like *\*.txt* or *\*.pdf* to communicate file type.
  - **Example:** *file example.txt*
  - **Option:** *\-s \&lt;filename\&gt;*: Used to know about special files, typically inside */proc* or */dev*.
    - **Example:** *file \-s /dev/sda1*, *file \-s /proc/cpuinfo*
- **touch** (Create empty files)
  - **Syntax:** *touch filename1 \&lt;filename2,...\&gt;*
  - **Usage:** Can specify the entire path and create multiple files at once.
    - **Examples:** *touch /home/jetlee/file1*, *touch file1 file2 file3*
  - **Option:** *\-t \&lt;properties\&gt; filename*: Used to set properties to the file during creation.
    - **Example:** *touch \-t 20220420000 test.txt*
- **rm** (Remove files or directories)
  - **Syntax:** *rm \&lt;path to file | directory\&gt;*
  - **Notes:** Removal is permanent; command line does not have trash/recovery systems.
  - **Options:**
    - *\-i*: Interactive (i), provides a prompt asking for confirmation before deletion.
    - *\-rf \&lt;path to directory\&gt;*: Used to delete non-empty directories. *r* stands for recursive, and *f* stands for force.
    - **Examples:** *rm \-rf testDirectory*, *rm testFile.txt*
- **cp** (Copy files)
  - **Syntax 1 (Single file):** *cp source target*
    - Copies the source to target destination. Both can be absolute paths. Target can be a file or directory.
  - **Syntax 2 (Multiple files):** *cp source1 \&lt;source2,..\&gt; target*
    - Target must be a directory.
  - **Options:**
    - *\-r directory target*: Copies files or subdirectories recursively.
    - *\-i source target*: Provides a prompt asking for permission if overwriting files.
- **mv** (Move and rename files)
  - **Syntax (Move):** *mv source target*
  - **Syntax (Rename):** *mv oldname newname*
  - **Options:**
    - *\-i*: Interactive (like *rm* and *cp*).
    - *\-r*: Move recursively.
  - **Example (Rename):**

```
$ls
file1.txt
$mv file1.txt backup.txt
$ls
backup.txt
```

### File Structure (Filesystem Hierarchy Standard \- FHS)

Linux partially follows the Filesystem Hierarchy Standard (FHS). Everything in the system is a file, arranged hierarchically, starting at the root directory.

- **/ \- Root Directory**
  - The starting point for the file system. Everything in Linux resides under root.
- **Binary Directories (Executable Programs)**
  - **/bin:** Contains software available for single-user mode and for all users\&apos; day-to-day activities (e.g., *cat*, *grep*, *cp*, *mv*).
  - **/sbin:** Contains system binaries essential for configuring the operating system. Majority require root privilege to run (e.g., *ipconfig*, *mkfs.ext4*, *fdisk*).
  - **/lib:** Contains shared libraries used by programs in */bin* and */sbin* (e.g., *libconsole.so.0.0.0*, *libcfont.so.0*).
  - **/opt:** Contains optional software outside the distribution repository; may be empty. A large application can store its files inside */opt/$packagename/* with subdirectories like */bin*, */etc*, */lib*, */man* (e.g., */opt/test/etc*).
- **Configuration Directories**
  - **/etc:** Contains machine-specific configuration files. Historically \&apos;etcetera,\&apos; now stands for \&apos;Editable Text Configuration.\&apos; Files usually have the package name with a *.conf* extension (e.g., *host.conf*, *adduser.conf*).
  - **Important Subdirectories:** */etc/init.d/*, */etc/X11/*, */etc/sysconfig/*
- **Data Directories**
  - **/home:** Directory for normal user data (personal/professional). Each user has a subdirectory (*/home/$Username/*). User profiles are stored via hidden files (e.g., *.bashrc*). *cd* or *cd\~* redirects a normal user here.
  - **/root:** The home directory for the **root** user, containing their data and profile as hidden files.
  - **/srv:** Contains data that is served by your system (e.g., *ftp*, *www*, *cvs*, *rsync* data). FHS approves administrative naming (e.g., */srv/sales/www*).
  - **/media:** Used as a mount point for external removable media devices (e.g., CD ROMs, digital cameras, USB-attached devices).
    - **Content Examples:** *usbdisk*, *cdrom0*
  - **/mnt:** Acts as a temporary mount point for local or remote file systems. FHS suggests it should be kept empty, but Linux/Unix systems may have subdirectories (e.g., */mnt/sda0*).
  - **/tmp:** Contains temporary files stored by users and programs. Data uses either local storage or RAM, managed by the OS.
- **In-Memory Directories**
  - **/dev:** Populated with files as the kernel recognizes hardware.
    - **Examples:** SATA device files (*/dev/sda0*) or IDE hard drives (*/dev/hda0*).
    - *dev/null*: Known as a storage blackhole with unlimited storage; nothing can be retrieved from it.
  - **/proc:** Special directory providing vital system information, often with 0 kB file sizes. Contains files about the kernel and what it manages. It can be used to directly converse with the kernel. Most files are read-only, though some in */proc/sys* are writable.
    - **Examples:** */proc/cpuinfo*, */proc/interrupts*
- **/usr** (Unix System Resources)
  - Contains sharable, read-only files.
  - **Subdirectories:**
    - *usr/bin/*: Binary files (in some systems, */bin* is a symbolic link here).
    - *usr/src/*: Ideal location for kernel source files.
    - *usr/local/*: Where admins can locally install software.
    - *usr/share/*: Files that are architecture independent.
    - *usr/include/*: Contains required C header files.
    - *usr/lib/*: Library files not directly executed by user or scripts.
- **/var** (Variable Data)
  - Contains variable data like logs, cache, and spool (simultaneous peripheral online) files.
  - **Important Subdirectories:**
    - *var/log/*: Central point for log files.
    - *var/cache/*: Main storage directory for cache data.
    - *var/spool/*: Contains spool directories for mail, cron, and other spool files.

# Module 02 \- Process

### Process Fundamentals

- Processes are programs in execution, or instances of programs.
- Linux is a multiprocessing OS, supporting simultaneous execution of several processes.
- The OS allocates required resources to create a suitable environment for a process.
- A process is created when a command is run in the shell (e.g., running *mdir* executes the corresponding program in */bin*).
- Each process has a unique identifier called PID (Process ID), which is a 5-digit number.
- No two processes can exist with the same PID simultaneously. If all combinations are used, the next batch of PIDs rolls up.

### Process Types

1. **Foreground Processes**
  1. The default mode; engages the user interface.
  2. Receives input from the keyboard and displays output on the monitor (e.g., running *pwd*).
  3. **Disadvantage:** If a process takes a long time, the user must wait for its completion to start another process, as the user interface is engaged.
2. **Background Processes**
  1. Run without keyboard input; usually manage processes with longer execution times.
  2. Will wait if user input is required.
  3. Enables parallel execution of several processes.
  4. A foreground process can be converted to background by appending *\&amp;* at the end of the command (e.g., *pwd \&amp;*).

### Process Commands

- **ps command** (Process Status)
  - Outputs currently running processes.
  - Output columns typically include process ID, user, uptime, and command.
  - **Option:** *ps \-x* outputs the processes started by the current user.

# Module 03 \- Getting System Information

### Getting System Information Commands

- **uname command**
  - Displays detailed information about the system.
  - **Syntax:** *uname \[option\]*
  - **Options:**
    - *\-a*: All information.
    - *\-r*: Kernel release information.
    - *\-s*: Kernel name information.
    - *\-n*: Network node hostname information.
- **hostname command**
  - Used to obtain the Domain Name System name and to set the host name and domain name of the system.
  - Hostname uniquely distinguishes a device connected over a network.
  - Usually specified in startup script files located in */etc/rc* and */etc/rc.local*. Editing the command in these files changes the hostname after the next reboot.
  - **Syntax:** *hostname \[-options\] \[file\]*
  - **Options:**
    - *\-a*: Displays aliases of hostname, if available (returns empty line otherwise).
    - *\-s*: Retrieves the short hostname (the portion before the dot).
    - *\-i*: Retrieves the IP address assigned against the hostname.
  - **Example:** *hostname \-s tester.test.com*
- **who command**
  - Used to get the following information:
    - Last time of boot.
    - Current run level of the system.
    - List of logged-in users.
  - **Syntax:** *who \[-option\] \[file\]*

# Module 04 \- Shell

### Types of Shell

1. **C Shell (***csh*)
  1. **Founder:** Bill Joy, University of California, Berkely.
  2. **Path:** */bin/csh*
  3. **Features:** Interactive features like alias, built-in arithmetic, command history, and C-like syntax.
  4. **User Prompt:** *username%* (Normal user), *username\#* (Root user).
2. **Bourne Shell (***sh*)
  1. **Founder:** Stevie Bourne, AT\&amp;T Bell Labs.
  2. **Path:** */bin/sh*
  3. **Features:** Fast, original UNIX shell, default for many Unix-based OS.
  4. **Limitations:** Does not have interactive features like command history or arithmetic capacity.
  5. **User Prompt:** *username$* (Normal user), *username\#* (Root user).
3. **Korn Shell (***ksh*)
  1. **Founder:** David Korn, AT\&amp;T Bell Labs.
  2. **Path:** */bin/ksh*
  3. **Features:** Super set of Bourne Shell, compatible with C shell scripts, faster than C shell. Includes Bourne Shell features and interactive features like arithmetic, C arrays, functions, and string manipulation.
  4. **User Prompt:** *username$* (Normal user), *username\#* (Root user).
4. **Bourne Again Shell (***bash*)
  1. **Path:** */bin/bash*
  2. **Features:** Includes features of both Bourne shell and Korn shells.
  3. **User Prompt:** *username-g.gg$* (Normal user), *username-g.gg\#* (Root user).
    1. *g.gg* corresponds to the shell version (e.g., *host-3.1.0$*).

### Alias and Unalias

- **alias**
  - Creates aliases (replaces a command with a familiar name).
  - Used to easily supply default parameters to commands.
  - **Syntax:** *alias aliasname=\&apos;some existing command name\&apos;*
  - **Examples:**

```
# create alias
$cat count.txt
one
two
$alias say=cat
$say count.txt
one
two

# create abbreviations
$alias c=&apos;clear&apos;
# command &apos;c&apos; will clear the screen

# to supply default parameters
$alias rm=&apos;rm -i&apos;
# command &apos;rm&apos; will execute remove command in interactive (-i) mode.

# to view aliases
$alias c rm
c=&apos;clear&apos;
rm=&apos;rm =i&apos;
```

- **unalias**
  - Removes existing aliases.
  - **Check Alias:** Use the *which* command to check if a command has an alias.
  - **Syntax:** *$unalias command\_name*

### Shell Programming

#### Looping Statements

Used when statement(s) need to be executed more than once. They function based on a loop control statement that specifies the execution condition. Termination is important to prevent infinite execution and resource consumption. The three types are: i. *for* loop, ii. *while* loop, and iii. *until* loop.

- **while loop**
  - Executes statement(s) repeatedly until the specified boolean condition becomes false.
  - **Syntax:**

```shell
while [ control_condition ]
do
# body of while loop
done
```

- **Example:**

```shell
#!/bin/bash

let a=0
while [ $a -le 10 ]
do
 echo "$a"
  a=&apos;expr $a + 1&apos;
done
```

- **Execution Flow:** If the condition is \&apos;true\&apos;, the body executes. If \&apos;false\&apos;, the body is ignored, and statements after *done* execute.
  - **for loop**
- Executes statement(s) repeatedly based on a condition.
- **Syntax 1 (C-style):**

```shell
for ((initilization; condition; iteration ))
do
 # body of for
done
```

```
* **Components:** Initialization (run once), Condition (evaluated), and Iteration (updated).  
* **Example:**
```

```shell
let a;
for (( a=1;a<=10;a=a+1 ))
do
 echo "$a"
done
```

```
* **Execution Flow:** Initialization runs once. The condition is checked, the body executes, and iteration executes. Repeats until the condition is false.  
```

- **Syntax 2 (Word-list style):**

```shell
for var in word1 word2 word3 ... wordN
do
 # operation on $var
done
```

```
* **Example:**
```

```shell
for var in 0 1 2 3 4 5
do
 echo "$a"
done
```

```
* **Execution Flow:** The loop iterates over the given set of space-separated items and executes the loop body for each one.  
```

- **until loop**
  - Behaves like a *while* loop but executes *until* the condition becomes true. If the condition evaluated is \&apos;false\&apos;, the body executes.
  - **Syntax:**

```shell
until [ condition ]
do
 # body of until
done
```

- **Example:**

```shell
let a=1
until [ $a -gt 10 ]
do
 echo $a
 a=`expr $a + 1`
done
```

- **Nesting:** Nesting of loops is possible, with no constraint on the count of nested loops.

#### Loop Control Keywords

- **break statement**
  - Terminates the current loop execution entirely.
  - Statements just before *break* are executed; the interpreter then jumps to the statement(s) after the loop.
  - **Syntax (Single Loop):** *break*
  - **Syntax (Nested Loops):** *break n* (where *n* defines the nth enclosed loop to exit from).
  - **Example:**

```shell
#!/bin/bash

let a=1
while [ $a -lt 10 ]
do
 if [ `expr $a / 2` == 0 ]
 then
  break
 fi
 a=`expr $a + 1`
done
```

- **continue statement**
  - Exits from the *current iteration* of the loop.
  - Statements before *continue* are executed. When *continue* runs, the remaining statements inside the loop body are ignored, and the loop proceeds to the next iteration after checking the condition.
  - **Syntax (Single Loop):** *continue*
  - **Syntax (Nested Loops):** *continue n* (where *n* specifies the nth enclosing loop\&apos;s current iteration to exit from).
  - **Example:**

```shell
#!/bin/bash
for (( no=1;no<=10;no=no+1 ))
do
 if [ `expr $no % 2` == 0 ]
 then
  echo "$no is an even number"
  continue
 fi
 echo "$no is odd number"
done
```

#### Conditional Statements

Used to execute specific code blocks based on the evaluation of a condition. The primary conditional constructs are *if* and *case*.

- **if statement**
  - Executes a block of code if a specified condition is true.
  - **Syntax:**

```shell
if [ condition ]
then
  # statement(s) to execute if true
elif [ another_condition ]
then
  # optional alternative statement(s)
else
  # optional statements if all conditions are false
fi
```

- **Example:**

```shell
#!/bin/bash
n=10
if [ $n -gt 5 ]
then
  echo "The number is greater than 5"
fi
```

- **case statement**
  - Simplifies selection among multiple choices by matching a variable against several patterns.
  - **Syntax:**

```shell
case variable in
  pattern1)
    # commands for pattern1
    ;;
  pattern2)
    # commands for pattern2
    ;;
  *)
    # default commands
    ;;
esac
```

#### Parameters

These are variables that hold command-line arguments passed to the script. They are also known as positional parameters.

- **Positional Parameters:**
  - **$0:** The name of the script itself.
  - **$1, $2, ... $9:** The first, second, and subsequent arguments passed to the script.
  - **${10}, ${11}, ...:** Used for arguments beyond the ninth.
- **Special Parameters:**
  - **$\#:** Total number of arguments passed to the script.
  - **$@:** All arguments passed to the script as separate quoted strings ("$1" "$2" ...). Useful for loops.
  - **$\*:** All arguments passed to the script as a single quoted string ("$1 $2 ...").
  - **$?:** The exit status of the most recently executed foreground command. *0* indicates success, and any non-zero value indicates an error.

#### Variables

Variables are memory locations used to temporarily store data in a script.

- **Types of Variables:**
  - **System Variables:** Defined and maintained by Linux (e.g., *HOME*, *PATH*, *SHELL*, *USER*). They are usually defined in uppercase.
  - **User-Defined Variables:** Created by the user. By convention, they are often defined in lowercase.
- **Working with Variables:**
  - **Assignment:** No spaces allowed around the equals sign (*variable=value*).
    - **Example:** *MyName="JetLee"*
  - **Access:** To retrieve the value of a variable, prefix its name with a dollar sign (*$*).
    - **Example:** *echo $MyName*
  - **Read-Only:** Use *readonly* to make a variable\&apos;s value immutable.
    - **Example:** *readonly MyName*
  - **Unsetting:** Use *unset* to remove a variable.
    - **Example:** *unset MyName*

# Linux Architecture

Linux follows a layered architecture: Hardware (innermost) \-\&gt; Kernel \-\&gt; Shell \-\&gt; User/Application Programs (outermost).

- **Hardware Layer:** Consists of all physical devices in the system.
- **Kernel Layer:** The core of the operating system.
  - Acts as an interface between hardware and the shell layer, performing low-level programming.
  - Stores bootstrap programs executed during OS booting.
  - **Responsibilities:**
    - Management of process and interrupts handling.
    - Device driver management.
    - Inter-process management.
    - I/O, file, and disk management.
    - Memory management.
- **Shell Layer:** A user program that provides an an interface between user/application programs and the kernel.
  - Also called the \&apos;command line interface.\&apos;
  - Converts human-readable command input into machine code (a command-line language interpreter).
  - Starts when the user logs in or a terminal is opened.
- **User/Application Programs:** Programs that aid the user and system, often having Graphical Interfaces (e.g., Office tools).
