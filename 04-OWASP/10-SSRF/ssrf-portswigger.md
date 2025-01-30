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
		- just find  internal-server (internal system) ip and delete user