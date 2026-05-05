# Lab: Blind SQL injection with conditional errors
![](https://img.shields.io/badge/Difficulty-PRACTITIONER-blue)

**Lab Link:** [https://portswigger.net/web-security/sql-injection/blind/lab-conditional-errors](https://portswigger.net/web-security/sql-injection/blind/lab-conditional-errors)

---

### Description
This lab contains a blind SQL injection vulnerability. The application uses a tracking cookie for analytics, and performs a SQL query containing the value of the submitted cookie.

The results of the SQL query are not returned, and the application does not respond any differently based on whether the query returns any rows. If the SQL query causes an error, then the application returns a custom error message.

The database contains a different table called users, with columns called username and password. You need to exploit the blind SQL injection vulnerability to find out the password of the administrator user.

To solve the lab, log in as the administrator user. 
---

### My solution:

First, I intercepted the request made to the webpage when i refreshed the page, then I edited the cookie value to test if there was a conditional error:
<img width="1060" height="366" alt="image" src="https://github.com/user-attachments/assets/ea09ba19-8f44-43d8-9e1e-7c89d9feee8e" />

And it worked, so I played around a bit and found an injection that works and will allow me to find the password of the administrator:

`ZSQXb6IrINNDbKRT' AND (SELECT CASE WHEN (username = 'administrator' AND SUBSTRING(password, 1, 1) = 'a') THEN 1/0 ELSE 'x' END FROM users)='x` -> I tried this first but it'd give me an error no matter what, then I realized it was an oracle database

`ZSQXb6IrINNDbKRT' AND (SELECT CASE WHEN (SUBSTR(password, 1, 1) = 'a') THEN TO_CHAR(1/0) ELSE 'a' END FROM users WHERE username='administrator')='a` -> I played around with the payload until I ended up with this, which worked

(go through alphanumerical characters to find the first character, then repeat for the second and so on)

But, I also need to find the length of the password first and to do that I will inject:

`ZSQXb6IrINNDbKRT' AND (SELECT CASE WHEN (LENGTH(password)=19) THEN TO_CHAR(1/0) ELSE 'a' END FROM users WHERE username='administrator')='a`

I get an error when I set the length to 20, meaning the length of the password is 20
<img width="1002" height="404" alt="image" src="https://github.com/user-attachments/assets/a38e4abc-94ab-4651-9f4a-39a871d31d1d" />

Now, I will set up an attack on intruder to find each character.
<img width="1551" height="693" alt="image" src="https://github.com/user-attachments/assets/4e224e42-a388-416e-bb9f-e63a70ce3d81" />
<img width="310" height="260" alt="image" src="https://github.com/user-attachments/assets/7456a152-dde8-4e2a-bf02-ced75762180a" />

Now I will run this for each position (manually) and I will have all attacks run in the background, since burpsuite names the tabs automatically in numerical order I will see which tab is responsible for which position (character).
<img width="350" height="600" alt="image" src="https://github.com/user-attachments/assets/0c67cab0-eeff-4fcb-a2ca-83059222838b" />
For example, here I can see that the 3rd character is `j`:
<img width="1855" height="512" alt="image" src="https://github.com/user-attachments/assets/4ba80126-1aed-4449-bffb-a00f15adaeeb" />

And now I will piece out the password: `o9j0tprhxywh7ya91yyv`

To finish the lab, I logged in with the credentials `administrator`:`o9j0tprhxywh7ya91yyv`
