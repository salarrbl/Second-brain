## How see perms?
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
