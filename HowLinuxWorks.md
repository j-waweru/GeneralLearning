# How Linux Works by Brian Ward  

## Basic commands   

ls --> lists files  
ls -l --> lists in long format  
ls -F --> lists file type information
ls -a --> shows the hidden dot files
---

cat --> reads from a specified file. If not specified defaults to stdin and then outputs to stdout.  

---

cp file1 file2 --> copies from file one to two   
cp file1 dir2 --> copies file1 to dir2 with the same name    
cp file1 file2 file2 dir2 --> copies the three files to the directory   

---

mv --> much like cp but cut pastes instead of copy paste. 

---

touch --> creates a file. If file is already present it just changes the modification timestamp.  

---

rm --> deletes a file   

--- 

echo --> prints its arguments to the standard input.   

---

### Directories

Begin with the root directory /.  
. refers to the current directory.  
.. refers to the parent directory.  

cd  --> used to change into a directory. If no arguments it defaults to the home directory.  
mkdir dir1 dir2   --> used to create a directory.  
rmdir  --> will delete or remove a directory if its empty. -r is used to recursively delete files in the directory.  

### Shell Globbing 

echo *  --> will echo all the files in the directory. 
`'*'  --> will escape the glob character.  `
?  --> will only match one character.  

## Intermediate Commands 

grep --> reads from a file or stdin and prints only those that match a specified expression.  
`grep root /etc/passwd`

grep -i  --> is used for case sensitivity  
grep -v --> is used to invert  a search  

less --> is used to show output one screenful at a time. Spacebar is used to move forward, b for back and q to quit.  The /word and ?word can be used to search forward and backwards for a word respectively.  

---

diff file1 file2  --> shows the differences between two files.  
diff -u  --> shows in a different format 
file file1 is used to show the type of file based on the magic bytes in the file.   

---

head file1  --> shows the first 10 lines by default of a file. -n can be used to specify the no of lines.
tail file1  --> similar to head but shows the last ten lines.  

sort --> sorts the names in alphanumeric order. -n used for numbers and -r to reverse the order.  
tr --> used to translate or delete characters
tr a-z A-Z --> will translate every lowercase to uppercase 

---

passwd --> changes the passwd of the current user.  
chsh --> changes the shell of the current user.   

---

### Shell and Environment variables

Environment vars will be passed to all the commands that run them but shell vars wont.   

MYVAR=HELLO will create a shell var myvar   
export $MYVAR will make it an environmental variable  

$PATH --> used to show the directiories in which the shell looks for commands to run separated by a semicolon.  
Can be added by PATH=$PATH:/opt  

The above commands are not permanent. Starting a new shell will return to default. To make them permanent they will be added to the config directory of the shell.   

### Getting Help

man command --> is used to access the linux manual pages 
info command --> is a simpler alternative to the man pages

command -h or --help --> can be used to find help for a command

The /usr/share/docs can also contain docs not found in the man and info pages

## Stdin ,Stderr and Stdout

command > file  --> is used to redirect the stdout of the file and overwrites the contents of the file.
command >> file --> will append to the file instead

command1 | command2 --> To send the stdout of one file to the stdin of another

command 2> file --> will redirect the stderr to the file 

command 2>& file --> will redirect both stderr and stdout to file  

command < file  --> will redirect the stdin for the command

--- 

## Processes 

ps x --> shows all running processes
ps ax --> shows all running processes for all users
ps u --> shows more information about the processes
ps w --> shows full command names

kill pid --> sends a TERM signal to a process. Other signals that can be sent are

kill -STOP -CONT etc

^C is the same as kill with the INT signal

kill -KILL will indiscriminately force the process to stop. Use with caution.  

---

command & --> will put the job in the background and give back the shell.  
jobs --> will show the jobs in the background

## Files 

### File permissions

-rwx-rw-r --> first bit shows file type followed by three bits pertaining the User,Group,Other permissions
An s may be included in the execute bit denoting a setuid flag telling the os to run the program as that user eg root

chmod --> used to modify permissions eg 

chmod owner+permission eg chmod u+x g+w 
chmod owner-permission to remove the permission

Directories are abit different
A readable means they can list the contents
Executable means they can access the files in the directory

umask value is used to set the default permissions for new files. For permanent effect the command will have to be added to a startup file

---
### Links


ln -s target linkname --> creates a symlink ie shortcut or alias of files and dirs 
ln will create a hardlink. Avoid

---

### Compressing files

