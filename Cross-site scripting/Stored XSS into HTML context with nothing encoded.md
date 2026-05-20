# Lab: Stored XSS into HTML context with nothing encoded
![](https://img.shields.io/badge/Difficulty-APPRENTICE-brightgreen)

**Lab Link:** [https://portswigger.net/web-security/cross-site-scripting/stored/lab-html-context-nothing-encoded](https://portswigger.net/web-security/cross-site-scripting/stored/lab-html-context-nothing-encoded)

---

### Description
This lab contains a stored cross-site scripting vulnerability in the comment functionality.

To solve this lab, submit a comment that calls the alert function when the blog post is viewed. 

---

### My solution:
Solved the lab by leaving a comment under a post: `<script>alert(1)</script>`
