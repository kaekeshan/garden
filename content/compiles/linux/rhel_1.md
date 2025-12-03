---
title: Red Hat Enterprise Linux - part 01
draft: false
date: 2025-12-02
tags: [linux,dev]
---

## Accessing The Command Line

### Theory pointer

1. __Console__ 

Place where the user gives  input and receives outputs, the user input will be taken into the kernel and output will be displayed in the console.
                
**Virtual console** : Ctrl + alt + F1 // short key  
For Virtual console, the mouse cannot be used for input, only the keyboard can be used
                
2. __BASH__
                
Bash shell : GNU Bourne Again Shell :: is a text based interface, which can be used to input instructions to a computer system.
                
A program called shell provides the command line for Linux.
                
3. __Shell Prompt__
                
`Username @ hostname Directory $` → normal user, # for root user
                
4. __Syntax__
                
`Command [options] [arguments]`
                
 > Command // defines the name  
 > options // set the behavior of the command.  
 > arguments // usually targets
                
e.g.   ls -l /home
                
5. __Logging in over the network__

SSH : secure shell login
                
Logging into a remote machine, we can transfer files between two machines, and for executing commands on the remote machine.
                
### Commands

- `usermod -L username` // lock a user
- `usermod -U username` // unlock user
- SSH

            ssh username@hostname/ipaddress
            e.g. ssh root@172.25.1.11 -X  // -X for graphical support

- exit or ctrl + d // exit from terminal
- `date` // to show current date
- `whoami` // to show the current user
- `useradd <username>`
- `passwd <username>` // set password
- `wc <filename>` // word count in a file, no of lines etc.
- `file <filename/directoryname>` // to know the type of file, ascii text if not empty , else empty, directory if directory name is given
- `cat` // to view a file and gives it content as output
- `tail` // last ten lines of a file, tail - n // to specify n no of lines
- `head` // first 10 lines of a file, we can use -n for specifying the no of lines
- `history` // to get executed command list and their job ids

### LAB
            
```bash
#date
#date +%x
// current date
#date +%r
// current time
#whoami
// current user
#useradd testuser
// test user is created
#passwd testuser
// change/set password
#wc /etc/passwd
// no of lines, no of words, no of characters , file
#file /etc/passwd
#file /etc
#cat /etc/passwd
// display the content
#tail /etc/passwd
#tail -n 4 /etc/passwd
// last four lines
#head /etc/passwd
#head -n 2 /etc/passwd
// first two lines
#history
#!<job_id_of_command>
```
            
## Managing files from Command Line
            
### File system 

1. __File Structure__ 

+ /root  
/ → root or parent directory  
/root → home of parent directory of root user
- /bin : Essential programs to get the system running.
- /boot : Files related to the boot menu or boot loader
- /dev : Information regarding hardware devices are stored.
- /etc : System configuration files
- /home : Users personal folder, home directory of users
- /lib : Support or library files required by software or applications.
- /media : Contains sub folders where storage devices can be mounted
- /proc : Virtual folder containing files representing status and settings  , process information
- /sbin : Essential software for system maintenance, used only for the root user.
- /tmp : Temporary files / folders
- /usr : Essentially, subdirectories containing most software used on the system, including system libraries and documentation
- /var : Data that is vital to the running of the system and  that is constantly being updated. It persists even after switching off the system, handy for troubleshooting
                    
2. __PATH__

- full path or absolute path : fully qualified name e.g. /root/Desktop/file.txt
- relative path : specify only the path necessary to reach the location for the present working directory.

3. __Links between files__
                
Creating multiple names that point to the same file
>Inode number or Index number : unique number to represent a file
                
- Hardlink : creating a backup of a file, the inode number of the files and the link will be same, hence any change in the source will reflect in the hardlink. Even if the file is deleted, the hardlink will remain undeleted.
                    
- Softlink : a shortcut for the file, different inode numbers for file and link. The softlink also gets  automatically removed if we delete the original file..tc
                    
4. __Pattern matching__  

- `*` // any string of zero or more characters
- ? // any single character
- [abc...] // any one character in the enclosed class
- [!abc..] // any one character not in enclosed class
- `[[:alpha:]]` // any alphabetic character
- `[[:lower:]]` // any lowercase character
- `[[:upper:]]` // any uppercase character
- `[[:alnum:]]` // any alphanumeric character

### Commands

