---
title: Red Hat Enterprise Linux - part 02
draft: false
date: 2025-12-03
tags: [linux,dev]
---
## Improve command line productivity
### Theory
1. __Bash scripts__
    - using vim editor, extension is **.sh**
        - script example 
            ```bash
           #! /bin/bash
           
          echo hello world
          ```
                    
            - give execution permission using `chmod +x test.sh` 
            - execute the file using `./test.sh`
        - script
            ```bash
           #! /bin/bash
           
          if [ 10 == 10 ]
          then
          echo they are equal
          fi 
          ```
                    
         - script
           ```bash
           #! /bin/bash
           if [10 == 5]
           then
           echo they are equal
           else
           echo they are not equal
           fi
           ```
                    
        - complex script
            - vim userlist
                ```bash
               user1
               user2
               user3
               user4
               ```
               
            - vim new.sh
                ```bash
                #! /bin/bash
                
               if [ $# == 0 ]
               then
               echo enter the file name
               elif [ -f $*  ]
               then
               for user in $(cat $*)
               do
               useradd $user
               done
               else
               echo enter a valid filename
               fi
               ```
                    
                - chmod +x new.sh
                - ./new.sh // no file input
                - ./new.sh userl // invalid filename
                - ./new.sh  userlist
                - tail -n 5 /etc/passwd
                
2. __Grep command__
    - used for pattern filtering  
        e.g. : grep root /etc/passwd
    - option
        - -i // case insensitive
        - -v // excluding the lines having the specified string
        - -A // extra lines after the matching output
        - -B // extra lines before the matching output
        - -e // using multiple search strings
        - -r // to use directory

### Commands

- `echo hai`
- `echo "hai"` // result wil be same
- `echo $(date)` // date is printed
- `echo "Today's date is "$(date)`
- `echo 'hostname` // prints the hostname, here hostname is treated as a variable
- `grep root /etc/passwd`
- `grep ^root /etc/passwd` // line start with root
- `grep nologin$ /etc/passwd` // line end with no login
- `grep -A 2 ^tom /etc/passwd`
- `grep -e tom -e root -e apache /etc/passwd`
- `grep -r baseurl /etc`

## Schedule future tasks
### Theory

1. __Scheduling__
    - **Deferred** user task
        - run a command or set of command at a set point in future, called job or task
        - the term deferred indicates that these tasks or jobs are going to run in the future
        - 'at' package can be used to manage the scheduling
        - at package
            - at package provides atd , system daemon along with a set of command line tools to interact with the daemon.
            - for a default rhel installation, the atd daemon is installed and enabled automatically
            - users can queue up jobs for the atd daemon using the at command
            - the atd daemon provides 26 queues, a to z, with jobs in alphabetically later queues getting lower system priority
        - TIMESPEC command to schedule a new job
        - ctrl + D // for finishing the  inputs
        - combination examples :
            - now +5min
            - teatime tomorrow(teatime is 16:00)
            - noon +4 days
            - 5pm august 3 2021
   - Scheduling recurring user jobs
        - recurring - repeated jobs
        - crond daemon, provided by the cronie package, enabled and started by default for recurring jobs
        - fields of crontab files
            - minutes
            - hours
            - day of month
            - month
            - day of week
            - command
        - field rules
            - first five fields use the same syntax rules
            - * → don not care or always
            - a number specifies number of minutes or hours, a date, or a weekday.
            - for week day, Sunday is 0,— 7 also equals monday
            - x-y for a range , x to y inclusive
            - x,y for list, list can include ranges as well.
            - e.g. 5,10-13,17,..
            - */x indicate an interval of x, for example, */7 in minute column runs a job every seven minutes.*
        - examples
            - `0 9 2 2 * /usr/local/bin/yearly_backup`  
              run the specified path at exactly 9.00 am on Feb 2nd, every year
            - `*/5 9-16 * Jul 5 echo "Chime"` 
              sends an email containing the word chime to the owner of this job, every five minutes between 9 a.m and 5 p.m. on every Friday in July
            - `58 23 * * 1-5 /usr/local/bin/daily/_report`  
              run the command /usr/local/bin/daily_report every weekday at two minutes before midnight.
    - Recurring system jobs
        - recurring jobs of system admins
        - best practice is to run these jobs form the system accounts rather than from user accounts.
        - do not schedule to run these jobs using the crontab command, but instead use system wide crontab files
        - system wide crontab files have and extra field before the command field; the user under whose authority the command should run
        - /etc/crontab file has a useful syntax diagram in the included comments
        - defined location
           - /etc/crontab file
           - /etc/cron.d/ directory
        - place the custom crontab file in /etc/corn.d to protect it from being overwritten if any package update occurs to the provider of /etc/
        - the crontab system also includes repositories for scripts that need to run every hour, day, week and month
        - these repositories are directories called:
            - /etc/cron.hourly/
            - /etc/cron.daily/
            - /etc/cron.weekly/
            - /etc/cron.monthly/
        - these directories contain executable shell scripts
    - /etc/anacrontab file
        - run parts command also  runs the daily , weekly, and monthly jobs, but it is called from /etc/anacrontab config file
        - purpose: make sure that important jobs always run, and not skipped accidently , because the system was turned off or hibernating when the job should have been executed.
        - fields
            - period of days // interval in days for the job that runs on a repeating schedule.
            - delay in minutes // amount of time that crond daemon should wait before starting this job
            - job identifier // the unique name the job is identified as in the log messages.
            - command // the command to be executed
