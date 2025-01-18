- ### Review
    - Format blocks of Data 
	- ### two type
		- #### request
			- ask resource
		- #### response
			- Carry results 
- ### Format
	- #### request
		- request line
		- Headers
	    - Body 
	    - Like this :
			```http
GET / HTTP/1.1
Host: 0.0.0.0
User-Agent: curl/8.11.1
Accept: */*
```
	- #### Response
	    - Status line
	    - Headers
	    - Body
	    ```http
			HTTP/1.1 200 OK
			Date: Sun, 05 Jan 2025 11:16:56 GMT
			Server: Apache/2.4.62 (Unix) PHP/8.3.15
			Last-Modified: Tue, 31 Dec 2024 19:21:20 GMT
			ETag: "2a2-62a95d57aa319"
			Accept-Ranges: bytes
			Content-Length: 674
			Content-Type: text/html
```
- #### HTTP Message
    
    - ##### Request line
         - Method<span style="color: red;">SP</span>Request-URL<span style="color: red;">SP</span>HTTPversion<span style="color: red;">CRLF</span>
            - Get /index HTTP/1.1  
    - ##### Response Line
        - HTTP==SP==version==SP==StatusCode==SP==ResonPharse==CRLF==
        - ##### CRLF AND SP
	        - ##### CRLF
		        - Carriage Return 
			        - (ASCCI13, HEX OD, \r)
			    - Line Feed
				    - ASCCI10 HEX OA , \n
			- ##### SP
				- Space (ASCCI32, HEX D 20)

- #### Method
	- **Telling the server what do to**
	- GET
		- Get document from the server
	- HEAD
		- GET just the header 
	- POST
		- Send data to the server
	- PUT
		- Store the body of the request the server
	- TRACE
		- trace the message through proxy server to the server
	- OPTIONS
		- request information about what HTTP methods (GET, POST, PUT, DELETE, etc.) the server supports for a specific resource.
	- DELETE
		- delete a resource  in the server
- #### Status Code
	- 100-199
		- information
	- 200-299
		- successful
	- 300-399
		- redirection
	- 400-499
		- Client Error
	- 500-599
		- <span style="color: red;">Server Error</span>