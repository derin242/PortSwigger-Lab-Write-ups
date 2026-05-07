# Lab: Blind SQL injection with time delays.md
![](https://img.shields.io/badge/Difficulty-PRACTITIONER-blue)

**Lab Link:** [https://portswigger.net/web-security/sql-injection/blind/lab-time-delays](https://portswigger.net/web-security/sql-injection/blind/lab-time-delays)

---

### Description
This lab contains a blind SQL injection vulnerability. The application uses a tracking cookie for analytics, and performs a SQL query containing the value of the submitted cookie.

The results of the SQL query are not returned, and the application does not respond any differently based on whether the query returns any rows or causes an error. However, since the query is executed synchronously, it is possible to trigger conditional time delays to infer information.

To solve the lab, exploit the SQL injection vulnerability to cause a 10 second delay. 
---

### My solution:
I followed this cheatsheet and tried them one by one, shaping them to fit my payload, because I didn't know which database the server used.

<img width="1414" height="454" alt="image" src="https://github.com/user-attachments/assets/70323ac0-9c68-4317-abff-2cbbf84b00f6" />
<sup>Source: https://portswigger.net/web-security/sql-injection/cheat-sheet</sup>
<br><br>

I ended up with this:
```sql
'; SELECT CASE WHEN (1=1) THEN pg_sleep(10) ELSE pg_sleep(0) END-- (I encoded it in URL before injecting)
