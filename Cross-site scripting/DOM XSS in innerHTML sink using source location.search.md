# Lab: DOM XSS in innerHTML sink using source location.search
![](https://img.shields.io/badge/Difficulty-APPRENTICE-brightgreen)

**Lab Link:** [https://portswigger.net/web-security/cross-site-scripting/dom-based/lab-innerhtml-sink](https://portswigger.net/web-security/cross-site-scripting/dom-based/lab-innerhtml-sink)

---
### Description
This lab contains a DOM-based cross-site scripting vulnerability in the search blog functionality. It uses an innerHTML assignment, which changes the HTML contents of a div element, using data from location.search.

To solve this lab, perform a cross-site scripting attack that calls the alert function. 

---

### My solution:
I tried to inject a script tag but it did not work so i injected an img tag:  
`<br></span><img src=1 onerror='alert(1)'><span>`  
It worked, and the lab was solved.