2. __Managing temporary files__
    - modern system require large no of temp files and directories
     - some application use volatile directories under /run to store temp files
     - if the sys reboots or loses power, the files are removed
     - it is necessary for these directories and files to be created when they do not exist and for old files to be purged
     - RHEL has a new tool called **sytemd-tmpfiles**, to manage temporary  directories and files
     - process
        - when systemd starts a system, one of the first service units launched is systemd-tmpfiles-setup
        - this service runs the command systemd-tmpfiles —create —remove
        - this command reads configuration files from /usr/lib/tmpfiles.d/* .conf, /run/tmpfiles.d/*.conf, and /etc/tmpfiles.d/*.conf
        - any files and directories marked for deletion in those config files is removed, and any files and directories marked for creation will be created with the correct permission if necessary

3. __Cleaning temp files with sytemd timer__

    - to ensure that long running systems do not fill up their disks with stale data, a systemd timer unit called systemd-tmpfiles-clean.timer triggers systemd-tmpfiles-clean.service on a regular interval
    - which executes the `sytemd-tmpfiles —clean` command.
    - the systemd timer unit config files have a [timer] section that indicate how often tthe service with the same name should be started
        - `#sytemctl cat systemd-tmpfiles-clean.timer` // to view contents of the systemd-tmpfilesclean.timer unit config file.

4. __Format of the config files of `systemd-tmpfiles`__

```bash
Type, Path, Mode , UID, GID , Age, Argument
```


5. __Examples__

    - `d /run/systemd/seats 0755 root root` 
    when creating files and directories, create the /run/systemd/seats directory if it does not yet exist. owned by the user root and group root, with permissions set to rwxr-xr-x
    - `D /home/student 0700 student student 1d`  
    create /home/student directory if it does not yet exist. if it does, empty it of all contents. When systemd-tmpfiles —clean is run, remove all files which have not been accessed, changed, or modified in more than one day.
    - `L /run/fstablink —root root -/etc/fstab`  
    create the symbolic link /run/fstablink pointing to /etc/fstab

6. __Configuration file precedence__

    - config files can exist in three places
        - **/etc/tmpfiles.d/*.conf**
        - **/run/tmpfiles.d/*.conf**
        - **/usr/lib/tmpfiles.d/*.conf**
    - **/usr/lib/tmpfiles.d/** are provided by relevant RPM packages
    - **/run/tmpfiles.d/** are themselves volatile files, normally used by daemons to manage their own runtime temp files
    - files under **/etc/tmpfiles.d/** are meant for administrators to configure custom temporary locations, and to override vendor provided defaults

### commands
            
- Inspecting and managing deferred user jobs
- `atq` or `at -l` // to get an overview for the pending jobs for the current user
- `at -c JOBNUMBER` // to inspect the actual commands that will run when a job is executed
- `atrm JOBNUMBER` // command removes a scheduled job before its execution
- Crontab
- `crontab -l` // to list the jobs for the current user
- `crontab -r` // to remove all jobs for the current user
- `crontab -e` // edit jobs for the current user.
- `crontab filename` // remove all jobs, and replace with the jobs read form file name. If no file is specified, stdin is used.
- Cleaning and creating temporary files mannualy
- `systemd-tmpfiles —create` // creating files and directories
- `systemd-tmpfiles —clean` // purge all files which have not been accessed , changed or modified more recently than the maximum age defined in the config file.

## Tuning System Performance
## Theory

1. __Tuned daemon__

    - applies tuning adjustments both statically and dynamically, via tuning profiles
    - the tuned daemon applies system settings when the service starts or upon selection of a new tuning profile

2. __Configuring static tuning__

    - it configs predefined kernel parameters in profiles that tuned daemon applies at run times.
    - with static tuning , kernel parameters are set for overall performance expectations, and are not adjusted as activity level changes.

3. __Configuring dynamic tuning__

    - here, the tuned daemon monitors system activities and adjust settings depending on runtime behavior changes.
    - it is continuously adjusting tuning to fit the current workload, starting with the initial settings declared in the chosen tuning profile

4. __Selecting a tuning profile__

    - types
        - power saving profiles
        - performance boosting profiles
            - low latency for storage and network
            - high throughput for storage and network
            - virtual machine performance
            - virtualization host performance
                    
    - available in rhel8
        - Balanced : compromise between power saving and performance boost
        - Desktop : derivative of balanced, fast response to interactive apps
        - throughput-performance : max throughput
        - latency-performance : low latency at the expense of high power consumption
        - network latency : derivative of latency performance, enables additional network tuning parameters to provide low network latency.
        - network throughput: derivative of throughput performance profile, enables additional network tuning parameters to provide high network throughput.
        - power-save : tunes the system for max power saving.
        - oracle : optimized for oracle database loads based on the throughput-performance profile
        - virtual guest  : max performance on vm
        - virtual host : max performance if it acts as a host for virtual machines.

5. __Linux process scheduling and multitasking__

    - technique for running more processes than the processing units is called time-slicing or multitasking
    - process are given different levels of importance
    - SCHED_OTHER // policy used for most processes in a regular system
    - SCHED_NORMAL // policy having relative priority
        - this priority is called nice value of a process. There are 40 different levels of niceness for any process
        - range is form -20 to 19
        - by default, process inherit their nice level form their parent , usually 0.
        - high nice level indicate less priority, while lower levels indicates high priority.
        - Priority = nice value + 20

## Commands

- `tuned-adm` // change the setting of the tuned daemon
    - query current settings
    - list available profiles
    - recommend a tuning profile for the system
    - change profile directly
    - turn off tuning
- `tuned-adm active`
- `tuned-adm list`
- `tuned-adm profile profile_name` // to switch the active profile to a different profile.
- `tuned-adm recommend`
- `tuned-adm off`
- setting nice value for process before starting the process
- `nice sha1sum /dev/zero &` : starts the sha1sum command as a bg job with the default nice level , *// & denotes background*
- `ps -o pid, comm, nice pid` : display process nice level
- `nice -n 15 sha1sum &` : starts the command as a b.g. job with a user defined nice value.
- setting nice value for running process
- `renice -n 19 processid` : change form current nice level to the desired nice level.
            

