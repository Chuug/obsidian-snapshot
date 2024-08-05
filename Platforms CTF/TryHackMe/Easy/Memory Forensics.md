#volatility

https://tryhackme.com/r/room/memoryforensics

---
### Task 2 - Login
I used the following pluging to dump hashes
```shell
python3 vol.py [.vmem] windows.hashdump.Hashdump
```

![[Screenshot_2024-06-19_14-26-35.png]]

I took the john's `nthash`  : `47fbd6536d7868c873d5ea455f2fc0c9`

```shell
hashcat -a 0 -m 1000 -O hash /usr/share/wordlists/rockyou.txt
```

John's password is `charmander999`

### Task 3 - Analysis
Got it with my assistant for the last shutdown
```shell
python3 vol.py -f ~/Documents/Lab/Snapshot19_1609159453792.vmem windows.registry.printkey --key "ControlSet001\\  
Control\\Windows"
```

For John's writing, I had to list every file in memoy finishing by `.txt`
```shell
./vol.py -f [.vmem] windows.filescan | grep .txt
```

It gave me `\test.txt` on the **physical address** `0x2f35f430`
![[Screenshot_2024-06-19_15-33-59.png]]

Then I dumped the `test.txt`
```shell
./vol.py -f [.vmem] -o ~/Documents/Lab windows.dumpfiles --physaddr 0x2f35f430
```

---

Another way was to dump the `cmd.exe` process and grep `THM` on a `strings` dump

List the process
```shell
./vol.py -f [.vmem] windows.psscan
```

The PID was `1920`, with that I can dump the process
```shell
./vol.py -f [.vmem] -o ~/Documents/Lab windows.memmap --dump --pid 1920
```

Then grep `THM`
```shell
strings pid.1920.dmp | grep THM
```

![[Screenshot_2024-06-19_16-05-28.png]]

### Task 4 - Truecrypt
For this one, I have to download the version 2.6 of `volatility` because the version 3 doesn't handle truecrypt anymore because of deprecated

So for getting the profile needed for using 2.6, I did
```shell
./volatility2.6 -f [.vmem] imageinfo
```

It suggested me some profiles, I choose the first `Win7SP1x64`
Then I juste had to use `truecryptpassphrase` plugin to simple get the passphrase in plaintext
```shell
./volatility2.6 -f [.vmem] --profile=Win7SP1x64 truecryptpassphrase
```

![[Screenshot_2024-06-19_16-49-42.png]]

