# Lab: Blind SQL injection with time delays and information retrieval
![](https://img.shields.io/badge/Difficulty-PRACTITIONER-blue)

**Lab Link:** [https://portswigger.net/web-security/sql-injection/blind/lab-time-delays-info-retrieval](https://portswigger.net/web-security/sql-injection/blind/lab-time-delays-info-retrieval)

---

### Description
This lab contains a blind SQL injection vulnerability. The application uses a tracking cookie for analytics, and performs a SQL query containing the value of the submitted cookie.

The results of the SQL query are not returned, and the application does not respond any differently based on whether the query returns any rows or causes an error. However, since the query is executed synchronously, it is possible to trigger conditional time delays to infer information.

The database contains a different table called users, with columns called username and password. You need to exploit the blind SQL injection vulnerability to find out the password of the administrator user.

To solve the lab, log in as the administrator user. 
---

### My solution:

After intercepting the request and sending it to repeater, I injected `'||pg_sleep(10)--` to see if the injection works.

It worked, so now I will use this payload: `'; SELECT CASE WHEN (LENGTH((SELECT password FROM users WHERE username = 'administrator')) > 10) THEN pg_sleep(10) ELSE pg_sleep(0) END--` (encoded in URL before injection) to find the length of the password of the administrator user.

It took 10 seconds to get the response back, so the length if more than 10, going back and forth testing different values, I ended up finding that the length was `20`

Now, I will find each character one by one using intruder.
<img width="1721" height="941" alt="image" src="https://github.com/user-attachments/assets/a5a749ec-d1e3-4109-ba97-328b46fc32e8" />
`'; SELECT CASE WHEN (SUBSTR((SELECT password FROM users WHERE username = 'administrator'), 1, 1) = '§a§') THEN pg_sleep(1000) ELSE pg_sleep(0) END--`  
<br>
I set it to sleep for 1000 seconds if the character matches, so that I can see where it paused when I go back.

Now I will manually run this attack for all 20 characters.

This is what it looks like when a match occurs:
<img width="1614" height="864" alt="image" src="https://github.com/user-attachments/assets/78178674-5e7a-4594-920a-d391d7f29ab9" />

And when we get the response:
<img width="1856" height="118" alt="image" src="https://github.com/user-attachments/assets/0cee7dc8-903c-4a33-9052-1054772472fb" />

It does not actually wait for a 1000 seconds though, so I think there is a system in place to stop that from happening, but it still waits a significant amount of time enough for me to identify a match. I wanted it to be more accurate though so I set it to 100 seconds instead.

Password: `qzh4js0hqdv73h6s6pgf`
