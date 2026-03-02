# Notes for the book Linux Command Line and Shell Scripting Bible.  

# Part 1

## Chapter 1

Linux contains four main parts.  

- Linux kernel  
- GNU core utils
- Apps
- GUI Desktop

And performs four main fns.  

- Hardware management
- RAM management(swap space)
- Program management
- File system management.  


The first process to be loaded on boot is the init process.  

The init system uses run levels.  

- Run level 1 loads the basic processes and one terminal for fixing stuff if it breaks.  
- Run level 3 loads the application software and networking configs.  
- Run level 5 loads the GUI.

The kernel also acts as a middle man between applications and the hardware via **drivers**.   
There are three hardware devises identified by **device files** :

1. Character
2. Block
3. Network

Character handles one chracter data at a time eg the terminal while block handles data in blocks.  
Networks use packets to send and retrieve data.  

Each device has a special file called a node that is used to communicate with it. Each node has a unique number pair that includes a major and a minor number with similar devices being grouped by the same major no.  

The kernel can support multiple file systems including *windows fs* though they have to be compiled with the kernel.  

Any disk has to be formatted with one of the supported fs and the kernel can communicate with them via the **Virtual File System** which provides an abstraction layer .  

Linus Torvalds didn't have core utils but GNU did so they combined the two to make a complete os.  

The core utils consists of three pars:  
1. Handling files
2. Manipulating text
3. Managing Processes



**The shell**   

The shell allow users to interact with the computer via a cli. eg start programs , work with the file system etc. Multiple shell commands can be grouped together into *shell scripts*.  
There are many kinds of shells but the default is usually bash. Other include zsh , fish, ash etc.  

**Desktop environments**  

The x window system(a core low level program) is used to communicate with the video card and monitor in your pc to display fancy windows and graphics on the screen.  
This however only produces a graphical display and nothing else. It cant manipulate files, lauch programs etc.  

Other window packages include wayland and MIR display server.

**KDE Desktop**

The K Desktop Environment was released in 1996 and was made to be similar to windows. It has several features.

- A desktop with icons that can be clicked to open files.  
- Botton panel - k menu(start menu) , program shortcuts, applets and the taskbar to show running applications.  

**Gnu Network Object Model Environment**  

Released in 1999 and is usually the default in many distros. It departs from the common windows look and feel.  

**The Unity desktop** 
Used in ubuntu. It was created with the aim of providing a single environment for all devices including tablets and phones.  

These environents however require alot of resources to use. Others like xfce, Fluxbox, have been developed for older pc's.  

### Linux distributions.  

A complete linux package is called a distribution. It packages all the parts above into one bundle.  
Each distro is made with a specific group in mind eg regular users, gamers, hackers etc.  

## Getting to the shell

There are several ways to get to the shell.  

- One is my taking the system out of GUI mode and into CLI mode eg the initramfs during boot up. This is done via virtual consoles. When the linux system starts, it creates five or six virtual consoles to work from (F1 to F7). 

- Graphical consoles - the linux GUI also provides terminal emulators that simulates a console terminal in the graphical window eg terminator.  

## Chapter 3 Basic bash commands

The **man** command gives access to the manual pages stored on the linux pages. eg man zsh to access the zshell manual page.  
They are displayed in sth called a **pager** that allow you to scroll to see the contents. To exit you can use the q key.  

There are also man page named section form 1-9 for different groups eg zsh(1) executables or shell commands while sudo(8) for super user commands.They are visible in upper left and right on the screen.Some entries can have more than one number eg man 1 hostname and man 7 hostname.  

There are other reference pages called **information pages** that use the info command.  
The **`--help`** command can also be used.  

### The File system

Unlike windows which uses drive letters linux uses a unified file sytem called a virtual filesystem with a single root `/`.  
The root drive aka the first hard drive will contain the virtual directory core and anything else will build from there.  
There are special directories called **mount points** that will be used as assign different storage devices.  

Here are the most common directories found in linux:  

![Linux directory](./2.png)

![Linux directory](./1.png)

**Navigating the file system**  

**cd and pwd**  