gzip filename --> compress
gunzip filename.gz --> uncompress

tar -cvf archive.tar file1 file2 file2 
tar -xvf archive.tar to extract archive

-c --> create mode
-x --> extract mode
-v --> verbose
-f --> for file name
-p --> to preserve permissions

zip and unzip for windows

> I Have created an extract file in my fish config that can extract all kinds of files

## Device Files

Devices are presented by the kernel as files and stored in the /dev directory.  

/dev/null is a device file handled by a driver. It just throws the contents away.

There are 4 types of device files: Character, block, socket and pipe. A  c,b,s,p will be shown in the ls -l.  

The listing in /dev aren't very useful instead /sys/devices is used to view more information and manage devices. 

udevadm info --query=all --name=/dev/sda : will show useful info about a device eg its sysfs path

### dd command

dd if=/dev/zero of=new_file bs=1024 count=1 : is used to copy a 1024 block from if to of file.   

*Refer to the book for more information*

### Common Conventios

/dev/sd : hard disk file
/dev/sda1 ,/dev/sda2 : for partitions of the disk

lsscsi : used to list all SCSI devices on the system

/dev/vd , /dev/xvd : virtual files eg in virtualization software

/dev/nvme : for nvme files. The nvme command is used to list them

/dev/sr : cd and dvd files

/dev/tty , /dev/pts : terminal files

---

Booting into graphical or text mode gives access to virtual consoles.

ALT-F1 takes you to /dev/tty1, ALT-F2 goes to /dev/tty2 in text mode
CTRL-ALT is used however in graphics mode

chvt 1,2,3 : To force switch to a diffent terminal

---

/dev/ttyS*, /dev/ttyUSB*, /dev/ttyACM* Serial Ports

screen command: used to connect to a serial device eg a micro controller

## Making device files

Not usually done manually but can be done using:
mknod /dev/sda1 b 8 1

Udev : The Linux kernel sends notifications to a user-space process called udevd upon detecting a new device onthe system (for example, when someone attaches a USB flash drive). This
udevd process could examine the new device’s characteristics, create a device file, and then perform any device initialization.

devtmpfs: The kernel creates device files as necessary, but it also notifies udevd that a new device is available. Upon receiving this signal, udevd does not create
the device files, but it does perform device initialization along with setting permissions and notifying other processes that new devices are available

ls -l /dev/disk/by-id : contains links to disks in the system

---
udevadm : admin program for udev. Search and explore devices and monitor uevents
udevadm monitor : used to show uevents eg run the command and then insert mouse to see what happens

---
lsusb is similar to lsscsi and lists usb device files attached to the system

## Disks and File Systems

Disks are presented as block devices with names like /dev/sda , /dev/sdb etc

Partitions are logical subdivisions of a disks denoted by a number after the device name eg /dev/sda1 , /dev/sda2
Partitions are defined in a *partition table *  

Filesytems are databases of files and directories.
Each partition can have its own filesystem

fdisk -l is used to show parition information
lsblk -l can be used to find drives
cat /sys/block/sda/sda2/... : provides more information about disks just like those in fdisk -l 

> Refer to book on how to create a partition using fdisk

--- 

mkfs -t btrfs /dev/sda : creates a fs in the device file. Can support many filesystems

---
Mounting: Attaching a file system to a running system

In order to mount a filesystem, you must know the following:

>The filesystem’s device, location, or identifier (such as a disk partition—where the actual filesystem data resides)
>The filesystem type.
>The mount point—the place in the current system’s directory hierarchy.Usually just a normal directory eg /mnt or /music

The mount command to check the status of the fs on currently mounted on the system
> mount -t ext4 /dev/sdf2 /home/extra will mount a flash to extra

To unmount a drive 

umount mountpoint 
Device name can also be used
The /mnt is a temporary mountpoint used for quick testing

Since device names change based on which the kernel sees first the UUID of the fs may need to be used
sudo blkid : used to list the uuids of attached devices

sync : used to write changes in the buffer to disk. Can resolve failed to unmount errors

### FileSystem Mount Options

-o remount,rw : remounts as readwrite

-r : readonly
-n : not to write to global mount db. Useful for readonly mounts
-t : fs type

long options

Preceeded by -o 

ro : read only
uid=1000 : treat all files as if they belong to user id 1000
exec,nonexec : whether you want executable files on disk
suid,nosuid : sets the suid functionality

mount -n -o remount / : if listing is in /etc/fstab
mount -n -o remount devicename 

