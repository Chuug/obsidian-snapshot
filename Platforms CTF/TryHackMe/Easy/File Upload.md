[CTF Link - Task 11](https://tryhackme.com/room/uploadvulns#collapse11)

We get a wordlist, possibly for the upload's naming scheme 

We can get the framework **Express** used by intercepting the response with burp
![[Screenshot_2024-03-26_12-25-19.png]] 

We can also get additionnal information with the extension Wappalyzer: This is a fully js website (backend/frontend) using **nodejs**

CTRL + SHIFT + R = Reload without cache, useful for intercepting in burp suite

Removed filters in uploads.js

![[Screenshot_2024-03-27_11-46-13.png]]

Uploaded the nodejs shell as a jpg, the short version of the node revshell didn't work

Found the shell in /content with the wordlist and the jpg e**x**tension
`gobuster dir -u http://jewel.uploadvulns.thm/content -w ./UploadVulnsWordlist.txt -t 100 -x jpg`

Triggered the reverse shell in /admin with `../content/[3LETTERS].jpg` input