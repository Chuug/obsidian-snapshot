#curl

https://tryhackme.com/r/room/greprtp

---
#### Nmap
3 http(s) ports on 80,443 and 51337

#### Gobuster
I get the domains required in the ssl certificates
##### On 443
If I setup the `grep.thm` in `/etc/hosts`, I'm landing on the web app
##### On 51337
If I setup the `leakchecker.grep.thm` in `/etc/hosts`, I'll landing on a form page on `https://leakchecker.grep.thm:51337`

I've found the cms project [here](https://github.com/supersecuredeveloper/searchmecms/). With that, I can register with the right api key found in a previous commit on the github depot

I register with a curl post including the new api key in the header
```shell
curl -d '{"username":"chuug","password":"123","email":"a@a.a","name":"chuug"}' -H 'Content-Type: application/json' -H 'X-Thm-Api-Key: ffe60ecaa8bba2f12b43d1a4b15b8f39' -X POST https://grep.thm/api/register.php -k
```

Then I remembered the upload part on `https://grep.thm/public/html/upload.php`. On github, I saw the magic number requirement on upload so uploaded a revshell named `revshell.jpg.php` with the magic number `ffd8ffe0` and here I m in.

Luckily, I quickly found the `users.sql` file in `/var/www/backup` with the admin's email flag.

Then I went back to `https://leakchecker.grep.thm:51337/` to see if the email was leaked and it was.

