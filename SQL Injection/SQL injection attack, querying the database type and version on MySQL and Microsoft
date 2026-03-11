# Lab: SQL injection attack, querying the database type and version on Oracle
![](https://img.shields.io/badge/Difficulty-PRACTITIONER-blue)

**Lab Link:** [https://portswigger.net/web-security/sql-injection/examining-the-database/lab-querying-database-version-oracle](https://portswigger.net/web-security/sql-injection/examining-the-database/lab-querying-database-version-oracle)

---

### Description
This lab contains a SQL injection vulnerability in the product category filter. You can use a UNION attack to retrieve the results from an injected query.

### Goal
To solve the lab, display the database version string. 

---

### My solution:
First, I had to see how many columns the original select statement returned (because to use union the returned columns need to be the same amount)

So I injected this query: `/filter?category=NULL' UNION SELECT NULL-- .`

And I added 1 more `NULL` to the select statement until I got an error

I got an error at 3 `NULL`s >>> `/filter?category=NULL' UNION SELECT NULL, NULL, NULL-- .`

This showed me that the original query returned 2 columns

Finally, I solved the lab by crafting and injecting this:
`NULL' UNION SELECT NULL, @@version-- .`
(There is a dot after the double dash on all injections, because Microsoft DB and MySQL both expect a space or a control character after the two dashes)
