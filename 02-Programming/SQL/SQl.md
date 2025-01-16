## Syntax 
### give data 
- ### select
```sql
select * from table_name;
```
- retriever all columns data from  table_name
- #### where
```sql
select * from users WHERE username = voorivex;
select * from users WHERE id = 1;
```
- a conditional select data in users just -> username = voorivex
#### or
```sql
select * from users WHERE id = 1 or id = 2;
```
### Delete 
```sql
DELETE  FROM users WHERE id > 25;
DELETE from users where username = 'salar';
```
### Insert data
```sql
insert data in the tables -> INSERT INTO users (username, email, age) VALUE('salar', 'rebel@yahoo.com', 19)
```
### update data
```sql
UPDATE users SET age = 32 where username = 'salar';
```
### Union select
```sql
select username, email, age from users UNION select user, email, age from usr;
```
- to select in a one query just will different tables and two each query columns ==
## describe
- show columns one a table
```sql
describe users;
```
## show
```sql
show databases;
show tables;
```
## Information Schema
- information_schema 
- 1.is a databases.
- 2.Automatically created when MySQL is installed 
- 3.stores databases, tables, columns, and mo important tables? - SCHEMATA -> databases avaliables on to MySQL server TABLES COLUMNS ---- MySQL database stores permission and user information




## why learn sql 
- #### learning for sql injection vulnerabilities
	- #### [[owasp#SQL Injection]]
	- 