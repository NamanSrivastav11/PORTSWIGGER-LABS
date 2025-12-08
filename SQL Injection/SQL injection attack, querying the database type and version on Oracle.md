## LAB DESCRIPTION

<img width="909" height="467" alt="image" src="https://github.com/user-attachments/assets/95317487-9c0b-4d95-9fa8-45fe73c1adcc" />

## SOLUTION

---------------------------------------------

Reference: https://portswigger.net/web-security/sql-injection

---------------------------------------------

Unlike some other DBMSes, Oracle requires every `SELECT` to select from a table, even when you are just selecting a literal value.
For ad-hoc expressions Oracle provides the built-in dummy table `dual`, so a minimal expression looks like `SELECT 'test' FROM dual`.

To retrieve version information, Oracle exposes the dynamic performance view `v$version`, whose `BANNER` column contains the database edition and version string.

Direct query would be - `SELECT banner FROM v$version`, which returns multiple rows including the "Oracle Database ..." banner.

First confirm the number of columns and type of columns in the vulnerable query via UNION.

Intercept the request that sets the product `category` filter and send it to Repeater.

<img width="1728" height="982" alt="image" src="https://github.com/user-attachments/assets/3ccd8a98-986a-42d5-83f9-894095c3468e" />

--------------------------------

Use `ORDER BY ...` to determine the column count.

`'+ORDER+BY+2--`
<img width="1920" height="1152" alt="image" src="https://github.com/user-attachments/assets/396f55e5-7273-4175-92d9-1993e39e9dc3" />

`'+ORDER+BY+3--`
<img width="1920" height="1152" alt="image" src="https://github.com/user-attachments/assets/9174172b-118f-44be-8ffd-0f0572b77021" />

Hence, there are two columns present.

---------------------------------------------

The query `'+UNION+SELECT+'abc','def'+FROM+dual--` returns two columns and both can hold text.

<img width="1920" height="1152" alt="image" src="https://github.com/user-attachments/assets/cfc61ccb-94de-4b60-91aa-2935eb40f1c8" />

After confirming two text columns, we can pivot from the dummy `dual` table to the real version view `v$version`.

QUERY - `SELECT banner FROM v$version`

To integrate this into the vulnerbale `category` parameter using UNION, we can craft the payload as following :

- `'+UNION+SELECT+BANNER, NULL+FROM+v$version--`

<img width="1920" height="1152" alt="image" src="https://github.com/user-attachments/assets/9b4d67ac-bc33-4ce4-bf71-90098050627a" />

Now, one of the response columns now contains the `BANNER` value from `v$version`.
When the application renders the page, at least one of the returned rows will show an Oracle version banner "Oracle Database 11g ....", which solves the lab.
