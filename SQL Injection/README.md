## LAB DESCRIPTION:

<img width="924" height="463" alt="image" src="https://github.com/user-attachments/assets/b7eb0a0e-014b-4b9f-a499-fdec3fac6c77" />

## SOLUTION

---------------------------------------------

Reference: https://portswigger.net/web-security/sql-injection

---------------------------------------------

When the user interacts with the site, it is filtering by **Category** (observed through the URL) which is done with a **GET** request (observed by capturing the request in Burp Suite).

<img width="1269" height="959" alt="image" src="https://github.com/user-attachments/assets/5f2dad5e-d58a-41ba-87fd-435dcd9de15c" />

<img width="1074" height="401" alt="image" src="https://github.com/user-attachments/assets/05316aec-7ac9-4e71-81eb-66693d26b3da" />

Used a basic SQLi command "--" in the Category parameter which essentially is a comment indicator in SQL; whcih mean that the rest of the query is interpreted as a comment, effectively removing it.

<img width="1631" height="952" alt="image" src="https://github.com/user-attachments/assets/b647881e-b47d-48a5-a423-6dd20ff42e2f" />
It can be observed that now the browser displays four products instead of three.

---------------------------------------------------------
Now, to show all values use
```
/filter?category=Corporate+gifts'+OR+1=1--
```
