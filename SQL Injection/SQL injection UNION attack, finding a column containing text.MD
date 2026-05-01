# Lab: SQL injection UNION attack, finding a column containing text
![](https://img.shields.io/badge/Difficulty-PRACTITIONER-blue)

**Lab Link:** [https://portswigger.net/web-security/sql-injection/union-attacks/lab-find-column-containing-text](https://portswigger.net/web-security/sql-injection/union-attacks/lab-find-column-containing-text)

---

### Description
This lab contains a SQL injection vulnerability in the product category filter. The results from the query are returned in the application's response, so you can use a UNION attack to retrieve data from other tables. To construct such an attack, you first need to determine the number of columns returned by the query. You can do this using a technique you learned in a previous lab. The next step is to identify a column that is compatible with string data.

The lab will provide a random value that you need to make appear within the query results. To solve the lab, perform a SQL injection UNION attack that returns an additional row containing the value provided. This technique helps you determine which columns are compatible with string data. 
---

### My solution:

I start by determining the number of columns returned again

`/filter?category=NULL' ORDER BY 1-- ` Increment by 1 until I got an error

Got an error at 4, so the original query returns 3 columns.


The next step is to identify a column that is compatible with string data.
I tried retrieving the table name so that I could check the columns but that did not work so I had no option but to brute force it.

`/filter?category=NULL' UNION ALL SELECT NULL, 'hM7ZT6', NULL--`

I placed the string in the 1st, 2nd, and 3rd position until I found the correct position, it was the 2nd position.
