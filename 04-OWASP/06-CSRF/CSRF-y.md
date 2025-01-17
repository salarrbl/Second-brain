# Cross Site Request Forgery

It’s called CSRF in Mitre, categorized as Broken Access Control in OWASP TOP10 2021. What is it?
 - It forces an end-user to execute unwanted actions on a web application in which they're currently authenticated
-  The action should be **state-changing**, such as<span style="color:rgb(192, 0, 0)"> update profile, change password</span>, etc
* How can attackers force a user to send HTTP request? it’s simple, we’ve learned it before
- The authentication system should work with <span style="color:rgb(192, 0, 0)">Cookies</span>, if SameSite isenabled, an XSS is needed on any subdomain to exploit the flaw
* Canany HTTP request be CSRFed? Can attackers forge any HTTP request? NO, it should be **simple** HTTP request
* The SOP still exists, although the attacker doesn’t need the response (**action has done**)

The attack scenario is shown below:
- ![[csrfy1.png]]

# The CSRF Protection

- ###### There are lots of protections to prevent CSRF. We should know all to bypass some bad implementations:
* Using anti CSRF token to prevent forgery (most common)
 -  Checking Referer header (the function which checks the Referer must be safe)
-  Using a custom header for each requests (or any action which makes the HTTP request complex, such as JSON content type) ---> not simple request

- ###### Let’s review the anti CSRF token mechanism:

-  User sends HTTP request to change password form (GET request)

 - In the response, server returns an anti CSRF token bound to the user’s Session

-  User enters new password and submits the form (POST request), the CSRF token will be sent to server

* Based on the Session, the CSRF token is checked in server side


![[04-OWASP/06-CSRF/pics/csrfy2.png]]
