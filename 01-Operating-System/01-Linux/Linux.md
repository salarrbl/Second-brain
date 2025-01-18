- Linux is an operating system
- Open source and community-developed
- For computer, server, mobile device
#### shells
- bash
- zsh
- etc
- ##### Customizing
	- /home/user/[.bashrc](#shells)  
	- /home/user/[.zshrc](#shells)
	- /etc/bash.bashrc 
		- when costuming this file Includes for **all users.**


#### Package Manager
- apt 
	- Debian bases
- yum
	- red hat base (but every one)
- pacman 
	- arch bases
		- yay 
		- paru
			- AUR Package 
- others


### Linux gives everything an ID.
 - inode
	 - file id
	 - ls -il & stat
		 - can see inode
	 -  show indoe 
 -  UID
	-   user id
 - GID
	-  group id
 - PID
	-  process id


### partitions in Linux
- mount point
    - /
        - /dev/nvme0n1p2    
    - /boot
        - /dev/nvme0n1p1
        - A directory in the logical structure that is mapped to a partition in the physical structure is called a mount point.
- #### partitioning Methods
	- MBR
	    - Master Boot Record ----> 512B
	    - maximum partitions
		    -  4 primary
            
- GPT
    - GUID Partition Table

- #### File System
	- -native
	    - ext2,ext3,ext4,btrfs,reiserf
	        - ext2
	        - ext3
	        - ext4
	        - jorurnal
	        - brtfs
	        - reiserfs
- non-native
    - with module
        - XFS,ZFS
    - without module
        - APFS,NTFS
- ccross-platfrom
    - FAT12,FAT16,FAT32,iso9660,CDFS(joliet)
- #### Format 
- `mkfs`
	- `mkfs /dev/sda1`
	- `mkfs -t exe4 /dev/sda1`
	- `mkfs -L "lable name"`
- #### Partitioning 
	- `fdisk`
		- `q`
			- quit and not save
		- w
			- write
		- p
			- list partitions
		- n 
			- add new partition
- #### Mounting
	- `mount`
	- `cd /mnt`
	- `mkdir name `
	- `mount /dev/sda1 -t ext4 /mnt/name`
	- `mount a`
	
### Directory and Important files
/
    -  Root file system or dirctory
- /boot
    
    - StatrUp files
        - kernel    
        - initRB
        - bootloder file an config
- /root
    - home dirctory root
    - kernel
- /home
    - home dirctory other users
- /bin
    - Abbreviation binary
    - exec file command verry one       
        - users commnads or general commnads
- /sbin
    - Abbreviation system binary
    - exec file system commnad just root
- /lib
    - Abbreviation library  
        - library system
        - kernel modules
- /opt    
    - optional
        - thrid party applicatons
- /tmp
    - temporary
        - It is a temporary place for everyone to use.
        - reboot linux atumatic clean files in dirctory
- /etc
    - etcetera       
        - host configurations files
- /dev
    - device
        - To the components of each device a file
- /mnt and /media and /run/media
	-  prepheral device
- /var
    - variable
        - Programming is irrelevant.
        - files service
            - log server
            - print server
            - web server
	            - [[Web-Server]]  Log /var/log/httpd
            - mail server
            - cache server
            - client server
            - a few application ceches

- /usr    
    - users
    - users for system
        - non-essential executable program
        - in directory sakhtary be shbeh be / dard '
- /porc
- /sys
    - log for kernel
- /srv in  arch
    - HTTP server
    - ftp 
- /dev/null 
	- سیاه چاله

#### files
- /etc/shadow
- /etc/group
- /etc/resolve.conf
- /etc/passwd
- /etc/hosts
- /home/user/.ssh

#### File Types
- regular file
    - txt
    - mp 3
    - jpg
- directory
- symbolic link
- socket
- pipe
- special device, character
- special device , block

#### links
- ##### symbolic link
	- `ln -s file-path  link-name`
- ##### hard link
	- `ln file-path link-name`
 
## Commands
### Base Commands
```shell
ls, ls -la    --> list files
cp ---> cpoy
mv ---> move and rename
cd    --> change working dirctory
clear --> claer the terminal
cat   --> show context file
pwd   --> show working dirctor
less
nl    --> number lins
mkdir, mkdir -p --> create a dirctory
touch   --> create empty file
rm -rf --> remove file and dirctroy
rmdir
echo $SHELL --> print To *
- tee
    - print to display and file    
- sed
    - change stdout and into file   
- head
    - show 10 line frist  
    - head -n 2 file
- tail
    - show 5 line last  
- grep
    - filtering in string  
    - grep -i salar filor *
- find
    - find file  
    - find path -name "salar" 2> /dev/null
- seq
- cut
    - cut -d: -f2 /etc/passwd   
- xargs
    - seq 1 10 | 
tree --> three mode show directorys

```


#### adding & delete a user
```shell
useradd rebel
passwd  rebel
userdel rebel
```
- need root pervilage.
#### Command Cool
```bash
fastfetch ---> show system informations
uname
```
#### Command search address
```shell
whereis command
which command

```

### Variables
- ?
	- status for last exciting command 
		- 0
			- true or successful
		- more number
			- <span style="color: red;">Error</span>
- PATH
	- show address for commands
- USER
	- Current 
- UID
- HOME
	- show user home directory
- SHELL
	- uses shell
- PWD
- OLDPWD
	- last pwd
	- ##### how uses 
		- `echo $var`
		- `echo $OLDPWD`
	
#### how see manual page Commands
```shell
man ls
info
ls --help
ls  -h
```
#### what is alias?
- nickname for command
 - ##### example 
```shell
alias ls='ls -l'
alias rebel='bash /bin/myknowlge'
alias ins='sudo pacman -S '
```
- whenadding this alias in the .bashrc[[Linux#shells]] more rc file you can just write ins tree and sudo pacman -S tree running
#### wildcards
      *
        - everythink
        - zero or more charcters
    ?
        - any single charter 
	[charters] 
        - specifiles a range
    - [!charcters]
        - specifiles range. if you did m[!auc]m it can not become; mam, mom , mum
    - [a-z]
        - a charcter from a to z
    - [!a-z]
        - a charcter not from a to z
    - {frag1, frag2, frag3,...}
        - an string curly brackets
#### find command
- `find  / filename`
- #### switch's
	- -name 
	- -user
	- -group
	- perm
	- -size
	- -type
		- `find / -type d -name "salar"`
	-  -lname
		-  symbolic link
	- -exec
		- find ./ -name "s.sh" -exec rm {} +
	- not see error
		- find / -name "rebel" 2> /dev/null
			- 2>
				- say error output`s
 

### Permissions

# How see perms?
```shell
ls -l 
```
```shell
rebel ~ → ls -l
drwxr-xr-x - rebel  5 Jan 12:06  Codes
drwxr-xr-x - rebel  4 Jan 01:49  Desktop
drwxr-xr-x - rebel  4 Jan 01:24  Documents
drwxr-xr-x - rebel  5 Jan 00:35  Downloads
drwxr-xr-x - rebel 17 Dec  2024  Games
drwxr-xr-x - rebel  3 Oct  2024  go
drwxr-xr-x - rebel  9 Dec  2024  Music
drwxr-xr-x - rebel 24 Dec  2024  Pictures
drwxr-xr-x - rebel 29 Nov  2024  powerlevel10k
drwxr-xr-x - rebel  1 Aug  2024  Public
drwxr-xr-x - rebel 31 Oct  2024  Templates
drwxr-xr-x - rebel 27 Nov  2024  tool
drwxr-xr-x - rebel  2 Jan 12:39  Videos
drwxr-xr-x - rebel  2 Jan 18:10  'VirtualBox VMs'
drwxr-xr-x - rebel  4 Jan 19:22  wim
drwxr-xr-x - rebel 29 Sep  2024  yay
```
- ##### Columns
    - rwx
        - user
    - rwx
        - user group
    - rwx
        - others  
- r
    - read    
- w
    - write    
- x
    - execute    
- - d
    - type of 

#### How change permissions
```shell
chmod +x rebel.sh
```
- method symbolic
    - u = user g = group o - others a = all ---------- r = read w = write x = execute
        - example
            - u+x
                - rwxr--r--    
            - g+w
                - rw-rwr--
            - o+r
                - rw-rw-r-- 
            - u=x
                - --xrw-rw    
            - a+x
            - a=x
            - a-r
            - ug+x
            - g+xw
            - u+x,g+w,o-w
#### Example
```shell
chmod u+x file
chmod au-x file
chmod a-wx  fle
```
#### How change owner 
```shell
chown -R root;root filename
```


## vim

### Start vim
- vim 
- vim file name
### Insert Mode
- how go insert mode
	- i
	- a 
	- etc
-  back Command Mode
	- Esc
### Command Mode
- <span style="color: red;">You Can not Type this Mode</span>
- Movement Commands  
	-  h j k l
		- Left, Down, Up, Right
	- shift+h
		- first line monitor
	- shift+l 
		- last line monitor
	- gg
		- first line file
	- shift+g
		- last line file
	- ^
		- First of the  line
	- $ 
		- End of the line
	- 0
	- Ctrl+f
	- Ctrl+b
	- x
		- clear one character by right side
	-  X
	- s
	- S
	- #### cut , copy, paste
		- cut
			- d$
			- dw
			- dd
		- copy
			- y
			- yy
			- y$
			- y^
		-  paste
			- p
	- #### Undo, Rendo
		- u
		- ctrl+r
			- rendo
	- #### search
		- /
		- ?

### Hot Command Mode
- :w
	- save changes
	- :w path and name 
		- save as
-  :q
	- exit
- :wq  & :x ZZ
	-  save and exit
- :12
	- go line 12
- set un
	- set number
- :e path and filename
	- open file
-  :q! 
	- not save and exit
- !system command 
- .! 


### More
- #### config
	- you can config and install Plugin for vim 
```path
/home/rebel/.vimrc
```
[vim-Persian](https://vimpersian.github.io/)
[more source for learning](https://www.freecodecamp.org/news/learn-linux-vim-basic-features-19134461ab85/)
```shell
vimtutor
```

# Process's management


### [Bash](../../02-Programming/Bash/Bash.md)