---
/etc/fstab
errors=continue : used to decide what the kernel does when it encounters a mount problem
noauto - ignores mount -a 

---
df : shows file system capacity
df dir : limits to the fs in the current directory
5% of the disk is reserved to stop system from failing when fs fill up and only the superuser can use the reserved blocks

du : prints the usage of every directory 

### Checking and repairing FileSystems

Happens for many reasons eg Hardware issues but ussually powering off incorrectly
> Never run fsck on a mounted filesystem.

fsck : check the filesystem for problems
fsck mountpoint or device : fsck /mnt or fsck /dev/sda
fsck -p : automatically fixes minor issues and aborts on major issues
fsck -n : checks without modifying anything
fsck -y : answer yes to all qns

fsck -b superblocknum : to replace the corrupted superblock with the superblock num

ext4 fs dont need to be checked. Usually. 

---

### Special fs

Proc mounted on /proc for storing information on various processes identified  by process ids. Other things included are cpuinfo, meminfo
sysfs mounted on /sys
tmpfs on /run : use memory as temporary storage
squashfs : readonly compressed fs
overlay: merges directories into a composite eg containers

---

Swap : used as RAM
free : command to show ram and swap space usage

To create a swap space :

*Refer to book on page 94*

---

### LVM

*Refer to book*

## How the Kernel Boots

> The machine’s BIOS or boot firmware loads and runs a boot loader.
> The boot loader finds the kernel image on disk, loads it into memory,and starts it.
> The kernel initializes the devices and its drivers.
> The kernel mounts the root filesystem.
> The kernel starts a program called init with a process ID of 1. This point is the user space start.
> init sets the rest of the system processes in motion.
> At some point, init starts a process allowing you to log in, usually at the end or near the end of the boot sequence.

journalctl -k : To view the log info for boot processes.

### Kernel Initilization and Boot Options

> CPU inspection
> Memory inspection
> Device bus discovery
> Device discovery
> Auxiliary kernel subsystem setup (networking and the like)
> Root filesystem mount
> User space start

cat /proc/cmdline : shows the kernel parameters.
> BOOT_IMAGE=/boot/vmlinuz-7.0.0-15-generic root=UUID=b505e7b9-56be-415e-806e-731c50c312bc ro quiet splash crashkernel=2G-4G:320M,4G-32G:512M,32G-64G:1024M,64G-128G:2048M,128G-:4096M

splash is for displaying splash screen , root is for the location of the root filesystem. It can be specified as a uuid, device file or logical volume.  


### Boot Loaders 

eg Bios, Efi , Uefi. 
efibootmgr : If you get a list of boot targets, your system has UEFI. If instead you’re told that EFI variables aren’t supported, your system uses a BIOS.

Job is to load the kernel from device into memory with the kernel parameters. 
The kernel and it's parameters are usually somewhere on the rootfs.
This is where the chicken and egg problem starts.

### Boot loader tasks

> Select from multiple kernels.
> Switch between sets of kernel parameters.
> Allow the user to manually override and edit kernel image names and parameters (for example, to enter single-user mode).
> Provide support for booting other operating systems.

### Grub

Grand Unified Boot Loader

filesystem navigation that allows for easy kernel image and configuration selection.  

e : view bootloader config info

/boot : contains boot files eg vmlinuz(kernel) and initrd(initial ram filesystem)

#### Exploring Grub

c : enters the grub cli and allows one to run commands like
ls (-l) : list devices grub has found. hd0, hd1 for disks found
echo $root: where grub expects to find the kernel
ls ($root)/ : list files and dirs in that directory
set : views the current grub parameters
boot : boot with the variables above
esc : return to boot menu

#### Grub config file

/boot/grub : contains all grub related info
grub.cfg : central config file
grub-mkconfig : edit the grub config file

To edit the grub cfg : create your own config in /boot/grub/custom.cfg
grub-mkconfig -o /boot/grub/grub.cfg : to generate new grub config file. Always backup old first

#### Install grub

*Refer to book on page 131*

Secure boot : requires that the bootloader to be digitally signed , if not it wont be loaded. Linux already has signed bootloaders so no problem.
However if one creates a custom grub and is dual booting with windows, it can cause issues since windows may not run without secure boot.

Uefi supports multiple boot loaders in the efi partition. This is called **chainloading** which allows one to use a different bootloader on different partitions. 

## How User Space Starts

User space starts in roughly this order:

init process(lauched by the kernel)
Essential low-level services, such as udevd and syslogd
Network configuration
Mid- and high-level services (cron, printing, and so on)
Login prompts, GUIs, and high-level applications, such as web servers

### Init

Resides in /sbin : start and stop essential services on the system
On most distors this will be the **systemd** 
Its basically a series of scripts that run in sequence one at a time .

### Systemd

More advanced init process. It loads units(scripts for some system task ) when they are needed instead of one at a time like trad init.

> Service units   Control the service daemons found on a Unix system.
> Target units   Control other units, usually by grouping them.
> Socket units   Represent incoming network connection request locations.
> Mount units   Represent the attachment of filesystems to the system.

When boot finishes it activates a default init eg default.target

Two main systemd config files:

/lib/systemd/system : contains system unit config files. **Dont change this dir**
/etc/sytemd/system : system config dir. Can be changed.  

systemctl -p UnitPath show : systemd search path
pkg-config systemd --variable=systemdsystemunitdir : system unit dirs
pkg-config systemd --variable=systemdsystemconfdir : system config dir

#### Unit files

Have a particular format.

The [Unit] section gives some details about the unit and contains description and dependency information.
[Service] section, includes how to prepare, start, and reload the service

A **specifier** is a variable-like feature often found in unit files. Specifiers start with a percent sign (%). For example, the %n specifier is the current unit name, and the %H specifier is the current hostname.
When activating a service from a unit file with instances, systemd replaces the %I or %i specifier with the instance to create the new service name.


#### systemctl
systemctl - used to interact with services

systemctl list-units (--full for long unit names , --all for all units ) : views all active units

systemctl status servicename : shows status of a service and recent logs
journalctl --unit=unitname : view all logs for a unit 

systemctl (start , stop , restart) to manage services
systemctl reload unit : reload config file
systemctl daemon-reload  : reload all config files
systemctl list-jobs : shows jobs aka systemctl requests

**Adding jobs to systemd**

Put them in /etc/systemd/system
*refer to pages 146 to 161*

### Initial Ram File System

Stored as a cpio archive
initrd - older and uses disk images

Upon start, the kernel reads the contents of the archive into a temporary RAM filesystem (the init-ramfs), mounts it at /, and performs the user-mode handoff to the init on
the initramfs. Then, the utilities included in the initramfs allow the kernel to load the necessary driver modules for the real root filesystem. Finally, the
utilities mount the real root filesystem and start the true init.

mkinitramfs and dracut : used to create initramfs file.

## SYSTEM CONFIGURATION:LOGGING, SYSTEM TIME, BATCH JOBS, AND USERS

journald : a utility provided by sytemd for logging
journalctl : used to view log information
/var/log : contains log files for different stuff
/var/log/journal/ : where journald stores it's binary log files
journalctl_PID=1234 : view logs for process with id 1234

journalctl -s -4h : view logs for the last 4hrs
> journalctl -S 06:00:00
> journalctl -S 2020-01-14
> journalctl -S '2020-01-14 14:30:00'

journalctl -u service : shows the log files for a particular service
journalctl -b : logs for the current boot eg from when it started to when it stopped
journalctl -r : reverse the output
journalctl -p 2..5 : view logs from severity 2 to 5. 
0:emerg
1:alert
2:crit
3:err
4:warning
5:notice
6:info
7:debug

journalctl -f : view logs as they arrive

### User Management files

The /etc/passwd file : maps usernames to user id's. 
user + home directory = account
Login name :Password(x indicates stored in shadow file):User ID:Group ID:Real name:Home directory:Shell

eg juser:x:3119:1000:J. Random User:/home/juser:/bin/bash

/etc/shadow : stores user authentication information eg encrypted passwds and their expiration information.  

passwd : used to interact with the /etc/passwd file
adduser , usedel : to create and delete users

/etc/group : similar to passwd

Group name:Password:Group ID:Additional users

disk:*:6:juser,beazley

getty : displays the login prompt in the terminal

### Cron Jobs

crontab : used to manage cron jobs

Minute (0 through 59). This cron job is set for minute 15.
Hour (0 through 23). This job is set for the ninth hour and 15th hour.
Day of month (1 through 31).
Month (1 through 12).
Day of week (0 through 7). The numbers 0 and 7 are Sunday.

` * means catch all`

15 09,14 * * * /home/juser/bin/spmake

at : command runs a job only once
atq : checks if a command has been scheduled

## Process and Resource Utilization



