- `pwd` // present working directory
- `cd` // change directory
- `touch <filename>` // create a file
- `mkdir <directory_name>` // create directory
- `ls <path>` // list files and directories  
- `ls -l <path>` // long list
- `ls -la <path>` // hidden files
- `ls - lR <path>` // to list subdirectories or files form the directory
- `rm -rf <file/directory name>` // to remove
- `rmdir <dir_name>`
- `cp <source_file> <destination>` // copy
- `mv <source_file> <destination>` // move  
- `ls -i <file_name>` // to list inode number of a file
- `ln <filename> <hardlinkname>` // creating hardlink
- `ln -s <filename> <softlinkname>` // creating softlink
            
### LAB
            
```bash
#pwd 
#cd 
// takes to home directoy of the login user
#ls
#cd Desktop
#cd ..
// one step back
#touch file1
// create a file
#ls
#touch /root/Desktop/file2
#ls /root/Desktop
#ls -l 
#ls -la
// hidden files, start with . symbol
#ls -lR
// subs are viewed
#mkdir test
// test is created
#ls 
#mkdir -p /root/dir1/dir2
// creating subdirectories
#cd /root
#cd dir1
#ls
#cat > file1
// provide the cotent into the file
// press ctrl + z to exit the commandline // editor.
#cat file1
#cp file1 file2
// copy the info form file1 to file2
#touch file3
#ls
#mv file1 file3
// moving the content form file1 to file3
// file1 gets automatically removed
#touch test{1..5}
// create five files
#ls
// to validate
#mkdir dir{1..10}
// create 10 directories
#ls
#touch a b c
// a , b and c files are created
#touch asd
#ls -i asd
// inode no of asd
#ln asd hardasd
// hardlink is created
#ls -i hardasd
// both inodes will be identical
#ln -s asd softasd
#ls -i softasd
// inode number will be different
#cat > asd
#cat asd
#cat hardasd
#cat softasd
// same output in three files
#rm -rf asd
#cat asd
#cat softasd
// no file or directory
#cat hardasd
// act as backup
#mkdir glob
#cd glob
#touch alfa bravo charlie delta basker cast dog esy able 
#ls
#ls a*
// every file start with a
#ls *a*
// a inside name
#ls [ac]* 
// start with a or c
#ls ????
// filename with four characters
#echo mesage
// to display a message
```
            
## Getting help
### Theory
+ man : used to display the user manual of any command that we can run on the terminal.
    - information in man page
                    1. Name
                    2. Synopsis
                    3. Description
                    4. Options
                    5. Exit status
                    6. Return values
                    7. Errors
                    8. Files
                    9. Versions
                    10. Examples
    - sections in man page  
    All man pages are divided into sections, each section designed to provide specific information about a command. In man page, there will be a section number mentioned in brackets for every command.
                    1. User commands
                    2. System calls
                    3. Library functions
                    4. Devices and Special files
                    5. File formats
                    6. Games
                    7. Miscellaneous
                    8. System administration tools and deamons
                    9. Linux Kernel API
                
- pinfo : pinfo is used to read tedtinfo manual pages.
    + structured as hyperlinked information nodes
    + more in-depth documentation than man pages

### Commands
- `man [option] <command>`
- `man -k cd` // all man pages with cd in its name
- `info` // allows navigation and links between pages

### LAB
            
```bash
#man ls
// manual page of ls command, press q to quit
#man 1 ls
// section no of ls is 1
#man -k cd
// search for cd in maual page names
#pinfo ls
// detailed info, q to quit
#info ls
// press q to exit
```
            
## Creating, Viewing AND Editing Text Files
### Theory

1. __Redirecting output__
                
Terminologies  
- stdin : standard input : user input to terminal, form terminal to processor  
- stdout : standard output : output for the corresponding input from the processor to the terminal
- stderr : standard error : if input is not valid, error is generated by the processor and outted via terminal // error can be redirected to a file
                
2. __Pipelines__ 

A pipe is a form of redirection, i.e. transfer of output to some other destination, here the output of one command / program / process to another.
+ e.g. `cat /etc/passwd | grep root`
+ grep tool can be used to filter results  

![](Untitled.png)
                
3. __Editing using [[scribbles/setups/neo-vim/| vim ]]__
    - improved version of vi text editor
        - Vim editor mods
            - command mode - default startup mode, used for navigation, cut and paste operation and other text manipulations, press esc to get into command mode from modes
            - insert mode
                - i button to enter into insert mode, here we can input into file.
            - visual mode
                - multiple characters may be selected for text manipulation.
                - v for mulit line
                - ctrl + v for block selection
             - extended command mode
                - : begins extended command mode for writing and saving the file and quitting the editor
        - vim basic workflow
            - i : insert mode
            - esc : form insert to command mode
            - u : undo the most recent edit
            - x : delete single character
            - :W // writes or save the file and remain in the command mode for further editing
            - :wq! // to write the file and quit vim
            - :q! // to quit vim, but discard all changes since last write
            - y : yank , copy
            - p : paste
            - v : character mode
            - ctrl + v : block mode
            - shift + v : line mode

