## What is DOM-based open redirection?

DOM-based open-redirection vulnerabilities arise when a script writes attacker-controllable data into a sink that can trigger cross-domain navigation. For example, the following code is vulnerable due to the unsafe way it handles the `location.hash` property:

`let url = /https?:\/\/.+/.exec(location.hash); if (url) {   location = url[0]; }`

An attacker may be able to use this vulnerability to construct a URL that, if visited by another user, will cause a redirection to an arbitrary external domain.
## Which sinks can lead to DOM-based open-redirection vulnerabilities?

The following are some of the main sinks can lead to DOM-based open-redirection vulnerabilities:
```js
location
location.host 
location.hostname 
location.href 
location.pathname 
location.search 
location.protocol 
location.assign() 
location.replace() 
open() 
element.srcdoc 
XMLHttpRequest.open() 
XMLHttpRequest.send() 
jQuery.ajax() 
$.ajax()
```

# [Lab](https://portswigger.net/web-security/dom-based/open-redirection/lab-dom-open-redirection)
# [Lab , rootme](https://www.root-me.org/en/Challenges/Web-Server/HTTP-Open-redirect?action_solution=voir&debut_affiche_solutions=8#pagination_affiche_solutions)
- use md5 hash --> hmac
