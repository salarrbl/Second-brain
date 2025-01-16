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
	
### [Directory and Important files](./Directory-and-Important-files.md)
### [Commands](./Commands.md)
### [Permissions](./Permissions.md)
### [vim](./vim.md)
### [Bash](../../02-Programming/Bash/Bash.md)