## Control access to files with ACLs
### Theory

1. __Interpreting file ACL__

    - Access Control list is used to grand access to users comes under others group for a file or directory
   - these additional users and groups are called named users and named groups respectively, because they are named not in a long listing but rather within an ACL

2. __File system ACL support__

    - file systems need to be mounted with ACL support enabled.
    - XFS file system have built in ACL support
    - other file systems such as ext3 or ext4 created on rhel8 have acl option enabled by default
    - to enable file sys acl support, user acl option with the mount command or in the file system's entry in /etc/fstab config file

3. __Viewing and interpreting acl permissions__

    - ls -l <file/dir> // only minimal acl settings details
    - + sign at the end of 10 char permission string indicates the existence of an extended acl structure with entries
   - `getfacl <file | /directory>`
        - example  
        ![](Untitled%202.png)
                    
4. __ACL mask__
    - denotes the maximum permission that you can grant to named users, group owner and named groups.
    - it does not restrict the permission of the file owner or other users
    - all files and dir implemented in acl have an acl mask
    - mask can be viewed via getfacl and explicitly set with setfacl
    - it will be calculated and added automatically if it is not explicitly set, but could also inherited form a parent directory default mask setting
    - by default, the mask is recalculated whenever any of the affected acls are added modified or deleted

5. __Changing ACL file permissions__

    - setfacl to add modify or remove standard acl on files and directories
    - r - read, x - executer, w - write
    - '-' indicated the absence of relevant permission
    - X - indicating that recursive setting of execute permission should only be set on directories and not regular files.
    - -m // for modifying
    - -M // modification passed via files

