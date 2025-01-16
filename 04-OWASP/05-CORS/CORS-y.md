## First Read this [[API-Sec]]
# Implementation    
- As we've mentioned, SOP prevents JavaScript to access cross-origin resources. There are some solutions to remove the restriction:
        
    - `postMessage` →Sending and receiving messages between two different origins
	-  `JSONP` → Using the `<script>` tag to transfer JavaScript objects
    -  Cross Origin Resource Sharing → Modifying SOP by some special response headers
    - Let's focus on the CORS, it updates SOP rules, so the DOM will be accessible even though the origins are different:
    - ![[p1cors.png]]
    - CORS headers
        - `Access-Control-Allow-Origin: <Origin> | *`
	            - which means that resource can be accessed by `<Origin>`{.highlight}
             - `✨`The Browser always put the correct origin in the request y Origin HTTP header, it cannot be spoofed or modified by java script
        - `Access-Control-Allow-Credentials: true`
            - indicates response can be exposed or not when HTTP request has Cookie
# CROSS ORIGIN HTTP REQUEST
    
- As we've learned in previous lesson, HTTP request can be issued on behalf of users browsing websites. However, some restrictions are applied by browsers:
    • Browsers only permit to send simple HTTP requests on behalf of users
    • Browsers send Pref light HTTP request if the request is not simple
    - Simple HTTP Request
        - One of the following requests methods:
            - `GET`
            - `POST`
            - `HEAD`
        - • Some headers (Original headers cannot be changed, more info)
        - • Only 3 content types:
            - `application/x-www-form-urlencoded`
            - `multipart/form-data`
            - `text/plain`
            - [More info](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS)
    - example simple request
```js
const xhr = new XMLHttpRequest(); const url = 'https://site.tld/resources/data/'; xhr.open('GET', url); xhr.onreadystatechange = handlerFunction; xhr.send();
```
 -       
    - Pref-light HTTP Request
        
        - If the cross origin HTTP request is not simple, an OPTION HTTP request will be sent which is called Pref light
        - If response headers permit the HTTP method and headers, the HTTP request will be sent.
            - `Access-Control-Max-Age: <seconds>` → value in seconds for how long the response can be cached
            - `Access-Control-Allow-Methods` → defines valid HTTP methods to access to resource
            - `Access-Control-Allow-Headers` → defines valid headers to be permitted to send
        - Let's make an example, if the following code is executed by https://memoryleaks.ir:
	    ```js
const invocation = new XMLHttpRequest();
const url = 'http://domain.tld/resources/api/me/data/';
function callOtherDomain () { if (invocation) { invocation.open('GET', url, true);
invocation.withCredentials = true;
invocation.setRequestHeader('Content-Type', 'application/json'); invocation.onreadystatechange = handlerFunction;
invocation.send(); Copy Caption 
	}
}
```
- 
	 - The following headers should be response of Preflight request:
    - `Access-Control-Allow-Origin: https://memory leaks.ir`
    - `Access-Control-Allow-Credentials: true`
    - `Access-Control-Allow-Headers: Content-Type`   
    - this flow
    - ![[p2cors.png]]