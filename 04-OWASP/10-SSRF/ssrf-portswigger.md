##### Abbreviation ---> Server-side request forgery
##### what is ?
- allows an attacker to cause the server-side application to make requests to an unintended location.
- attacker can send request for internal-only server and give response(sensitive data, such as authorization credentials)
- ![[portswigger1.png]]
- #### impact
	- result in unauthorized actions or access to data within the organization.
		- this can other back-end systems that the application can communicate with.

### Common  SSRF
- #### SSRF attacks against the server
- [link](https://portswigger.net/web-security/learning-paths/ssrf-attacks/ssrf-attacks-common-ssrf-attacks/ssrf/ssrf-attacks-against-the-server)
- [part2](https://portswigger.net/web-security/learning-paths/ssrf-attacks/ssrf-attacks-common-ssrf-attacks/ssrf/ssrf-attacks-against-the-server-2p1v)
- ### [lab1](https://portswigger.net/web-security/learning-paths/ssrf-attacks/ssrf-attacks-common-ssrf-attacks/ssrf/lab-basic-ssrf-against-localhost)
	- ![[portswigger2.png]]
	- ![[p-lab1-2.png]]
	- ![[p-lab1-3.png]]
	- ![[p-lab1-4.png]]
	- ![[p-lab1-5.png]]
	- ![[p-lab1-6.png]]
	- ![[p-lab1-7.png]]

####  SSRF attacks against other back-end systems 
- [link](https://portswigger.net/web-security/learning-paths/ssrf-attacks/ssrf-attacks-common-ssrf-attacks/ssrf/ssrf-attacks-against-other-back-end-systems)
- ###### [lab2](https://portswigger.net/web-security/learning-paths/ssrf-attacks/ssrf-attacks-common-ssrf-attacks/ssrf/lab-basic-ssrf-against-backend-system#)
	- like above lab 
		- just find  internal-server (internal system) ip (by burp or ffuf ...) and delete user

###  Circumventing common SSRF defenses
- #### SSRF with blacklist-based input filters
- [link](https://portswigger.net/web-security/learning-paths/ssrf-attacks/ssrf-attacks-circumventing-defenses/ssrf/ssrf-with-blacklist-based-input-filters)
- ###### [Lab3](https://portswigger.net/web-security/learning-paths/ssrf-attacks/ssrf-attacks-circumventing-defenses/ssrf/lab-ssrf-with-blacklist-filter)
	- bypass by Irregular uppercase and lowercase characters 
	- `http://LoCalHOsT/AdMiN/DeLeTE?username=carlos`
	- `http://127.1/AdMin/DeLeTE?username=weiner`

####  Bypassing SSRF filters via open redirection
- [link](https://portswigger.net/web-security/learning-paths/ssrf-attacks/ssrf-attacks-circumventing-defenses/ssrf/bypassing-ssrf-filters-via-open-redirection)
- [write-up](https://medium.com/@cyberseccafe/ssrf-with-filter-bypass-via-open-redirection-9949b6ed8eb9) for understanding  
-

## Blind SSRF vulnerabilities
Blind SSRF vulnerabilities occur if you can cause an application to issue a back-end HTTP request to a supplied URL, but the response from the back-end request is not returned to the application's front-end response.

Blind SSRF is harder to exploit but sometimes leads to full remote code execution on the server or other back-end components.

read in the future
