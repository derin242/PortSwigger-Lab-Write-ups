# Lab: Reflected XSS into HTML context with nothing encoded
![](https://img.shields.io/badge/Difficulty-APPRENTICE-brightgreen)

**Lab Link:** [https://portswigger.net/web-security/cross-site-scripting/reflected/lab-html-context-nothing-encoded](https://portswigger.net/web-security/cross-site-scripting/reflected/lab-html-context-nothing-encoded)

---

### Description
This lab contains a simple reflected cross-site scripting vulnerability in the search functionality.

To solve the lab, perform a cross-site scripting attack that calls the alert function. 

---

### My solution:
Solved by injecting `<script>alert(1)</script>` into the search bar.