### Command
- setting access control permissions using acl
    - `setfacl -m user::mary:rx file` : Named user with read and execute permissions for a file
    - `setfacl -m group:admins:rwx` /directory
    - `default:m::rx /directory` : read and execute permission set as the default mask
    - `default:user:mary:rx /directory` : named user granted initial read permission for new files, and read and execute permissions for new subdirectories
- ACL on systemd jounal files
    - `getfacl /run/log/journal/cb44....8ae2/system.journal`  

    ![](Untitled%203.png)

- ACL on systemd managed devices
    - `getfacl /dev/sr0`  

    ![](Untitled%204.png)

- setfacl examples
    - `setfacl -m u:name:rX file` //user of named user
    - `setfacl -m g:naem:rw file` // group or named group
    - `setfacl -m o::- file` // other
    - `setfacl -m u::rws, g:consultants:rX, o::- filei`
    - `getfacl file-A | setfacl —set-file=-file-B` // o.p of getfacl as i.p of setfacl
    - `setfacl -m m::r file` // acl mask explicity
    - `setfacl -R -m u:name:rx dir` // recursive acl modifictions
- delete acl
    - `setfacl -x u:name, g:name file`
    - `setfacl -b file` // to delete all acl entries on a file or directory
- default acl
    - `setfacl -m d:u:name:rx directory`
    - `setfacl -x d:u:name directory` // delete a particular default acl
    - `setfacl -k directory` // to delete all default acl entries

## Managing SELINUX security
### Theory

1. __SELINUX__

    - security enhanced linux
    - protect user data form compromised services
    - user, group, other based model known as **discretionary access model**;
    - SELINUX provides an additional layer of security that is object based and controlled by more sophisticated rules, known as **mandatory access control.**

2. __Why ?__

    - enforces access rules preventing a weakness in one application from affecting other applications or the underlying system. // a weakness in one part of system does not spread to other parts of system.
    - extra layer of security
    - high learning curve, but effective
    - if selinux works poorly with a particular subsystem, you can turn off enforcement for that specific service until you find a solution to the underlying problem

3. __SELinux modes__

    - enforcing // default : enforcing a set of access rules
    - permissive // records warning for violation of rules. Used for testing and troubleshooting.
    - disabled : selinux is turned off entirely, no selinux violations are denied, nor even recorded,
4. __Basic concepts__
    - has rules that determine which process can access which files, directories, and ports.
    - every file, process, directory and port has a special security label called an **SELinux context**.
    - context is a name used by selinux security policy to determine whether a process can access a file , directory or port.
    - by default, no interactions are allowed, unless an explicit rule grants access. If there is no allow rule, no access is allowed.
    - available contexts types
        - user
        - role
        - type
        - sensitivity
    - type context names usually end with \_t
    - targeted policy : the default policy enabled in rhel: rules in targeted policy is based on type context.  

    anatomy of selinux file context:
    `system_u:object_r:password_file_t:s0`
                
