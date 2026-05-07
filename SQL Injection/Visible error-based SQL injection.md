# Lab: Visible error-based SQL injection
![](https://img.shields.io/badge/Difficulty-PRACTITIONER-blue)

**Lab Link:** [https://portswigger.net/web-security/sql-injection/blind/lab-sql-injection-visible-error-based](https://portswigger.net/web-security/sql-injection/blind/lab-sql-injection-visible-error-based)

---
### Description
This lab contains a SQL injection vulnerability. The application uses a tracking cookie for analytics, and performs a SQL query containing the value of the submitted cookie. The results of the SQL query are not returned.

The database contains a different table called users, with columns called username and password.

To solve the lab, find a way to leak the password for the administrator user, then log in to their account. 
---

### My solution:

I started by testing if I can view the error message:
<img width="1789" height="818" alt="image" src="https://github.com/user-attachments/assets/2fdee29d-bd58-4300-97e7-18030af6f183" />

I can, so now my plan is to use the CAST function to cause an error that will leak password of the user of administrator:

`' AND CAST((SELECT password FROM users WHERE username = 'administrator') AS INT)`

But when I injected it I realized that there is a maximum character cap:
<img width="1814" height="872" alt="image" src="https://github.com/user-attachments/assets/8156c7c5-d963-4307-831b-d9043da976e2" />

I played around with the payload until i shortened it enough, I ended up with:

`'; SELECT CAST(password as int) from users limit 1--` and I encoded it in URL (CTRL+U to encode selection in URL)

This ends the original query and starts a new one that selects the password from users (limit by 1 so it returns only 1 row, which happens to be the administrator's row) and tries to convert that password into an int which results in an error being thrown that leaks the password of the administrator:
<img width="1815" height="835" alt="image" src="https://github.com/user-attachments/assets/c8e4caea-dd87-4483-aa2c-ebaae369708e" />