-Absolute paths - these paths start from the root `/` eg /usr/bin .
-Relative paths - access files and directories relative to the current dir. eg cd Documents.Don't begin with a slash.   
There are also two special chars used in relative paths.  
- .(single dot) - refers to the current dir.  
- ..(two dots) - refers to the parent dir.  
A sequence of double dots and slashes can be used to move more than one parent up eg cd ../../Dowloads.  

**ls**  
Used to list the contents of a directory.  
`ls -F` can be used to distinguish files from dirs.  

To display hidden files *begin with a .* the `ls -a` flag is used.  
`ls -R` can be used to recursively show subdirectories in the current directories.  

The ls command also uses wildcards eg  
- ? to represent one character  
- * to represent any number of chars. 
Using these commands is know as **file globbing**.  

Other include :
- ls -l f[ae]ll will match fall and fell.  
- ls -l f[a-k]ll will match the middle char to any characters btwn a and k
- ls -l f[!a]ll won't match fall but will match all others.  


### Handling files
Each file has a unique inode value`-i` and a timestamp of the last modification date.  

Use the **`touch`** command to create one or more files eg  
`touch file1 file2`  
Use the **'cp'** to copy files from one place to another. 
The `-i` can be used to ask for confirmation b4 overwriting.
The `-R` can be used to recursively copy folder contents.  

Wildcards  can also be used just like the ls command.  

eg  
```
cp oldfile1 newfile1
cp -i file1 ../Downloads/file2
```

**Linking files**  

Similar to shortcuts in windows. There are of two types.  
1. Symbolink link - creates a file that points to another file.  `ln -s file1 file1link` different inode
2. Hard link - creates a virtual file containing info about the original file. They're however in the same file. `ln file1 file1link` same inode  

**Renaming files**

In the linux world renaming files is called *moving files*  and is done using the **`mv`** command. It can be used for both files and directories.    
`mv file1 file1renamed`
It can also be used to change a file's location. 

`mv /home/christine/Pictures/fzll /home/christine/fall`to do all at once
inode and timestamp values do not change.  


**Deleting files**  

This is one dangerous procedure. Its usually a good idea to use the `-i` command with the rm command. 
It's done with the `rm` command. The `-f` command can be used to force removal.  

```
rm -i file1
```

### Directories

To make a directory use `mkdir`.
```
mkdir NewDir
mkdir -p NewDir/SubDir1/SubDir2 for nested dirs.
```
To remove dir `rmdir` and `rm -r`. The `-i` can also be used to prompt for deletion.  
```
rmdir NewDir only for empty dirs
rm -r NewDir for recursive deletion of files in dir and the dir itself.  
rm -f force deletion
rm -rf reculsively force deletion
```

### Viewing file contents
Cat and file can be used. 
Other include more for large files, less which is more advanced 
```
file myfile
cat myfile  -b can be used to number the lines with text in them.  
more myfile
less myfile
tail myfile - last ten lines by default tail -n 20 for more lines  -f can be used to view a file in a running process.  
head myfile - similar to tail but the top lines.  

```
## Chapter 4 More bash shell commands

### Managing processes. 

**Ps command** 
By default it shows the processes running in the terminal. 

```
ps -A shows all the running processes.  
ps -ef shows all the running in a nice format
ps -el shows long format with more information
ps --forest shows a treelike structure tracing both parent and child processes.  
```

The ps command however only shows processes at snaptshot in time.  

For real-time monitoring the `top` command is used.  

**Stopping Processes**   
In the unix world, processes communicate with each other via *signals*. A signal is a predifined message that a process can recognize and may choose to act on.  

![Signal types ](./3.png)

```
kill - sends a term signal to PID's of the target process.
kill -s HUP 1234 ->For other signals use the -s tack. 

killall -stops processes by their names instead of PID nums. 
killall http* - kill all http processes.
```

**Monitoring disk space**   
Before accessing a new media you must place it in a virtual directory. This is called **mounting**.  
`mount` - this command shows all the mounted devices on system.  
The mount command provides four pieces of information:

The device ﬁlename of the media  
The mount point in the virtual directory where the media is mounted  
The ﬁlesystem type  
The access status of the mounted media  

To manually mount a device `mount -t type(fileformat) device directory`  
To umnount use `umount directory|device`

**The df command**
Used to see how much disk space is available on an individual device.  
`df -h` shows the output in human readable form.  
```
du- shows how much space is used by the directory already cd'd into.  
 -c: Produces a grand total of all the ﬁles listed
 -h: Prints sizes in human-readable form, using K for kilobyte, M for megabyte, and G for gigabyte
 -s: Summarizes each argument
```
### Working with data files

The `sort` command sorts files as characters.  
```
sort -n sorts as numeric
sort -M sort by months eg when viewing logged timestamps

```
***grep*** - used to search for data in a file.  
`grep [options] pattern [file]`
```
grep -v inverses the search
grep -n shows the line numbers where the pattern was found
grep -c shows the count of the matches found
grep -e pattern -e pattern - to use more than one pattern

```
By default grep uses unix style regex.  

```
grep [tf] file - to scan for a match containing a t or f not both t and f
egrep - for extended posix characters
fgrep specifies mathcing patterns as a list of fixed size strings
```

**Compressing data**   
There are two main types of compression formats.  
`gzip .gz` for the gnu's compression utility
`zip .zip` similar to the windows zip file.

Gzip is the most common in the linux world.  
```
gzip - for compressing 
gzcat - for viewing compressed data
gunzip - for uncompressing data
```
**Archiving data**   
The `tar` command. It can be used to archive data in a directory and is a common method for distributing source code files in linux.  
`tar -zxvf filename.tgz` can be used to extract files above. 

## Chapter 5 : Understanding the shell

Shells usually resides in the `/bin` directory. 
The `/bin/bash` shell is usually the default for users while `/bin/sh` is the default for system scripts.  
Any shell can be started by typing the path of the shell and the exit command to exit it.  

When you start another shell from within the default, the new shell will be a child of the old shell. Not all the data from the parent is copied and subshell can also spawn other subshells.
A subshell is also created when you run a shell script.  

**Looking at process lists**     

Multiple commands can be ran on a line by separating them with a `;` between the commands.  
When the commands are put in `()` they are turned into a process list and creates a subshell to execute the commands.  
Putting commands in `{;}` can also be used to group commands.  

The command `echo $BASH-SUBSHELL` returns 0 for no subshell and 1 for subshell created.  

The `&` at the end of a command will run it in the background and frees up the cli for other use. eg sleep 10& will sleep for 10 seconds in the background.  
The `jobs` command can be used to see which processes are running in the background.  

**Co-Processing**  
The `coproc` command creates a subshell in the background and executes a command in that sub-shell.  

**Understading built in shell commands**  
An external command is a program that exists outside of a bash shell.  
When an external command is created, it *forks* a new child process and the output is displayed in both the parent and child.  
The `type command` can be used to tell whether a command is built in or not.  
Some commands however have both types.  
```
type -a command - can be used to display all
which command - only shows the external command
```
**History command**   
The `history` command keeps track of all the previous commands used. 
The `!!` can be used to reuse the last command in history list.  
The history is first stored in memory and then written into `~.bash_history` .

**Aliases**   
Allow one to create an alias name for commands for easier typing eg the one i created for zed and dvwaup.   

## Chapter 6: Using environment variables

They are of two types : global and local vars.  
Global vars- visible to the shell and its subshells.  
Local variables are only visible to the shell that creates them. 

`printenv VAR` command is used to show global environment varibles.  
`echo $VAR` can be used to view the values of the variable.  
`set` command can be used to display variables for a process both local and global.  

**Setting local variables**  
`echo $my_var && my_var="Hello world"`
> It's imperative to not leave any whitespace between the equal signs as bash will interpret them as commands.  

**Setting global variables**   
Done by the `export` key word.  
```
my_var="Hello Global"
export my_var 

```
Changing a global varible in a child doesn't affect the variable in the parent.  

**Removing environment variables**     

`unset my_var` this command can be used to remove any set variable.  

### Setting the path environment variable