### Commands

- `> file` // redirect stdout to overwrite a file
- `>> file` // redirect stdout to append a file
- `2> file` // redirect stderr to overwrite a file
- `2> dev/null` // discard stderr by redirecting it to /dev/null
- `> file 2>&1 &> file` // redirect stdout and stderr to overwrite the same file
- `&>>file`  // redirect stdout and stderr to append to the same file

### LAB
            
```bash
#date > /tmp/saved-timestamp
#cat /tmp/saved-timestamp
// redirected stdout
#date >> /tmp/saved-timestamp
#cat /tmp/saved-timestamp
// redirected stderr
#dat 2> /new
#cat /new
#cat /etc/passwd | grep root
#vim <filename> 
// using vim to edit a file
- insert mode, i 
- save and quit, :wq!
- quit without save , q!
#vimtutor 
// detailed info regarding vim editor, esc + :q! to quit
```
            
## Managing Local Users and Groups
### Theory

1. __User__

    - Every process on the system runs as a user
    - every file is owned by a user
    - access to files and directories are restricted by user
    - the user associated with running process determines the files and directories accessible to that process.
    - system distinguishes user account by the unique identification number assigned to them
    - info about local users are stored in /etc/passwd
    - Types of users
        + superuser : user account for administration of system, name of superuser is root and the id is 0, full access to the system.
        + system user : User account which are used by process that provides supporting devices. Users do not interactively login using a system user account.
        + regular user : used for day - to - day work. Limited access to the system.
                        
2. __/etc/passwd__ : format
                    
```bash
username:password:UID:GID:GECOS:/home/dir:shell
```
                    
+ username : mapping of UID to a name
- password : value X is give, encrypted form, information can be obtained in `/etc/shadow`
- UID : user id , a unique identifier
- GID : group id , primary group of the user is created when a user is normally created. They have the same name.
- GECOS : arbitrary text , usually original name of user.
- /home/dir : home directory of the user
- shell : login permission, normally `/bin/bash` , they have login permission. `/sbin/nologin` // no login permission.
                    
3. __Groups__

    - Group is a collection of users that needs to share access to files and other system resources.
    - Groups can be used to grant access to a set of users , instead of only one single user.
    - groups have GID for unique group identification
    - by default, system uses /etc/group file to store information about local groups
        - format
            ```bash
          groupname:password:GID:list_of_users_in_group
          ```
                    
        - Types
            - primary groups
                - every user has exactly one primary group
                - for local users, the primary group is defined by the GID of the user in `/etc/passwd`
                - the primary group owns the new files created by that user
                - normally, the primary group of a newly created user is a newly created group with the same name as the user. The user is the only member of this **User Private Group (UPG)**
            - supplementary / secondary group
                 - User may be a member of zero or more supplementary group
                 - the users that are supplementary members of local groups are listed in the last field of the groups's entry in /etc/group
                 - For local groups , user membership is determined by a comma separated list of users found in the last field of the group's entry in /etc/group
                        
       - superuser access
            - complete access
            - superuser in rhel is root user
            - switching user // you have to provide the password  
                su - <username>  
                su - // switch to root user
                        
            - sudo
                - used to acquire root privileges without switching the user
                - config file for sudo is `/etc/sudoers`
                - sudo requires users to enter their own password for authentication, not the password of the account they are trying to access, like in su.
                - to access the sudo command, the user must be a member of WHEEL group or go to /etc/sudoers sys file and add user details in that file.
                - configuring sudo
                    - main file is /etc/sudoers
                    - supplementary file /etc/sudoers.d/
                        1. create /etc/sudoers.d/user_name
                        2. or edit sudoers file visudo command with the following command
                            
                        ```bash
                            	
                        username ALL=(ALL)  ALL
                        %groupname ALL=(ALL)  ALL
                        uesrname2 ALL=(ALL) NOPASSWD:ALL
                        // allow username2 to run the command without givin password
                        ```
                            
4. __UID range__

    - UID 0 : for superuser account
    - UID 1-200 : range of system user, assigned statically to system processes
    - UDI 201-999 : range of  system users used by the system processes that do not own files on the file system.
    - UID 1000 + : range of id available to regular users.

5. __User Password__

    - encrypted password, or password hashes are stored in the more secure /etc/shadow file
    - format of /etc/shadow file
    ```bash
    name:password:lastchange:minage:maxage:warning:inactive:expire:blank
    ```
