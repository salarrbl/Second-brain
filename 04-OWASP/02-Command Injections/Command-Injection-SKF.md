# Lab 1
- we in lab resizing a image
- ![[skf-lab1-1.png]]
- let's go see packet
- ![[skf-lab1-2.png]]
```http
POST /home HTTP/1.1
Host: 127.0.0.1:5000
User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:134.0) Gecko/20100101 Firefox/134.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate, br
Content-Type: application/x-www-form-urlencoded
Content-Length: 7
Origin: http://127.0.0.1:5000
Connection: keep-alive
Referer: http://127.0.0.1:5000/home
Cookie: wordpress_logged_in_5c016e8f0f95f039102cbe8366c5c7f3=salar%7C1738426858%7C9XvMuDG4HIWntaNkPq99mJMyTQK9oxrLcPyn7vEDKyO%7C144626f650db871bb0c719b4da10d34d881c4c3a0dc6e3d346e08b12e3804d45; wp-settings-time-1=1737215777; wp-settings-1=mfold%3Do
Upgrade-Insecure-Requests: 1
Sec-Fetch-Dest: document
Sec-Fetch-Mode: navigate
Sec-Fetch-Site: same-origin
Sec-Fetch-User: ?1
Priority: u=0, i

size=50
```
- The input we give to the application here is  `size`
- We assume that the application uses the `convert` command.
- `convert path -resize `
- we use 