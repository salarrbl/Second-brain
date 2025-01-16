# DOM, BOM
### A web page on browsers provides object for programmers to talk with it. DOM, BOM, JavaScript ,etc, lets start with Browser Object Model(BOM)
- #### BOM
    - The BOM provides objects that expose the web browsers functionality.        
    - Let's make some example
        - popping up an alert on the browser
            - `alert`
        - Redirecting users to a web page by
            - `window.location`
        - Getting size of the screen by
            - `window.screen`
        - Getting users User-agents
            - `window.navigator.userAgent`
- #### DOM
	 -  The Document Object Model (DOM) is a programming interface for web (HTML and XML) document
	- it represents the page so that programming can change the document structure style , and content  
	- example
	    - Setting and reading Cookies for users by
	        - `documnet.cookie`
	    - Accessing to the web page object by
	        - `document.documentElement`
	    - Selecting a HTML element by
	        - `document.getElementById`
	    - redirecting users to a web page by
	        - `document.location`
- #### When a web page loads, many HTTP requests may by issued, during the recourse loading the DOM is being update
- ![[p2-api-sce.png]]
## HTTP Request in Browser
- XmlHttpRequest is built-in browser object that allows to issue HTTP request by JavaScript. fetch is a modern way to deal with request
        - this type name is **cross-origin** http request
```javascript
var xhr = new XMLHttpRequest();  
      xhr.open("GET", "http://127.0.0.1:8000");  
      xhr.onreadystatechange = function () {  
        if (xhr.readyState === 4) {  
          if (xhr.status === 200) {  
            console.log(xhr.responseText);  
    alert(chr.responseText);  
          } else {  
            console.error("Request failed wit status:", xhr.status);  
          }  
        }  
      };  
      xhr.send();
```
- 
    - this name http request
```javascript
var xhr = new XMLHttpRequest();  
      xhr.open("GET", "/class");  
      xhr.onreadystatechange = function () {  
        if (xhr.readyState === 4) {  
          if (xhr.status === 200) {  
            console.log(xhr.responseText);  
    alert(chr.responseText);  
          } else {  
            console.error("Request failed wit status:", xhr.status);  
          }  
        }  
      };  
      xhr.send();


### exmple
Iframes
The <iframe> provides browsing context, embedding another HTML page into the current one
<iframe id="myIfream" src="https://yephpwork.site" frameborder="0"></iframe>
```
### Dive in Cookie
- HTTP Cookie are small blocks of data created by a web server while a user is browsing a website and placed on the user`s computer or other device bby the user`s web browser, when visiting a website, browser automatically include Cookie belonged to the website (not always). Cookie have various attributes(Cookie`s flags), lets take a look of each briefly:
	-  `domain`    -->   to specify the domain for which the cookie is sent  
	- `path`         -->   to specify the path for which the cookie is sent  
	- `secure`     -->   to set whether the Cookie is transmitted only over an encrypted channel (connection)  
	- `httpOnly`  --> to set whether the Cookie is exposed only  through HTTP and HTTPS channels  
	- `expires`   --> to set the Cookie expiration data   
	- `SameSite` --> to prevent browser from sending Cookies along whit Cross-Site request
    - 
        - example

```php
setcookie("user", "salar", time() + (7 * 24 * 60 * 60), "/", "", false, ture); setcookie("user", "salar", time() + (7 * 24 * 60 * 60), "/", "", false, false);
```
- 
    - Cookies same Site Rules
        - None
            - the Cookies will sent along with request initiated by third-party website (cross-site website)
        - Lax
            - the Cookies will be sent along with GET request initiated by third-party website (<span style="color:rgb(192, 0, 0)">top-level navigation is needed</span>)    
                - Top-level means that the URL in he address bar changes because of this navigation. 
                - Resource loaded with<span style="color:rgb(255, 89, 0)"> img, iframe, and script tags</span> do not cause top-level navigation                    
        - Strict
            - The Cookies will not be sent along with request initiated by third-party website
    - But how to identify third-party website? any website which is not same site cross-site website (third-party)
        - The combination of(e) TLD and the domain part just before it, eTLD+1
            - For example
                - https://cdn.rebel.com:4443 , the site is rebel.com
                - The https://cdn.rebel/com:4443 and https://rebel.com are same-site , but not same-origin
                - see in action
                - #### [in this repo](https://github.com/Voorivex/same-site-poc)
# Same Origin Policy
- ### Please pay attention to the following scenario:
    -  A company has a website, authenticated users can see their profile, if they are not logged-in, the login page is loaded
    -  An employee logs in by their credentials (the Cookies are SameSite=None)
    - The employee visits memoryleaks.ir, there is a hidden iframe inside it sourced to company's website
    -  Question: are the Cookies sent along the HTTP request?
    - Question: What's the data loaded in the iframe? an authentication page or the employee's data?
    -  The memoryleaks.ir has a code to read inside iframe's content and steal the data, is it possible?
    - -Let's see in action:
    - in note
```html
<!DOCTYPE html>  
<html lang="en">  
<head>  
<meta charset="UTF-8">  
<meta name="viewport" content="width=device-width, initial-scale=1.0">  
<title>Document</title>  
</head>  
<body>  
<iframe src="https://rebel.com" frameborder="0" id="id_iframe"></iframe>  
<script>  
function read() {  
var data = document.getElementById("id_iframe").contentWindow.document.body.innerHTML  
aler(data)  
}  
</script>  
</body>  
</html>
```
- 
	- The Same Origin Policy stops reading inside iframe since the origin of the website (memoryleaks.ir) and iframe source (company.com) are not same.
	-  ![[pic2api-sec.png]]
- ## What is SOP ?
- ![[pic3api-sec.png]]
- OK thinking about those
    - Site-A includes a script from site-B by `<script>`, what's the example?
    - What is the origin of the script?
    - Seems the script is loaded successfully, where is SOP then?
- ## (CSP) Content Security Policy
	-  Content Security Policy (CSP) is an added layer of security that helps to detect and mitigate certain types of attacks, including XSS. There are two ways of enabling the CSP:
    -  Response header → Content-Security-Policy: `<policy-directive>`
	- Meta tag → <meta http-equiv="Content-Security-Policy" content="<policy-directive>">
    - Where` <policy-directive> consists of: <directive> <value>`
       -  Content-Security-Policy: img-src https://rebel.com; script-src https://www.google.com