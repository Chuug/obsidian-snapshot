
https://tryhackme.com/r/room/breakoutthecage1

[Boxentriq](https://www.boxentriq.com/): cipher tool
[Guballa](https://www.guballa.de/vigenere-solver): this one broke the cipher without the `namelesstwo` key
[Pspy](https://github.com/DominicBreuker/pspy): Tool for snooping linux process

---
#### Nmap
![[Screenshot_2024-05-25_18-59-01.png]]
#### Gobuster
![[Screenshot_2024-05-25_19-16-17.png]]

I went on the ftp as `anonymous` and I've got a file with a base 64 string in it:
```text
UWFwdyBFZWtjbCAtIFB2ciBSTUtQLi4uWFpXIFZXVVIuLi4gVFRJIFhFRi4uLiBMQUEgWlJHUVJPISEhIQpTZncuIEtham5tYiB4c2kgb3d1b3dnZQpGYXouIFRtbCBma2ZyIHFnc2VpayBhZyBvcWVpYngKRWxqd3guIFhpbCBicWkgYWlrbGJ5d3FlClJzZnYuIFp3ZWwgdnZtIGltZWwgc3VtZWJ0IGxxd2RzZmsKWWVqci4gVHFlbmwgVnN3IHN2bnQgInVycXNqZXRwd2JuIGVpbnlqYW11IiB3Zi4KCkl6IGdsd3cgQSB5a2Z0ZWYuLi4uIFFqaHN2Ym91dW9leGNtdndrd3dhdGZsbHh1Z2hoYmJjbXlkaXp3bGtic2lkaXVzY3ds
```

Decoded, it's seems to be an enigma encrypted shit
```ŧext
Qapw Eekcl - Pvr RMKP...XZW VWUR... TTI XEF... LAA ZRGQRO!!!!
Sfw. Kajnmb xsi owuowge
Faz. Tml fkfr qgseik ag oqeibx
Eljwx. Xil bqi aiklbywqe
Rsfv. Zwel vvm imel sumebt lqwdsfk
Yejr. Tqenl Vsw svnt "urqsjetpwbn einyjamu" wf.

Iz glww A ykftef.... Qjhsvbouuoexcmvwkwwatfllxughhbbcmydizwlkbsidiuscwl
```

The mp3 on `/auditions` has a word hidden in it but I need a better `Spectrogram analyzer` 
![[Screenshot_2024-05-25_20-14-47.png]]
Maybe `namelesstwo` ?

The enigma shit was a `Vigenere cipher` variant `Beaufort cipher` with the key `namelesstwo` gave me:
```text
Dads Tasks - The RAGE...THE CAGE... THE MAN... THE LEGEND!!!!
One. Revamp the website
Two. Put more quotes in script
Three. Buy bee pesticide
Four. Help him with acting lessons
Five. Teach Dad what "information security" is.

In case I forget.... Mydadisghostrideraintthatcoolnocausehesonfirejokes
```

A lot of basic binaries were uninstalled but I've found one using python `sudo Baron Samedit 2` [here](https://codeload.github.com/worawit/CVE-2021-3156/zip/main). The `exploit_nss.py` was the one who made me root and happy

Once root, the flags were easy to get


