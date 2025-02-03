# Request 
```HTTP
GET / HTTP/1.1 
Accept: image/gif, image/x-xbitmap, image/ jpeg, image/pjpeg, */* 
Accept-Language: en-us 
Accept-Encoding: gzip, deflate 
User-Agent: Mozilla/4.0 (compatible; MSIE 5.01; Windows NT) 
Host: hypothetical.ora.com 
Connection: Keep-Alive

```

1. give resource with GET Method
2. what kind of data browser accepted 
3. which language accepted 
4. 
5. Client identifies itself
6. a
7. TCP connection open until explicitly 

# Response
```HTTP
HTTP/1.1 200 OK 
Date: Mon, 06 Dec 1999 20:54:26 GMT 
Server: Apache/1.3.6 (Unix) 
Last-Modified: Fri, 04 Oct 1996 14:06:11 GMT 
ETag: "2f5cd-964-381e1bd6" 
Accept-Ranges: bytes 
Content-length: 327 
Connection: close 
Content-type: text/html 

<title>Sample Homepage</title> <img src="/images/oreilly_mast.gif"> <h1>Welcome</h1>
```

1. what happened in the server and http version
2. Current Date
3. what kind of software the server is running.
4. most recent modification time of the document requested by the client
5. This provides the web client with a unique identifier for the server resource
6. 
7. bytes are in the entity body
8. connection will close after the server’s response
9. after all this information, a blank line and the document text follow. Figure 1 shows the transaction
# Header's
- Four type
	1. General headers 
	2. Request headers
	3. Response headers
	4. Entity headers 

- ## General Headers
	-  uses by Both Server and Client
	- `Cache-Control: directives`
	- specifies desired behavior from a caching system, as used in proxy servers
	- 