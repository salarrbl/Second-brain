
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
 