The `path` environment variable denotes which directories as colon separated values the shell searches for to find a program to run a command.  
For commands that aren't in path, one must specify the absolute path to the program.  eg /opt/ghidra  

`PATH =$PATH:new dir` 

> Changes to the path and other env var are not persistent. Once you exit or reboot they are lost.  

### Making environment variables persistent

This can be done by adding a .sh file in the /etc/profile.d directory.    
It can also be done by modifying the /.zshrc file.

**Variable Arrays**   
`mytest=(one two three four)` command can be used to create an array of variables.    
`echo ${mytest[1]}` command can be used to display the contents of the array.   
`unset mytest` to unset the whole thing  

## Chapter 7: File permissions

File permissions apply to groups and individual users.   
Each user and group has a unique id associated with them.   

### The /etc/passwd file
Used to match the login name eg joew with the corresponding UID.  
The file contains the admin account which is the root at UID 0.  
There are also many system accounts used to access resources on a system. Usually UID 500 and below.  

The fields in the passwd file are:   
```
The login username
The password for the user
The numerical UID of the user account
The numerical group ID (GID) of the user account
A text description of the user account (called the comment ﬁeld)
The location of the HOME directory for the user
The default shell for the user
```
The password field contain an x. Before they contained the encrypted password but that was later changed and moved to the /etc/shadow file.  

### The /etc/shadow file

Contains one record for each user account on the system.  
```
rich:$1$.FfcK0ns$f1UgiyHQ25wrB/hykCn020:11627:0:99999:7:::

There are nine ﬁelds in each /etc/shadow ﬁ le record:
■
 The login name corresponding to the login name in the /etc/passwd ﬁ le
■
 The encrypted password
■
 The number of days since January 1, 1970, that the password was last changed
■
 The minimum number of days before the password can be changed
■
 The number of days before the password must be changed
■
 The number of days before password expiration that the user is warned to change
the password
■
 The number of days after a password expires before the account will be disabled
■
 The date (stored as the number of days since January 1, 1970) since the user
account was disabled
■
 A ﬁ eld reserved for future use
```
### Adding a new user.  

`useradd`  command is used to add new users and setup their home directories.   
The command uses defaults set in the `/etc/default/useradd` and command line values to setup a new user. 
`user -m test` is used to setup the home directory and create the basic files in /etc/skel folder in their home directory.   
The command line parameters and changes to defaults
eg create password , group ,default shell etc and be added using various flags

### Removing a user

`userdel` command is used. It only removes data in the /etc/passwd file but does not remove any files. 
`userdel -r` will remove the users home directory and the mailing directory.  

### Modifying a user

![File mods](./4.png)

The `finger` command can be used to show info about a spefic user.  

### Linux groups

Allow multiple users to share common permissions for a file, folder or devices.   

### The /etc/group file

GID's bellow 500 are for system accounts.  
The field in the file are:
```
■
 The group name
■
 The group password
■
 The GID
■
 The list of user accounts that belong to the group
```

The `groupadd` command is used to create new groups and the `usermod` is used to add users to the group. -g replaces the default group while -G appends it.   

**Modifying groups**   
The `groupmod` command is used. -g changes the GID while the -n changes the name.   

### Decoding file permissions.   

-rw-rw-r--   

The first parameter denotes:  
```
- for ﬁles
d for directories
l for links
c for character devices
b for block devices
n for network devices
```

The rest are three sets of three characters.  
r for read w for write and x for execute.  

The first set is for owner the second for group and third for all.   

The `umask` command sets the default permissions for a file.  

![FilePermissionsCode](./5.png)
### Changing permissions

Done using the chmod command   
`chmod options mode file`
You can specify the octal values or use the   
chmod u+x file- to add or remove files  
u -user  
g-group  
o-all  
and + or - to add or remove a permission and finally the filename.  

### Changing ownership
The `chown owner filename` command can be used.  
`chown owner.group filename` can be used to change bot the owner and group of a file.  

The `chgrp` can be used to change the group owner

### Sharing files