5. __SELinux access example__
    - apache : httpd_t
    - mariaDB : msqld_t
    - /var/www/html : httpd_sys_content_t
    - /data/mysql : mysqld_db_t

6. Investigating and resolving selinux issues

    - install setroubleshoot-server package to monitor selinux violations. It sends selinux messages to /var/log/messages
    - settroubleshoot-server listens for audit messages in /var/log/audit/audit.log and sends short summary to /var/log/messages
    - This summary includes unique identifier (UUID) for selinux violations that can be used to gather information.
    - The sealert -l UUID commnad is used ot produce a report for a specific incident
    - sealert -a /var/log/audit/audit.log to produce reports for all incidents in that file

### Commands
- change current selinux mode
    - `setenforce 0` // permissive
    - `setenforce 1` // enforcing  
    To change permanently  
    - /etc/selinux/cofig : change mode mannualy
- initial selinux context
    - -Z displays the context of a file.  
    `ls -Z /var/www/html/index.html` 
    - -Zd displays the context of a directory
- change context
    - semanage fcontext : declare the default labeling for a file
    - restorecon : apply that context to the file command
    - chcon : changes context, but it does not store the context changes in selinux context database.
    - chcon -t httpd_sys_content_t /virtual
    - restorecon -v /virtual
- semanage fcontext
    - -a, —add : add a record of specific object type
    - -d, —delete : delete a record of specific object type
    - -l, —list : list records of the specific object type
    - semanage fcontext -l
    - semange fcontext -a -t httpd_sys_content_t '/virtual'
- selinux boolean
    - are switches that change the behavior of the selinux policy
    - either enable or disable
    - getsebool : list booleans and its states
    - setsebool : modify booleans
    - setsebool -P : for persistent
    - semanage boolean -l : report whether or not a boolean is persistent, along with a short description of the boolean
    - getsebool -a // all booleans
    - getsebool httpd_enable_homedirs
    - setsebool httpd_enable_homedir on
    - semanage boolean -l | grep httpd_enable_homedir
    - setsbool -P httpd_enable_homdirs on

## Managing Basic storage
### Theory

1. __Partition__

    - divide a hard drive into multiple logical units, called partitions
    - sys admin can use diff. partitions for diff. purposes.
    - advantages
        - can limit the available space to apps and users
        - separate o.s. and prog. files from user files
        - create a separate area for memory swapping.
        - limit disk space user to improve the performance of diagnostic tools and backup-imaging.

2. __Types of partitionsi__

    - MBR : Master Boot Record Partition scheme
        - applied on sys running BIOS firmware
        - supports a max of four primary partitions
        - On Linux system, with the use of extended and logical partitions, admins can create a maximum of 15 partitions.
        - Partition size data is stored as 32 bit value, disk partitioned with MBR scheme have a **maximum disk and partition size of 2 TiB**
    - GPT :  GUID partition table
        - part of UEFI standard and addresses many of the limitations of the old MBR based scheme
        - GPT provides max of 128 partitions
        - allocates 64bits for logical block addresses
        - GPT accommodate partitions and disks of up-to eight zebibytes (ZiB) or eight Billion tebibytes
        - GPT offers redundancy of its partition table information.
    - partition editor
        - used for make changes for sys partitions
        - parted - partition editor for both MBR and GPT
        - parted command takes the device name of the whole disk as the first argument and one or more subcommands

3. __/etc/fstab fields__  
![](Untitled%205.png)  
- first field : Device name or UUID
- second field : directory mount point
- third field : file system type
- fourth field : comma separated list of options to apply to the device, defaults is a set of commonly used options
- fifth field : dump command to back up device.
- last field : fsck order filed , determines if the fsck command should be run at system boot to verify that the file system is clean

