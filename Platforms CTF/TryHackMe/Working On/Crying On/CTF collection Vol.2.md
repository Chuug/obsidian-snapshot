
https://tryhackme.com/r/room/ctfcollectionvol2

---
##### Easter 1
I found it in `/robots.txt`

##### Easter 2

##### Easter 3
Hidden on `/login`

##### Easter 5
`SQLmap` gave the `DesKel:cutie` credentials for the `/login` page
##### Easter 8
Replace the user-agent by
```text
Mozilla/5.0 (iPhone; CPU iPhone OS 13_1_2 like Mac OS X) AppleWebKit/605.1.15 (KHTML, like Gecko) Version/13.0.1 Mobile/15E148 Safari/604.1
```

##### Easter 9
On `/ready` before the redirection

##### Easter 10
```shell
wget --header="Referer: tryhackme.com" http://10.10.46.58
```
Or with burp, add `Referer: tryhackme.com` when intercepting the page
##### Easter 11
Replace one `select` value by `egg`
```html
<option value="egg">egg</option>
```
##### Easter 12
```js
ahem()
```
<small>In browser console</small>
##### Easter 13
On `/ready/gone.php`
##### Easter 14
Uncommented thebase 64 encoded image
##### Easter 15
G a m e O v e r
##### Easter 16
Modify on of a three forms to add fields on `/game2`
##### Easter 17
Bin -> Hex -> Ascii
##### Easter 18
```shell
curl -H "egg:Yes" http://10.10.46.58
```
##### Easter 19
On `/small`
##### Easter 20
```shell
curl 10.10.46.58 -X POST -d 'username=DesKel&password=heIsDumb'
```



