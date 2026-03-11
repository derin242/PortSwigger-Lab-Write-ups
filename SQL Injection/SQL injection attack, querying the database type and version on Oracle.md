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

So I injected this query: `/filter?category=NULL' ORDER BY 1--`

Then I incremented `ORDER BY 1` to `ORDER BY 2` and so on until I got an error message.

I got an error on `ORDER BY 3` and this showed me that 2 columns were being returned by the original query.

So I crafted this query to solve the lab, it selects the **BANNER** column which has the information we need, and I added **NULL** as the second column because **NULL** supports almost all data types
`/filter?category=NULL' UNION ALL SELECT BANNER, NULL FROM v$version--`
