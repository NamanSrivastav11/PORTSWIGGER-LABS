## LAB DESCRIPTION

<img width="913" height="598" alt="image" src="https://github.com/user-attachments/assets/08d47c4a-e094-4315-890f-9c81805a9b22" />

## SOLUTION

---------------------------------------------

- Reference: https://portswigger.net/web-security/sql-injection
- SQL Injection Cheat sheet: https://portswigger.net/web-security/sql-injection/cheat-sheet

---------------------------------------------

The vulnerable parameter is the `category` used to filter products. This value is concatenated into a `WHERE` clause in a SQL query without parameterization, so a crafted `UNION SELECT` lets us inject our own query and its results rendered with the products.

<img width="1685" height="769" alt="image" src="https://github.com/user-attachments/assets/311fe6b9-1f4f-41f3-b934-698c134cd928" />

Here, `dual` is Oracle's dummy table used when selecting constants.
As the page loads normally and displays `abc` and `def`, the UNION (2 text columns) is confirmed.

----------------------------------------------

**Enumerate tables with `ALL_TABLES`**

On Oracle, `ALL_TABLES` lists all tables accessible to the current user.

QUERY: `'+UNION+SELECT+TABLE_NAME,NULL+FROM+ALL_TABLES--`

<img width="1920" height="1152" alt="image" src="https://github.com/user-attachments/assets/ac1c5a48-eeec-4a3a-afb4-e24489584180" />

- Column 1: `TABLE_NAME` (string)
- Column 2: `NULL` placeholder

Sending this playload in the `category` parameter causes table names to appear in the product listing.
Among them is a randomly named users table (e.g., USERS_ZYWMXU).

<img width="1231" height="479" alt="image" src="https://github.com/user-attachments/assets/725b2b78-802f-4d8e-9634-a620c005c0a6" />

--------------------------------------------

**Enumerate columns with `ALL_TAB_COLUMNS`**

To list columns of that users table, Oracle exposes `ALL_TAB_COLUMNS`.

PAYLOAD: `'+UNION+SELECT+COLUMN_NAME,NULL+FROM+ALL_TAB_COLUMNS+WHERE+TABLE_NAME='USERS_ZYWMXU'--`

<img width="1920" height="1152" alt="image" src="https://github.com/user-attachments/assets/3b500433-8882-4d78-be18-05019f7bbb98" />

<img width="1218" height="521" alt="image" src="https://github.com/user-attachments/assets/e8fd0988-a6f4-4e68-aa13-a2a5916a15f6" />

This returns each column name of `USERS_ZYWMXU`, one per row, rendered in the response.

----------------------------------------------

**Dump usernames and passwords**

With the table and credential columns identified, we can UNION-select them directly.

PAYLOAD: `'+UNION+SELECT+USERNAME_PGQQOW,PASSWORD_ZTYTHT+FROM+USERS_ZYWMXU--`

<img width="1920" height="1152" alt="image" src="https://github.com/user-attachments/assets/ed7dff99-f63f-4e8a-98f6-d9a7f437e579" />

This returns all rows from the users table, embedding username/password pairs into the product listing.
Locate the row where the username is `administrator` and note the associated password value.

<img width="1312" height="445" alt="image" src="https://github.com/user-attachments/assets/289b4aba-c1c7-49f7-9ad7-fff83f118e6a" />

--------------------------------------------------

Navigate to the login page and sign in using `administrator` and the extracted password.

<img width="1268" height="626" alt="image" src="https://github.com/user-attachments/assets/1f8ec529-6a77-47c7-94d0-128486e64ba1" />

<img width="1271" height="624" alt="image" src="https://github.com/user-attachments/assets/71413a25-3d7f-42df-afde-d83dd66e2ef7" />


A successful login as `administrator` completes the lab's objective.
