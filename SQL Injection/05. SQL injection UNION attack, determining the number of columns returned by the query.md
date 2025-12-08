## LAB DESCRIPTION:

<img width="909" height="494" alt="image" src="https://github.com/user-attachments/assets/e55e7a6c-be3d-455d-8a01-bb887a81086e" />

## SOLUTION

---------------------------------------------

Reference: https://portswigger.net/web-security/sql-injection

---------------------------------------------

The application lists products by category using a URL parameter `/filter?category=Toys+%26+Games` which triggers a server-side SQL query to fetch products for that category.
The `category` parameter is concatenated directly into the SQL statement without parameterization, making it vulnerable to injection with special characters like quotes and certain keywords.

<img width="1235" height="799" alt="image" src="https://github.com/user-attachments/assets/32184b0c-3076-4f46-81c8-551239e4d537" />

-----------------------------------------------

A UNION-based SQL injection combines the results of the original query with the results of an attacker-controlled query, but the column counts must match on both sides.
If the number of columns or thier data types do not align, the database returns an error instead of merging the result sets.

To avoid guessing exact data types during enumeration, `NULL`is used in the UNION part because it is generally compatible with most colujmn types.
Therefore, by injecting UNION SELECT payloads into the `category` parameter and incrementally increasing the number of `NULL` values untill the application responds without an error.

-----------------------------------------------------
**PAYLOAD 1**
- `'+OR+1=1--`

<img width="1920" height="1152" alt="image" src="https://github.com/user-attachments/assets/fff02d02-8025-40cc-9f4e-d65a84f1ef75" />

This displays all the values.

-------------------------------------------------------

**ORDER BY** clause to find the number of columns that is being used by the query.

**PAYLOAD 2**

**ORDER BY 1**
- `+ORDER+BY+1--

<img width="1344" height="704" alt="image" src="https://github.com/user-attachments/assets/29eda22c-19e5-4ced-a3b1-37e38842caad" />

**ORDER BY 2**

- `+ORDER+BY+2--
<img width="1339" height="782" alt="image" src="https://github.com/user-attachments/assets/5010d119-daed-4c26-98ea-f7e8675c00bf" />


**ORDER BY 3**

- `+ORDER+BY+3--
<img width="1336" height="784" alt="image" src="https://github.com/user-attachments/assets/1035a6a5-0714-4697-b301-748b919f1a30" />

**ORDER BY 4**

- `+ORDER+BY+4--
<img width="1347" height="507" alt="image" src="https://github.com/user-attachments/assets/2f0f4b13-1d68-4a89-bb54-433ae7872703" />

This throws an error. It means that there are only 3 columns that are being retreived in the query by the web application.

---------------------------------------
We use **UNION SELECT NULL** payload to return an additional column that contains null values.

`'+UNION+SELECT+NULL,NULL,NULL--`

<img width="1920" height="1152" alt="image" src="https://github.com/user-attachments/assets/4e510595-f1a0-414d-9356-34787db60ce6" />