- name : name of user
- password : encrypted
- last change : last pass change
- minage : minimum number of days before a password may be changed, default = 0, no minimum age requirement
- maxage : the maximum no of days before a password must be changed
- warning : warning period that a password is about to expire
- inactive: the no of days an account remains active after a password has expired
- expire : the account expiration date, represented by the no of days since 1970 . 01. 01
- Blank : reserved for future use
                
    - format of password // has three parts, separated by $ symbol
        - e.g. `$1$gCjLa3/z$6Pu0EK0AzfCjxjv2hoLOB/`
            - $1 : id of hashing algorithm, 1 indicated md5, 6 for SHA - 512
            - $gcjLA3 : random combination, it prevents identical entries, it is called salt
            - 6PPu0EK0AzfCjxjv2hoLOB/ : encrypted hash
                
### Commands
- `su - <username>` // switch user
- `su -` // switch to root user
- `useradd <username>`
- `userdel <username>` // deletes the info of user from /etc/passwd, but leaves the home dir of the user untouched
- `userdel -r <username>` // to completely remove the info and home directory of the user
- `id <username>` // info like uid, gid and group name, `id` // similar info of the current user
- `usermod <option> <username>` // to modify the user properties
- `-c` , `—comment` // GECOS field value , such as full name of user
- `-g`  // specify the primary group id
- `-G` // specify supplementary groups
- `-a` // used with the G option to append a user to a group
- `-d` // specify the home directory of the user
- `-m` // to move user home directory  to a new location
- `-s` // specify a new login shell for the user account
- `-L` , `-U` // lock and unlock user respectively
- `-s /sbin/nologin <username>` // to set no login shell
- `passwd <username>` // to change password
- `groupadd <groupname>`
- `groupadd -g <id> <groupname>` // to specify GID, and add a group
- `groupadd -r <groupname>` // create a system group
- `groupmod -n <newname> <currentname>` // to change the present name of a group
- `groupmode -g <gid> <gropuname>` // to change the groupid
- `groupdel <groupname>` // to delete a group
// password aging            
- `chage -m <minimum days> -M <maximum days> -W <warning days> -I <inactive days> <username>`
- `chage -d 0 <username` // force a password update on next login
- `change -E YYYY-MM-DD <username>` // to set expiratory date for the account itself
- `date -d "+45 days"` // date command to calculate date in the future
        
### LAB
            
```bash
#useradd tom
#passwd tom
// creating and setting password for a user tom
#su - tom
// switching to tom
$pwd
// validate the home dir of tom
$su - 
// to switch to root
// provide root password
#pwd
// validate the home dir
$su - tom
$sudo useradd jerry
// using sudo
// provide pass of tom
// error occured
$su - 
#usermod -aG wheel tom
// adding tom to group wheel
#groups tom
// display group information
#su - tom
$sudo useradd jerry
$tail /etc/passwd
// info of jerry and tom
$su - 
#vim /etc/sudoers
            	root ALL=(ALL)   ALL
              -- add user here --
              %wheel ALL=(ALL)   ALL
              -- add group here --
#groupadd redhat
// create redhat group
#groups tom
#usermod -g redhat tom
// change the primary group
#groups tom
// verify the change
#id tom
#id 
// current logged in user info 
#tail -n 3 /etc/passwd
// check shell
#usermod -s /sbin/nologin tom
#tail -n -2 /etc/passwd
#tail /etc/groups
#groupmod -g 3000 redhat 
// changed the gid
#cat /etc/shadow
#chage -l tom
// info about password age
#chage -m 2 -M 90 -W 3 -I 2 tom
#chage -l tom
// verify change
#chage -E 2020/12/2 tom
#chage -i tom
#date -d "+45 days"
// date after 45 days from today
#chage -d 0 tom
// change passwd on first login
#chage -L tom
// lock
#chage -U tom
// unlock
```
            
## Controlling Access to Files
### Theory

1. __Permissions__

    - access to file is controlled via permission
    - UGO concept
        - Owner user : the file is owned buy a user, normally the creator
        - Group owner : the file is also owned by a single group, usually the primary group of the user who created the file, but it can be changed
        - Other : all other users on the system that are not the user or a member of owner group
     - types of permission
        - read (r)
            - contents of the file can be read
            - contents of the directory can be read
        - write (w)
            - contents of the file cab be changed
            - any file in the directory may be created or deleted
        - execute (x)
            - files can be executed as commands
            - contents of the directory can be assessed (depending on the permission of the files in the directory)
        - format
             - ls -l <file>  
             ![](Untitled%201.png)
             - '-' for file types
             - 'd' for directory
        - changing file permissions // use chmod command, flag -R : recursive 
                    
             - symbolic method
                - u - user
                - g - group
                - o - other
                - a - all
                - \+ // add
                - \- // remove
                - = // set exactly
                - r // read
                - w // write
                - x // execute
             - numeric method // chmod ### <file | dir>
                - r = 4
                - w = 2
                - x = 1
                - Each # represents an access level, user or group or other
                - value of # is sum of r, w and x