Can simply be done using groups but for more complex file sharing the set group id SGID bit.  
By setting this bit, all new files created in a shared directory are owned by the directory group and the user's group.  
This bit can be set using the chmod command in the front of the three digit octal values.   

![setbits](./6.png)

## Chapter 8 Managing File Systems

`fdisk` command is used to organize partitions on any storage device on a system. 

## Chapter 9 : Installing software packages.  

Each distro comes with a Package Management Utility(PMS). Software packages are stored on servers called repositories. The PMS database stores installes packages,  
files in each package an versions of packages.  
The PMS also handles dependencies aka ather packages to be installed for the software to run properly.   

## Chapter 10: Text Editors

/searchterm to search for a command in vim and enter to move through the words  
:/first,lasts/old/new/g to replace a work in specified lines  
:/s/old/new to replace in a line  
:/%s/old/new to replace in whole documents.  


# Part 2

## Chapter 11 : Bash script building

Multiple commands can be run on the same line by separating them with a semicolon. eg date;who. Multiple commands can be chained together until 255 chars.     

To start a script one starts by putting the shebang line `#!/bin/bash`      
You can add any commands in the shell and they'll be processed in the order they are found separated by carriage return or semi colons.     

```
#!/bin/bash
date
who
```
The `echo` command can be used to dispay messages to the user eg `echo this is a test`. Note there are no poundsigns or brackers like in print statements.     
In the case of using different kinds of quotes, use one in the outer and one in the inner like in python.  
`echo "Truth'nt"`   

The `echo -n` command will output the text on the same line as the command.    

### Using Variables

Environment variables can be displayed by the use of the dollar sign.  
`echo $PATH`

A dollar sign is always interpreted as a varible. To display the dollar sign it must be escaped with a `\`. eg `echo "I have \$15 dollars"`


**User defined variables**    

A varible name can contain alphanumeric digits and underscores and values can be assigned using `=` just like in a programming language.    
`MyVar=1`    
Make sure not to leave trailing whitespaces around the equals sign.   

They can be referenced by use of the dollar sign.  
`echo $MyVar`   


### Command Substitution     

Used to assign the value of a command to a variable.  
This can be done by using backticks ` ` or the $() format.  
```
date_var=$(date)
echo The date is: $date_var

```

Command substitution creates a subshell to run the command with the shell not the script as the parent so it won't have access to the scripts variables.     

### Output redirection

The `>` symbol can be used to redirect output of a command to a file eg `command > file`. This ovewrites the content of the files.  
`>>` will append to the end of the file.   

### Input redirection
Done using the `<` symbol. `command <file`    
**Inline input redirection** is done using the `<<` command. This will allow you to enter data in a sub prompt sandwiched between two similar variables.   

### Pipes
Used to send the output of one command into another.    
eg `ls -la|grep *.png`   

### Math
Can be done using the `expr` command but that's abit clunky.  
Instead square brackets can be used.`[1+2]  sum=$[1+2]` 
Normal brackets can also be used inside the square ones eg `var1=$[1+(2+3)]`  

NB: The brackets only perform integer math. For **floating point** the **zsh** can be used or the bash calculator **bc**.    
It can be accessed by using the `bc` command and can be used in a script by piping data into it. eg `echo scale=3; 3.44/5.33|bc`    

### Exiting the script

The `exit` command allow one to specify the exit status of our scripts. This can also include variables in the script.   

## Chapter 12 Using structured commands 

### If then else statement
```
if command
    then 
        commands
    else
        commands
fi
```
Unlike in programming the if command will execute the command and checks if the exit status is 0. 
If it is it runs the then commands else it goes on with the rest of the program or theelse statement if it's included.   

An elif command can be used for nested if's    
```
if command1
then
    commands
elif command2
then
    more commands
fi
```

### Test
The test command can be used to evaluate other commands just like in a programming lang and returns an exit code of 0 if true.  
`test expression`

Another way of testing a condition without using the test command is by using`[]` brackets.  
`if [ condition ]` the space around the condition must be there. The following comparisons can be made    

Numeric   
![NumericComparisons](./7.png)

String

![StringComp](./8.png)

File
![FileComp](./9.png)
![FileComp](./10.png)


Greater and less signs must be escaped or else they'll be interpreted as shell redirect commands.   

Compund conditions can be done using.  
```
[ condition1 && condition2 ]
[ condition1 || condition2 ]

