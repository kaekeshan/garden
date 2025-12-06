---
title: Linux Administration - Overview
draft: false
date: 2025-12-06
tags: [linux]
---

## Module 01 - Files and Directories

- Working with directories
    
    ```text

    # print working directory
    $pwd
    syntax : pwd
    example output : /home/jetlee
    
    # change directory
    $cd 
    syntax : $cd <absolute / relative path>
    example : 
    $cd /etc
    if pwd is /home
    $cd jetlee
    
    if cd is used alone, it will navigate to current user's home directory
    
    absolute path starts with / , that is starting point is root directory at the top of 
    file hierarchy.
    relative path start without a /, with the starting point as the current directory
    
    $cd ~ 
    this command will change the directory to users hone directory
    example output : /home/jetlee
    
    $cd ..
    this command will navigate user to the parent directory of the current directory. 
    
    # list contents of a directory
    $ls
    syantx : $ls <path>
    if path is not given, it will list the files and folders of the current directoy, else 
    of the <path> specified.
    example :
    $ls /etc
    $ls 
    
    $ls -l 
    this command is called the long list, it will display additional information about 
    the listed files and folders like permission, owners, dates etc
    
    $ls -h 
    this command will list the contents in a more human readable form, especially the 
    content size
    
    $ls -a
    this command will list all contents. i.e. even those contents which are hidden. A file 
    or directory starting with a dot (.) is hidden in unix and linux. Hence to display 
    such contents , -a argument can be given
    
    # to create a directory
    $mkdir directoy_name
    the command will create a new directory with specified 'directory_name' as its name.
    it requires atleas one argument, i.e. directory_name
    
    $mkdir -p /parent1/parent2/directory
    this -p argument can be used to create parent directories
    
    # to remove directories
    $rmdir directory_name
    the command will remove directory with specified 'directory_name'
    ```
    
- Working with files
    
    ```text

    # to know the type of files
    $file <filepath>
    the linux does not use extensions like *.txt, *.pdf etc. to communicate 
    about file, instead file command can do that for the user.
    example
    $file example.txt
    
    $file -s <filename>
    the argument s can be used to know about special files , normally inside /proc 
    or /dev
    example
    $file -s /dev/sda1
    $file -s /proc/cpuinfo
    
    # to create empty files
    $touch filename1 < filename2,..>
    the command will create empty files for the user, we can specify the entire path
    if available
    $touch /home/jetlee/file1
    We can create mulitple files in one go like below
    $touch file1 file2 file3
    
    $touch -t <properties> filename
    the command can be used to set some properties to the file while creation itself.
    example
    $touch -t 20220420000 test.txt
    
    # to remove files
    $rm <path to file | directory>
    the cmmand will remove specified file or directory permanantly since the trash like
    recovery systems are not avaliable in command line.
    
    $rm -i <path to file | directory>
    the command provides an additional promt asking for confirmation to prevent user from
    accidently deleting important data. here i stands for interactive
    
    $rm -rf <path to directroy>
    the command can be used to delete non empty directories. r stands for recursive and f
    stands for force. Recursive is an option suited for directories since it removes all the 
    files and sub directories recursively.
    
    example
    rm -rf testDirectory
    rm testFile.txt
    
    # to copy files
    cp source target
    copy the source to target destination. Both arguments can be passed as absolute paths.
    target could be file or directory.
    
    cp source1 <source2,..> target
    the command can be used to copy muliple files to target destination. the target should
    be a directory
    
    cp -r directory target
    to copy files or subdirectories recursively in a directory
    
    cp -i source target
    to copy files and provide with a promt asking for permission if overwriting files.
    
    # to move and rename files
    $mv source target
    $mv oldname newname
    
    $mv -i // like rm and cp
    $mv -r // to move recursively
    
    example
    $ls
    file1.txt
    $mv file1.txt backup.txt
    $ls
    backup.txt
    ```
    
