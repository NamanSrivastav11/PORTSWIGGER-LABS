## LAB DESCRIPTION

<img width="896" height="550" alt="image" src="https://github.com/user-attachments/assets/f5a600c4-11f3-4d29-b2b2-ed28db45507c" />

## SOLUTION

---------------------------------------------

Reference: https://portswigger.net/web-security/sql-injection

---------------------------------------------

The vulnerable parameter is the product `category`, passed in the URL or request when browsing items `/filter?category=Lifestyle`.
This parameter is embedded into a `WHERE` clause in a SQL query without parameterization, so adding a quote and extra SQL allows manipulation of the query.

<img width="1198" height="329" alt="image" src="https://github.com/user-attachments/assets/ac1bb80a-483c-4d79-8962-2d611c2e688a" />

-------------------------------------------------

The check for number of columns can be done by `'+UNION+SELECT+NULL,NULL,NULL--` query.
<img width="1920" height="1152" alt="image" src="https://github.com/user-attachments/assets/a611d0cd-42db-46f1-89b9-50eace96f495" />

--------------------------------------------------

The labs provides a random string (for example "CPKPpo" ) and requires this value to appear in the query results rendered on the product page.

Use a working UNION payload with three `NULL`s ; Replace the first `NULL` with the random string ; If it errors, move the string to the second column, then the third.
The column where the payload works and shows the string is the test-compatible, visible column.

**String test in the first column**
<img width="1542" height="459" alt="image" src="https://github.com/user-attachments/assets/40b534fa-13eb-404a-8e41-94fe62c34390" />

First column is not text-compatible

**String test in the second column**
<img width="1542" height="839" alt="image" src="https://github.com/user-attachments/assets/b13832a9-e3d4-4b62-8205-0b6355357d66" />

This returns the normal page and displays the string in the product listing.
Hence, **the second column is text-complatible and reflected.**

**String test in the third column**
<img width="1540" height="441" alt="image" src="https://github.com/user-attachments/assets/b496a915-cc45-48c6-84c3-ddbfbf382ae9" />
The third column is not text-compatible as well.