```
### Advanced if then statements

Double brackets enable the use of advanced mathematical symbols



Double square brackets allow for pattern matching eg
```
if [$HOME == r*]
    then 
```
![Math](./11.png)
![Math2](./12.png)

### Case command

```
case variable in
pattern1 | pattern2) commands1;;
pattern3) commands2;;
*) default commands;;
esac
```
You can list more than one pattern in a line separated by `|`.   
The asterisk `*` is a catch all.   

## Chapter 3 Loops

### For Loop

```
for var in list 
    do;commands
done
```
This is similar to python that iterates through all the values in the list.   

Values in list can be specified by   
**list separated by a space.**
The quotation marks can be escaped using the backslash.   

```
for char in a b c d
    do
        echo $char
done
```
The char var above will retain the value of the last variable of the for loop and can be used anywhere in the script.    

A predifined list in a variable enclosed in quotes can also be used like in python.   

**Reading from a command**   

 `for var in $(cat $file)`  
 The file if in the same directory just use the name else a full pathname.   

 Field separators in bash are a space, tab, newline.  
 Any values separated by the above can cause issues with the above for command.  
 To specify a separator the `IFS=$\n` etc can be used.  
 The following can be used to restore the old ifs value.  
 ```
 IFS.OLD=$IFS
 IFS=$'\t'
 IFS=$IFS.OLD
 ```
The ifs can include any value or more than one.   
```
IFS=:
IFS=$'\n':;"

