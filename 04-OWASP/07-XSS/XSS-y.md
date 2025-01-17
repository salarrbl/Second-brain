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