4. __Managing swap space__

    - area of disk under linux kernel subsystem
    - swap space is used to supply system RAM by holding inactive pages of memory
    - combined system ram + swap space = virtual memory
        - if memory usage > limit kernel.search(RAM) // kernel looks in the idle memory pages assigned to process in ram.  
        - kernal.write(idle_pages, swap_area) // kernel writes the idle pages to the swap area and reassigns the ram pages to other processes
        - if program→request_access_to_page_on disk kernal.write(idle_pages, swap_area) then recalls the needed page from the swap area.
        - swap area reside on disk. hence slow compared to ram. Hence swap is not a sustainable solution for insufficient RAM.

5. RAM and swap space recommendations

    - 2Gib or less : swap → twice the ram
    - 2Gib ≤ ram ≤ 8Gib : swap → same as ram
    - 8Gib ≤ ram ≤ 64Gib : swap ≥ 4Gib
    - ram ≥ 64Gib : swap ≥ 4Gib

### Commands

- parted
    - `parted /dev/vda print` : print subcommand to display the partition table on the /dev/vda disk.
    - if a sub command is not provided, then an interactive session will be started for issuing commands
   - units available in parted
        - s : sector
        - B : bytes
        - MiB, GiB or TiB : powers of two
        - MB, GB or TB : powers of ten
    - `parted /dev/vda unit s print`
    - parted /dev/vda mklabel msdos :MBR disk label
    - parted /dev/vda mklabel gpt : GPT disk label
- creating partitions : MBR
    - specify the disk device
        - parted /dev/vdb
    - use mkpart subcommand to create a new primary or extended partition
        - mkpart
    - indicate the file system type that you want to create on the partition. eg: xfs
    - specify the sector on the disk that the new partition starts on. eg: 2048s
    - specify the disk sector where the new partition should end.eg. 1000MB
        - size = End - Start
   - exit parted : quit
   - run udevadm settle
        - alternative to interactive mode :
        `parted /dev/vdb mkpart primary xfs 2048s 1000MB`
                
- creating partitions : GPT
    - specify the disk device
    - parted /dev/vdb
    - use mkpart subcommand to create a new primary or extended partition
        - mkpart
    - indicate the file system type that you want to create on the partition. eg: xfs
    - specify the sector on the disk that the new partition starts on.  
      eg: 2048s
    - specify the disk sector where the new partition should end.eg. 1000MB 
        - size = End - Start
    - exit parted : quit
    - run udevadm settle
        - alternative to interactive mode :
    `parted /dev/vdb mkpart userdata primary xfs 2048s 1000MB`
    // userdata is the name of the partition
                
- deleting partition
    - specify the disk : parted /dev/vdb
    - identify the partition number of the partition to delete : print
    - `rm <partition number/>` // delete partition
    - exit parted : quit.
- create file system : formatting a partition with a file system.
    - mkfs.xfs /dev/vdb1 : apply xfs to a block device
    - mkfs.ext4 /dev/vdb1
- mount file system
    - mount /dev/vdb1/mnt
    - persistently mounting file system on boot
        - create an entry in /etc/fstab file (white-space-delimited file with six files per line) : for UUID : use blkid command  
        ![](Untitled%206.png)
        - reload the daemon - `systemctl daemon -reload`
        - use `mount -a` to mount the system
- create a swap partition
    - parted /dev/vdb : device name
    - mkpart
    - partition name
    - file system type
    - start
    - end
    - print
    - udevadm settle   
  = formatting the device
    - mkswap /dev/vdb2
- activate and deactivate swap
    - swapon <device>
    - swapoff <device>
    - for persistent activation : create entry in /etc/fstab
    ![](Untitled%207.png)
    - swapon -a : activate all swaps
    - systemctl daemon -reload
    - free -h : display virtual memory status
 - setting priority for swap space
    - pri command for specifying priority
    - by default, swap space are used sequentially
   - use pri option in /etc/fstab [fifth field]
   - kernel uses last entry first, pri - 10, then second and finally first. default pri value = -2  
   ![](Untitled%208.png)  

## Managing Logical Volumes
### Theory

