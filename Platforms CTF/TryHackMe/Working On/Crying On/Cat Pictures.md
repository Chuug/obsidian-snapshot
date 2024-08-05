
https://tryhackme.com/r/room/catpictures

---
#### Nmap
![[Screenshot_2024-06-18_19-03-54.png]]

Originaly, there was only 2 opened ports: 22 and 4420, so I made this script to bruteforce the 4420 login until I decided to redo a nmap scan, which gave me 5 ports
```shell
#!/bin/bash

while IFS= read -r line; do
	test=$(echo "$line" | nc 10.10.121.100 4420)
	if ! echo "$test" | grep -q "Invalid"; then
		echo "$line"
		break
	fi
done < "$1"
```
