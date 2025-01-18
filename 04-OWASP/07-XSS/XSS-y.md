# Cross-Site Scripting

- Whatis XS$? A vulnerability in which an attacker can inject malicious JavaScript code into websites. The script will be executed on user’s browser. There are two types of XSS:
	 -  Normal XSS — occurs when HTML s being parsed by browsers
	- DOM XSS — occurs when JavaScript code is executed as a result of modifying the DOM

- Each of types can be Reflected or Stored. In the reflected mode, no data is saved on the server, so the exploit should be delivered to user by a side channel. In the stored mode, the payload is saved in server and
- automatically injected to users when they visit vulnerable page.

-  ✨There is also an XSS type named Blind XSS, the blind here refers that we cannot see the results of our payload. We inject payload and hope for a vulnerability. Other users (specially moderators) open the
- payload, we will be noticed (how?)

- There are many XSS vectors, let’s introduce some popular ones:
```html
<script>alert(origin)</script>
<img src=x onerror=alert(origin)>
<svg onload=alert (origin)>
```


 Let’s discover an XSS in a WordPress’s plugin and write an exploit code to pop an alert



# XSS Exploitation

## Cross-Site Scripting Exploitation
- XSS disables the most important security feature in the browsers, what is it> SOP
- Considering this, by XSS we can perform any action on behalf of the victim | For example assume we have an XSS in a WordPress website, in another word we are 
  administrator & We don’t have
- administration interface, but we can force administrator to do anything we want.

## Add Administration Account

- In the WordPress administration panel, there is a section named Users which is for user management. In this section, we can force administrator to add new privileged user. There is a problem here, the request
- is protected against CSRF. However, we have an XSS so the exploit flow is:

* Sending an HTTP request to the Users sec(GIon to load the page
-  Searching and extracting CSRF token in response (DOM is accessible due to XSS)
* Sending an HTTP request to add administration account (CSRF token included)
-  Bingo! we will have a high privilege account by XSS

The attacking flow is:
# ![[xss1.png]]


# setup word-press 6.3.1
