# Lab: DOM XSS in document.write sink using source location.search
![](https://img.shields.io/badge/Difficulty-APPRENTICE-brightgreen)

**Lab Link:** [https://portswigger.net/web-security/cross-site-scripting/dom-based/lab-document-write-sink](https://portswigger.net/web-security/cross-site-scripting/dom-based/lab-document-write-sink)

---
### Description
This lab contains a DOM-based cross-site scripting vulnerability in the search query tracking functionality. It uses the JavaScript document.write function, which writes data out to the page. The document.write function is called with data from location.search, which you can control using the website URL.

To solve this lab, perform a cross-site scripting attack that calls the alert function. 
---

### My solution:
Quick testing shows me that the website injects the query into an image tag
<img width="1023" height="303" alt="image" src="https://github.com/user-attachments/assets/9975f910-0965-45cb-ad3a-df9d7e1e7bb6" />

I solved the lab by injecting `" onerror='alert(1)'` to break out of the string and inject the alert function.
