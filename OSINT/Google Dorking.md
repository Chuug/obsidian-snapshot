[Google Hacking Database](https://www.exploit-db.com/google-hacking-database)

---
#### Google Hacking Database categories examples

**Footholds**
<small>Might reveal server misconfiguration</small>
```text
intitle:"index of" "nginx.log"
```

**Files containing usernames**
<small>Might reveal files with juicy informations</small>
```text
intitle:"index of" "contact.txt"
```

**Sensitives directories**
<small>Might reveal sensitive directories with sensitive files in</small>
```text
inurl:/certs/server.key
```

**Web server detection**
<small>Might reveal web server informations</small>
```text
intitle:"GlassFish Server - Server Running"
```

**Vulnerable files**
<small>Might reveal vulnerable files to exploit</small>
```text
intitle:"index of" "*.php"
```

**Vulnerable servers**
<small>Might reveal vulnerable servers to exploit</small>
```text
intext:"user name" intext:"orion code" -solarwinds.com
```

**Error messages**
<small>Might reveal error messages to exploit</small>
```text
intitle:"index of" errors.log
```