```
**Iterating files in a directory**    
`for file in /home/joew/test/*`
`do echo $file`

You can also use a list of directories in the for loop
`for file in /home /opt`
Wildcards can also be included in the filename.   

### C like for loop

```
for(( i=1; i<=10; i++ ))
    do
        echo $i
done
```
Multiple variable can be included though only one can be used as the condition   
```
for (( a=1, b=10; a <= 10; a++, b-- ))
do
echo "$a - $b"
done
```
### The while command

Loops until the command returns an exit code of zero.   
```
while [command]
    do 
        other commands
    done
```
### The until command

Executes as long sa the exit code is non zero and stops at a zero exit code.   

```
until [commands]
do
    other commands
done
```

### Nesting loops

```
for (( a = 1; a <= 3; a++ ))
do 
    echo "Loop a is at $a"
    for (( b = 1; b <= 3; b++ ))
    do 
        echo "The loop b is at $b"
    done
done

```
### Break and continue
They rely on condtitions.   \
**Break** will exit the currently processing loop if a condition fails.   
To break out of upper but not inner use the `break n` where n is the level of loop going outwards.      

**Continue**  

Similar to break but doesn't exit the loop but skips to the next iteration.   

### Processing loop output

After the done command a command can be added to define what to do with the loops output eg  
```
done > output.txt
done | sort
```
## Chapter 14 Handling User Input

Similar to other languages bash handles parameters from $0=name of the program and $1 going forward for the other parameters.    
The `$#` contain the count of the parameters passed.  
The `!#` contains the last passed value. If none are supplied then the name of the file is present.   
The `s*` takes all the commands as a single parameter.  
The `s@` takes them as separate words in the same string that can be iterated over.  

### shifty

The shift command shifts all the parameters to the left by one value by default and the first value is discarded but the program name remains unchanged.   
You can also supply the number of times to shift.  `shift 2`

### Options

The `getopts` command is used.    

```

#!/bin/bash
echo

while getopts ab:c opt
do
case "$opt" in
    a) echo "a";;
    b) echo "b has param $OPTARG";;
    c) echo "c";;
    *) echo "NO command $opt";;
esac
done

shift $[ $OPTIND - 1 ]
echo

for param in "$@" 
do
    echo $param
done
```

### Getting user input interactively 

Done using the `read` command which is similar to pythons input method.   

```
echo "Please enter your name"
read name
echo "Your name is $name"

```
The read command doesn't break on input containing spaces.     

read -p allows for entering a prompt.    

`read -p "Enter your name" name`

The REPLY variable contains data if no variables are used in the read and if too many vars are used then the last var will contain the remainder vars.    

To specify the no of options to take the `-n` command can be used and will only accept that no of inputs and sets them to the value of the variable.    

The `-s` command is used to not diplay the input as it's being typed.   


### Reading from a file
Reading from a file reads one line at a time.   
`cat test | while read line  do ...`

## Chapter 15 Presenting data

The stdin(fd 0) uses the keyboard as the input but the `<` sign can be used to replace it's file descriptor with that of the file specified and it will be read as if it was input from the keyboard.   
stdout(fd 1) is similar but replace keyboard with monitor.    

stderr(fd 2)- errors are not handled by stdout. If you want to log error messages to a file then you can place a 2 before the `>` symbol.    

To redirect both output and errors to two separate file then `ls -la 2> log 1> log2`   
To redirect to both then `ls -la &> logall`   

### Redirection output in scripts

To output error to stderr you can redirect the echo command to the `>&2`    

To redirect all output to a file then the exec command is used.   
`exec 1>logfile`   
This will overwrite the default file descriptor for the entire script.   
Multiple exec commands can be used in the same file.   

The same command can be used to redirect the stdin.  
`echo 0< inputfile`    
### Creating your own redirection 

A process can have any other file descriptors from 3 to 8.    
This can also be done using the exec command.   
`exec 3>newfd or >> can be used to append`  

A single fd can be both used for reading and writing 'exec 4<> testfile' though it's usually a bad idea.   

### Closing file descriptors

To close a fd redirect it to a special command `exec 3>&-`

**Listing Open File Descriptors**         

lsof command lists all the open fd in the entire linux system.   
`usr/bin/lsof`     
`/usr/bin/lsof -p $$ -d 0,1,2,3,4,5,6,7,8,9 -a` can be used to filter for the current shell
### Supressing output
Can be done by redirecting to a special null file in `/dev/null`.   
eg 
```
ls -ls absent 2> /dev/null
cat /dev/null > logfile - clear the logfile
```
### Using temp files

The `/temp` directory is used for files that don't need to be kept indefinetely and are removed at boot up.   
The `mktemp` command can be used to create a file in the temp folder.   

In a script `tempfile=$(mktemp test.XXXXXX)` The mktemp automatically makes the file unique by using the template in the xxxxxx.   
`-t` will return the fullpath of the file.     
`-t` will create a temporary directory.    

### tee command

Used to redirect the data from stdin to two outputs one in stdout and another in the file specified after tee. `tee filename`      
This however overwrites the file each time. To append the `-a` command is used.

## Chapter 16 Script Control

### Handling signals

Ctrl c sends a SIGINT to the current process.   
ctrl z stops a process but it's still found in memory and will continue executing from where it left off.   

To intercept signals the `trap command signals` is used.  
In the command option you just list the commands and a space separated list of signals to trap.   

`trap -- SIGINT` can be used to remove the trap.   

### Background processes

They run without being associated with the first 3 file descriptors.    
Done by placing the & after the command.    
If you run a script that displays to the terminal they still work and are intersparsed with the rest of the commands you type.  


To **restart** a job in foreground use the `fg` and for background use `bg` commands along with the job number.

### Scheduling

-20 for highest priority and +19 for lowest priority.   
All shell processes start at 0.   
The `nice -n` command allows you to set a priority of the command as you start it.   
The `renice -n -p PID` command changes the priority of an already running process.  

Both commands only lower priority. To increase the sudo command must be used.    

### Script scheduling


`at` and `cron` commands are used.   

#### at command

The **at** command allow one to specify the time the system will run the script.   
This is accomplished by the *atd* daemon that runs in the background and is started at boot time.  

The format is `at [-f filename] time`
filename refers to the file from which to run commands from and the time can be specified in either 12 or 24 hr system and also a standard date format among others.   

Once the job finishes **the email account of the user is used and output**. This is usually a bad idea so it's better to redirect the stdout and err elsewhere.   

The `atq` commands shows pending jobs.   
The `atrm` used to remove jobs.   

#### cron command

This allow you to run the script at the same time in the format. `min hour dayofmonth month dayofweek command.`
An asterisk `*` is used in any of the format to rerun every time eg ` 15 10 * * * command` to run everyday at 10:15.  

For the week days either a value from 0 to 6 can be used or the three char value eg mon.  
For months a value from 0 to 11 can be used.   

The command must be a **full path** to the script to be run.   

**Cron directories**   
Cron has specific directories in the etc directory and one can put a script into one of those categories to be run at that time.  

### Anacron

Cron assumes 24/7 operations and if a time passed the job won't be executed.  
Anacron however will rerun missed commands asap.  
Anacron only uses files in the cron directories except hourly.  

# Part 3 :Advanced shell scripting

## Chapter 17 Creating Functions

`function name {commands}` or   
`name(){ commands }`  

The default exit status of a function the that of the last command that was run.  

**`return`** is used to exit a function with a specific status code or value less than 256 else it will wrap around.   
For robust soln then assign the output of a function to a variable.   
eg   
`result=$(myfun)`  

Parameters can be passed as.  
`fun $value1 $value2`  

Functions like script define the passed values from `$0 $1 etc`.  

### Scope

**Global** vars are global to both the script and functions.   

Variables in a function are local to that function declared by the `local` keyword.  
If a similar value is found outside the fn the shell treats them as separate vars.  

### Arrays and functions

Arrays can be directly passed. They have to be dissasembled when passing and reassembled in the function.   

```
function testit {
local newarray
newarray=(;'echo "$@"')
echo "The new array value is: ${newarray[*]}"
}
myarray=(1 2 3 4 5)
echo "The original array is ${myarray[*]}"
testit ${myarray[*]}
```
On how to return arrays from fn just google it.  

### Function Recursion

```
function factorial {
if [ $1 -eq 1 ]
then
echo 1
else
local temp=$[ $1 - 1 ]
local result='factorial $temp'
echo $[ $result * $1 ]
fi
}

```

### Creating a library

Once you create a file with fns they can be imported using the `source` command.  

eg `source ./myapis` can be included after the shebang line. If not in the currend directory then the full path must be specified.   

### Using fns on the command line

The fn can be pasted into the shell and used from there but it's gone after the shell session is over.  
For persistence then they can be added in the bashrc file or sourced from a directory.   

## Chapter 19 : Sed and gawk

### sed

Is a stream editor like vim.It can manipulate data from the command line or a txt file.   

Similar to the `%s/old/new;` in vim but without the %. 
Multiple sed commands can be paired eg `s/old/new; s/old2/new2` 
The commands can also be added to a script(.sed) and read using the `-f` flag.  

### gawk ie gnu awk

The gawk adds features by including programming like interface.  
I dont think i care, i'll stick to vim.  


## Regular Expressions

The asteris `*` used so far is used to match any line containing the specified pattern.  
`.*[]^${}\+?|()` are special characters recognized by regex and must be escaped using a backslash in the pipe key.   

`^` used to anchor to the beginning of a line of text. eg `^Books`. If used in the middle it's treated just like another character.   
`$` used to anchor to the ending of a line eg `book$`  
`.` matches any character except new lines.Tabs and spaces will be matched.  eg `.at`
`[ch]at` define character spaces to match. Matches c or h at that place eg cat hat.     
`[^ch]at` does the opposite of the above.    
`[0-9],[a-z][A-DJ-Z]` to specify ranges   

![OtherPatterns](./13.png)

`a*` Placing asterisk means the a can appear *0 or more times*.    
`[at]*` pattern doesn't have to appear.    

`a?` similar to above but it matches *0 or 1 times.*  
`a+` similar to above but matches *1 or more times*  
`{a,m}` used to define how many times a pattern appears eg `be{1,2}t`  will only match 1 or 2 ees.   
`expr1|expr2|expr3` used to specify two or more patterns. Only one needs to pass.  
`()` are used to group multiple patterns.eg `(c|a)a(b|t)` matches cab cat bab bat.   

# END