- file structure
    
    Linux partially follows Filesystem Hierarchy Standard or FHS.
    
    - / - root directory
        
        In Linux system, everything is file, and is arranged in hierarchical fashion. At the top of the hierarchy, we have the root directory. It is the starting point for the file system and everything in Linux resides under root.
        
    - Binary Directories
        
        these directories contains compiled source codes or machine codes of different programs that are essential for the user and system.
        
        - /bin
            
            contains software that are available for single-user mode; for all users for day to day activities.
            
            example: cat, grep, cp, mv 
            
        - /sbin
            
            the directory contains system binaries that are essential for configuring the operating system. Majority of the programs require root privilege to run for changing the ip addredss, partitioning the disk, changing the file system etc. 
            
            example : ipconfig, mkfs.ext4, fdisk etc.
            
        - /lib
            
            lib contains shared libraries used by programs in /bin and /sbin.
            
            example: libconsole.so.0.0.0, libcfont.so.0, libc-2.5.so
            
        - /opt
            
            this directory contains optional software that are outside from the distribution repository and could be empty in many systems.
            
            Another use is that, a large application can store its files inside /opt/$packagename/ in subdirectories called /bin /etc /man /lib etc.
            
            if the packages name is test, we have
            
            /opt/test/etc, /opt/test/lib, /opt/test/bin, /opt/test/man
            
    - Configuration Directories
        - /etc
            
            contains machine specific configuration files. Usually the files will have the same name as the package name, but with .conf extension. The ‘etc’ historically stands for ‘etcetera’ but now it is ‘Editable Text Configuration’ in Linux.
            
            /etc/*.conf examples

            host.conf, adduser.conf, deluser.conf, updatedb.conf  

            Important sub directories inside /etc are : 

            /etc/init.d/ , /etc/X11/ , /etc/sysconfig/ 
            
    - Data directories
        - /home
            
            this is the directory where the normal user stores his / her personal or professional data. By Filesystem Hierarchy Standards, each user has a subdirectory under /home like /home/$Username/ to store data. Also user profiles are kept inside their corresponding home directory via hidden files like .bashrc, .bash_profile etc. ‘cd’ or ‘cd~’ statements redirects a normal user to users home directory. 
            
        - /root
            
            it is the home directory of the root user where the user can keep his/her data. It also contains profile of the user as hidden files.
            
        - /srv
            
            contains data that is served by your system. FHS locates ftp, www, cvs, rsync data in /srv and approves administrative naming in /srv 
            
            example : /srv/sales/www
            
        - /media
            
            this is the directory which is used as mount point for external removable media devices like CD ROMs, digital cameras, and similar USB- attached devices. 
            
            /media/ content example  
            usbdisk, cdrom, cdrom0, usbdisk0
            
        - /mnt
            
            this is the directory which acts as mount points for local or remote file systems. According to FHS, /mnt/ should be used as a temporary mount point and kept empty. However, Linux and Unix systems have sub directories inside /mnt/ 
            
            example : /mnt/sda0, /mnt/sda1, /mnt/hdd1
            
        - /tmp
            
            contains temporary files stored by users and programs. The stored data may either use local storage or RAM storage. Both type are managed by Operating System.
            
    - In memory directories
        - /dev
            
            the directory gets populated with files as the kernel is recognizing hardware. 
            
            example : common hard drive devices are found inside /dev like SATA device files for laptop like /dev/sda0, /dev/sdb0 etc or IDE attached hard drives for desktop like /dev/hda0, /dev/hdb0
            
            /dev/null → has unlimited storage, known as a storage blackhole. Nothing can be retrieved from it.
            
        - /proc
            
            This is another special directory where files may seem ordinary, but provides vital information about system. It contains files which even though has a lot of data, sill has less storage size, some times even 0 kB. This directory contains files about kernel or what kernel manages. It can be used to directly converse with kernel. Most files are read only, some require root privileges, but some files in /proc/sys are writable.
            
            example: /proc/cpuinfo ,/proc/interrupts, /proc/kcore
            
    - /usr  
    This directory name stands for Unix System Resources and contains sharable, read-only files. its sub directories include
        - /usr/bin/ → binary files , in some systems /bin is the symbolic link to /usr/bin
        - /usr/src/ → ideal location to store kernel source files
        - /usr/local/ → admins can locally install a software in this directory
        - /usr/share/ → files that are architecture independent
        - /usr/include/ → contains required c header files
        - /usr/lib/ → library files that are not directly executed by user or scripts
        etc.
        
    - /var  
    Directory contains variable data like log, cache, and spool(simultaneous peripheral online ) files. 
        - Important subdirectories include
            - /var/log/ → central point for log files
            - /var/cache/ → main storage directory for cache data
            - /var/spool/ → contains spool directories for mail and cron and other spool files.
        
- Linux Architecture
    
    The Linux follows a layered architecture where, the innermost layer is hardware, on top of hardware, the kernel resides, on top of kernel, we have shell and on at the outer layer we have user or application programs. 
    
    - Hardware layer : hardware layers consists of all the physical devices in the system. That is all things associated with the system that can be physically touched is a hardware component.
    - Kernel layer : kernel is the core of the operating system. It acts as an interface between the hardware and the shell layer and it performs low level programming. Also bootstrap programmes are stored in kernel. i.e. those programs that are executed during the booting process of operating system.
        
        Different responsibilities of kernel includes
        
        - management of process and interrupts handling
        - device driver management
        - inter process management
        - i/o , file and disk management
        - memory management
    - Shell layer : shell is a user program that provides an interface between user or application programs and kernel of the system and through shell user can avail services of operating system. Hence it is also called as ‘command line interface’. It converts the human readable command like input to machine code that is understandable to kernel. Hence it is a command line language interpreter. It starts when the user logs in or when a terminal is opened.
    - User of application programs : these are different programs that aid the user and system in daily interactions. Most have Graphical interfaces. Office tools like MS office are an example of such programs.
    

## Module 02 - Process

- Process
    - Fundamentals
        - Processes are program in execution or instances of programs.
        - Linux is a multiprocessing operating system, i.e. it supports execution of several processes simultaneously.
        - OS will create suitable environment for process by allocating required resources to it.
        - An example of a process creation is when user runs a command in shell. example : ‘mdir’ is a command to create a directory and the command has a corresponding program in the system (/bin). Whenever the command is invoked, the program gets executed and a process is created
        - Each process has a id associated with it called pid or ‘process id’.
        - Pid helps OS to uniquely identify each process and it contains 5 digits.
        - If all possible combination of pids are used up, next batch of pid rolls up
        - No two process with the same pid can exists in the system at the same time since it is used as a unique identifier to track each process.
    - Types
        1. Foreground processes
            1. default mode
            2. processes that engage the user interface
            3. receives input from the keyboard and displays output in the monitor
            4. example would be running ‘pwd’ command, its output will be displayed on screen
            5. disadvantage of foreground process OR requirement of background process
                1. If a process takes a lot of time to complete its execution, user has to wait for its completion to start another process since user interface is currently engaged.
        2. Background processes
            1. process that run in background without keyboard input. 
            2. usually process having longer execution time are managed as background processes.
            3. they will wait if user input is required
            4. this enables the parallel execution of several processes since each process can begin its execution without waiting for the completion of previous processes.
            5. a foreground process can be converted to background process by appending ‘&’ at the end of the command.
            6. example would be : pwd &
    - commands
        1. ps command 
            1. ps when expanded stands for process status will output currently running processes.
            2. the output will have process id, user, uptime and command.
            3. ps -x will output the processes started by the current user.
    

## Module 03 - Getting System Information

- Getting system information
    - uname command
        
        ‘uname’ command displays detailed information about the system
        
        syntax: uname [option]
        
        - -a // all information
        - -r // kernel release information
        - -s // kernel name information
        - -n // network node hostname information
    - hostname command
        
        ‘hostname’ command is used to 
        
        - obtain the Domain Name System name
        - to set the host name and domain name of the system
        - hostname is given to a device connected over a network in order to uniquely distinguish the system.
        - hostname command is usually specified in start up script files located in /etc/rc and /etc/rc.local
        - edit the hostname command in these files will change the hostname of the system in the next reboot onwards.
        
        syntax
        
        hostname [-options] [file]
        
        example
        
        hostname -s tester.test.com
        
        - -a // to display aliases of hostname if available. returns empty line if no aliases are available
        - -s // if the hostname has a dot, the portion before the dot is called shorthostname; this argument retrieves short hostname.
        - -i // this retrieves the IP address of the system assigned against the hostname.
    - who command
        
        used to get the following information
        
        - last time of boot
        - current run level of system
        - list of logged in users
        
        syntax
        
        who [-option] [file]
        

## Module 04 - Shell

- types of shell
    1. c shell 
        - denoted by ‘csh’
        - founded by Bill Joy , at the university of California, Berkely
        - interactive features like
            - alias
            - in built arithmetic
            - command history
            - c like syntax
        - path : /bin/csh
        - normal user prompt : username%
        - root user prompt : username#
    2. bourne shell
        - denoted by ‘sh’
        - Stevie Bourne in AT & T Bells Labs
        - features
            - fast
            - original UNIX shell
            - default for many unix based os
        - does not have any interactive features like
            - command history
            - arithmetic capacity
            - logical expression handling
        - path : /bin/sh
        - normal user prompt : username$
        - root user prompt : username#
    3. korn shell
        - denoted by ‘ksh’
        - David Korn at AT & T Bells Labs
        - super set of Bourne Shell
        - compatible with c shell specific scripts and faster than c shell
        - include features of Bourne  Shell
        - interactive features like
            - arithmetic
            - c arrays
            - functions
            - string manipulation
        - path : /bin/ksh
        - normal user prompt : username$
        - root user prompt : username#
    4. bourne again shell
        - denoted by ‘bash’
        - include features of Bourne shell and Korn shells
        - path : /bin/bash
        - normal user prompt : username-g.gg$
        - root user prompt : username-g.gg#
            - [g.gg](http://g.gg) → corresponds to the shell version
            - example: host-3.1.0$
- alias and unalias
    - alias
        - create aliases (replace command with a name that is familiar to user)
        - easily supply parameters to command
        - alias aliasname=’some existing command name’
        
        ```text
        # create alias
        $cat count.txt
        one 
        two
        $alias say=cat
        $say count.txt
        one 
        two
        
        # create abbreviations
        $alias c='clear'
        the command 'c' will clear the screen
        
        # to supply default parameteres
        $alias rm='rm -i'
        the command 'rm' will execute remove command in interactive (-i) mode.
        
        # to view aliases
        $alias c rm
        c='clear'
        rm='rm =i'
        ```
        
    - unalias
        - to remove existing alias, use unalias command
        - to check if a command has alias or not, use ‘which’ command
        - to remove the alias : use
            
            $unalias command_name
            
- Shell programming
    - Looping statements
        
        Used when statement(s) need to be executed more than once in a script. They function based on a loop control statements which specifies the condition for executing each iteration. Termination  of a loop is important because, if not specified loops can execute infinitely, consuming all available resources of the system. There are three types in shell programming:
        
         i. for loop, ii. while loop and iii. until loop
        
        - while loop
            
            while loop is used to execute statement(s) repeatedly until the specified condition becomes false. The specified condition should have a boolean output.
            
            ```bash
            while [ control_condition ]
            do
            // body of while loop
            done
            ```
            
            ```bash
            # example
            #!/bin/bash
            
            let a=0
            while [ $a -le 10 ]
            do
            	echo "$a"
              a='expr $a + 1'
            done
            ```
            
            Here, the control condition is evaluated. If the output is ‘true’ then the statement(s) inside the body of loop (inside do - done) is executed until the condition becomes ‘false’. If the condition is false, the body of the loop is ignored and statement(s) after ‘done’ is executed.
            
        - for loop
            
            Like while loop, for loop is also executes statement(s) repeatedly based on some condition. If the condition evaluated is true, then the loop’s body of statements will execute, else the body is ignored completely and statement after the loop gets executed.
            
            1. **syntax 1** 
            
            ```bash
            for (initilization; condition; iteration )
            do
             // body of for
            done
            ```
            
            for loop consists of three components, the initialization, where the loop control variable gets initialized, and condition statement, where the loop condition is evaluated to Boolean, and iteration, where the loop control variable gets updated.
            
            if the condition is true, body gets executed and if it is false, the statement(s) after ‘done’ is executed. 
            
            ```bash
            let a;
            for ( a=1;a<=10;a=a+1 )
            do
            	echo "$a"
            done
            ```
            
            the flow of execution is , the initialization is executed once. the condition is checked, the statement(s) in body are executed and iteration part is executed. Again the condition part is evaluated and the cycle repeats until condition becomes false.
            
            1. **syntax 2**
            
            ```bash
            for var in word1 word2 word3 ... wordN
            do
            	// operation on $var
            done
            ```
            
            ```bash
            for var in 0 1 2 3 4 5 
            do
            	echo "$a"
            done
            ```
            
            The loop iterates over the given set of characters separated by space and executes the body of the loop. 
            
        - until loop
            
            until loop behaves just same as while loop with a twist in evaluation condition. The loop executes until the condition becomes true. i.e. if the condition evaluated is ‘false’ the body of the loop gets executed.
            
            ```bash
            until [ condition ]
            do 
             // body of until
            done
            ```
            
            ```bash
            let a=1
            until [ $a -gt 10 ]
            do
            	echo $a
            	a=`expr $a + 1`
            done
            ```
            
        
        Nesting of loops is possible, i.e. one loop inside other loop structures and there is no count constraint on the number of loops nested.
        
    - Loop control keywords
        - break statement
            
            The statement is used to terminate the current loop execution. The statement(s) just before the break statement are executed. When break statement is executed, the interpreter will go to the end of the loop and execute statement(s) after the loop.
            
            syntax is :  break
            
            We can employ break statement in nested loop structures. 
            
            syntax is : break n
            
            here n defines the nth enclosed loop to exit from.
            
            ```bash
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
            
        - continue statement
            
            The statement behaves like break statement as it also manipulates the loop execution. Instead of exiting from the entire loop structure, the continue statement exits from the current iteration of the loop. All statements until continue statement is executed. When continue statement is executed, remaining statements inside loop body is ignored and loop begins its next  iteration, after checking condition. 
            
            syntax is : continue
            
            we can employ continue in nested loop structures
            
            syntax is : continue n
            
            here n specifies the nth enclosing loop’s current iteration to exit from.
            
            ```bash
            #!/bin/bash
            for ( no=1;no<=10;no=no+1 )
            do
            	if [ `expr $no % 2` == 0 ]
            	then
            		echo "$no is an even number"
            		continue
            	fi
            	echo "$no is odd number"
            done
            ```
            
    - Conditional statements
    - Parameters
    - Variables