1. __LVM : logical volume management__

    - if a file system that hosts a logical volume needs more space, it can be allocated from the free space in its volume groups and the file system can be resized
    - if a disk starts to fail, replacement disk can be registered as a physical volume with the volume group and the logical volume's extents can be migrated to the new disk.

2. __LVM definitioni__

    - ==physical device== : storage device. These are block devices and could be disk partitions, whole disks etc.
    - ==physical volumes (PV)== : one must initialize a device as a physical volume before using it in LVM system.
    LVM tools segment physical volumes into Physical extents (PEs), which act as the smallest block in a physical volume.
    - ==volume group (VGs)== : storage pool made up of one or more than PVs. This is a functional equivalent of a whole disk in basic storage. A PV can only be allocated into a single GV.
    - ==Logical volume(LVs)== : created from free physical extents in a VG and provide the storage for apps, users etc.  
    LVs are collections of logical extents, which map to physical extents, the smallest chunks of a PV.  
![](Untitled%209.png)

3. Extending and reducing a volume group
    - add more disk space to a volume group by adding additional physical volume, called extending the volume group. Assign new physical extents from the additional physical volumes to logical volumes
    - remove unused physical volumes from a volume group. This is called reducing the volume group.
    - one can perform these actions while the logical volumes in volume groups are in use.

### Commands
- creating a logical volume
    - prepare the device
        - `parted -s /dev/vdb mkpart` 
        - `primary 1mib 269mib`
                
        - `parted -s /dev/vdb set 1 lvm on`
    - create a physical volume
        - `pvcreate /dev/vdb1`
    - create the volume group 
        - `vgcreate vg01 /dev/vdb1`
    - create a logical volume
        - `lvcreate -L 128M` : size exactly 128 Mib
        - `lvcreate -l 128` : size exactly 128 extents
    - add the file system
        - `mkfs -t xfs /dev/vg01/lv01`
        - `mkdir /mnt/data`
    - add an entry to the /etc/fstab file
       - `/dev/vg01/lvo1 /mnt/data xfs defaults 0 0`  
       - `mount /mnt/data`
- remove a logical volume
    - unmount lv
        - `unmount /mnt/data`
    - remove lv
        - `lvremove /dev/vg01/lv01`
    - remove vg
        - `vgremove vg01`
    - remove pv
        - `pvremove /dev/vdb1`
- reviewing LVM status information
    - **pvdisplay /dev/vdb1 :** display physical volume
    - **vgdisplay vg01** : display vg
    - **lvdisplay /dev/vg01/lv01** : display lv
- extending a volume group
    - prepare a physical device and create a physical volume
        - `parted -s /dev/vdb mkpart` 
        - `primary 1027 mib 1539 mib`
        - `parted -s /dev/vdb set 3 lvm on`
        - `pvcreate /dev/vdb3`
    - extent the volume group
        - `vgextend vg01 /dev/vdb3`
    - verify the new space is availble
        - `vgdisplay vg01`
- reducing a volume group
    - move the physical extents
        - `pvmove /dev/vdb3`  : relocate any physical extents from the physical volume you want to remove to other physical volumes in the volume groups
    - reduce the vg
        - `vgreduce vg01 /dev/vdb3`
- extending a logical volume and xfs file system
    - extending a logical volume 
        - `lvextend -L +300M /dev/vg01/lv01`
    - extending the file system
        - `xfs_growfs /mnt/data`
- extending a logical volume and ext4 file system  
    - lvextend -l +extents /dev/vgname/lvname
        - extend the file system  
        `resize2fs /dev/vg01/lv01i`
- extending a logical volume and swap space
    - verify the volume group has available space
        - `vgdisplay vgname`
    - deactivate the swap space
        - `swapoff -v /dev/vgname/lvname`
    - extend the logical volume
        - `lvextend -l +extents /dev/vgname/lvname`
    - format the logical  volume as swap space  
        - `mkswap /dev/vgname/lvname`
    - activate swap space  
        - `swapon -va /dev/vgname/lvname`