2. __Types of files__

    1.  \- : regular file
    2.  b : binary file
    3.  c : character file
    4.  d : directory
    5.  l : symbolic link
    6.  s : socket file
    7.  p : piped line

3. __Special permission__

    - u+s or suid // execute the file as the user who owns the file, no effect on diectories  
    = value for setuid = 4
    - g+s or sgid // execute the file as the group who owns the file. Files newly created in the dir have their group owner set to match the group owner of the directory  
    = value of setgid = 2
    - o+t or sticky // no effect on files, users with write on the directory can only remove the files that they own, they cannot remove or force saves to files owned by other users  
    = value for sticky bit = 1

4. __Default permissions__

    - created by root:
        - file : 644
        - dir : 755
    - created by user:
        - file : 664
        - dir : 775
5. __Umask value__

        - every process has a umask
        - octal bitmask that is used to clear the permission of the new files and directories created by the process
        - if a bit is set in the mask, then the corresponding permission is cleared in new files
        - actual permission = base permission - umask value
        - modify /etc/bashrc and /etc/profile to change the default umask for bash shell users
        - users can override the system defaults in .bash_profile and .bashrc files in the home directories
        
### Command
- `ls -l <file>`
- `ls -ld <dir>`
- `chmod [options] <argument>`
            
// user or group ownership
            
- `chown username <file | dir>` // change the ownership
- `chown :groupname <file | dir>` // to change group ownership
- `chown user:group <file | dir >` // change both user and group
- `umask 000` // setting umask to 000

## Accessing Linux Filesystem
### Theroy

1. __Partition__  

Hard disks and storage devices are normally divided up into smaller chunks called partition. To use a partition, it has to be formatted with a filesystem
                
Different parts of storage devices can be formatted with different file systems of used for different purposes
                
2. __Block devices__
                
Storage device format is called block device and it is stored in /dev directory.
In RHEL, first SCSI, PATA/SATA or USB hard drive detected is /dev/sda, the second is /dev/sdb and so on.
            
3. __Mounting__

To use the disk space, partition the disk space, then format it with a filesystem, then create a mountpoint, and attach the partition to that mount point

Mount point is a directory
                
A file system residing on a SATA/PATA or SCSI device needs to be mounted manually to access it. Mount command is used for this
                
For every partition, there exists a unique UUID
+ `mount <filesystem_for_mounting> <target_mountpoint>`
+ `umount <mountpoint>`
                
4. __Locating files on the system__

We can locate files by name, with 'locate'command. 'find' command is also used
                
### commands
- `df` // disk free, amount of free space
- `df -h` // human readable option
- `du` // disk usage
- `du -h` // human readable form
- `blkid <partition>` // to view UUID
- `mount /dev/vdb1 /mnt/mydata`
- `umount <mountpoint>`
- `isof` // list of all open files
- `locate passwd` // search passwd in name or path
- `locate -n 5 <file>` // result is limited to 5
- `updatedb` // update locate database
- `find / -name <file>` // search file by filename in root
- `find / -name *.txt` // search file by .txt extension
- `find / -iname '*message'` // case insensitive search
- `find / -user <usernme>` // search for user owned files
- `find / -group <groupname>` // group owned files
- `find / -perm <permission>` // search for files with specific permissions
- `find / -size 10M` // files with size 10M
- `find / -size +10M` // more than 10M
- `find / -size -10M` // less than 10M

### LAB
            
```bash
#df -Th 
// -T is for type
#du -sh /etc
// s for size
#blkid 
// uuid of partition
#lsblk
// list the block, device name, size , mountpoint, partitions can be viewed
#locate passwd
#locate -n 5 passwd
// limit to first 5 output
#find / -name passwd
// by name, in /
#find -/home -name passwd
#find / -iname passwd
// case insensitive
#find / -user root
// files owned by root, ctrl + c for termination
#find / -group root
// files owned by group root
#find / -perm 666
// permission 666 files
#find / -size 10M
#find / -size +10M
#find / -size -10M 
#find / -user root -type f
// search for files only
#find / -user root -type d
// search for directory only
#find / -user root -type -l
// search for softlinks
```

