## LAB DESCRIPTION

<img width="900" height="469" alt="image" src="https://github.com/user-attachments/assets/126d50c0-5474-4664-b07a-1b60fafc00f4" />

## SOLUTION

---------------------------------------------

Reference: https://portswigger.net/web-security/sql-injection

---------------------------------------------

The Vulnerable parameter is the `category` used to filte products on the catalog page. This value is concatenated into a `WHERE` clause in a SQL query without proper parameterization.

First, determine the number of columns in the original query and which columns accept and display text.

----------------------------------------------------------------

> NOTE - Since it is either microsoft or MYQL db use **#** as comment

---------------------------------------------------------------

### Determine number of columns

`'+ORDER+BY+2#`

<img width="1686" height="649" alt="image" src="https://github.com/user-attachments/assets/45ea6933-9962-403d-a552-e91ec61395f2" />

Two columns are confirmed.

`'+ORDER+BY+3#`

<img width="1683" height="509" alt="image" src="https://github.com/user-attachments/assets/70eec6ae-8abd-4e66-8f9b-f66c349e8cc9" />

Returns an error, hence only two columns are being used.

---------------------------------------------------

#### Determine column which has text data

The query `'+UNION+SELECT+'abc','def'#` returns a normal page and both `abc` and `def` appear in the product listing; hence confirming there are 2 columns and both are text-compatible and rendered.

<img width="1688" height="656" alt="image" src="https://github.com/user-attachments/assets/a6e0c343-70d1-4a55-98e9-40e60563f932" />

---------------------------------------------------

PortSwigger's SQLi cheat sheet shows that both MySQL and Microsoft SQL Server support querying the version with the same expression:

- `SELECT @@version`

In a UNION SQL injection context, this expression is inserted into one of the text columns of the UNION query while keeping the overall column count aligned.
The front-end will then render the `@@version` output in place of a product field, exposing the databse banner directly in the response.

PAYLOAD : `'+UNION+SELECT+@@version,NULL#

<img width="1920" height="1152" alt="image" src="https://github.com/user-attachments/assets/3937fefd-25e2-4c5e-b9a7-c796ee9f5582" />


When the modified request is sent, the application returns a normal page and one of the rendered fields now contains the database version string `8.0.42-0ubuntu0.20.04.1`.
