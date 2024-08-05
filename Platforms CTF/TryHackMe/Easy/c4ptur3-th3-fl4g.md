#spectrogram #steganography #steghide
### Translate, shift and decode

---
###### ==FLAG 1 :== c4n y0u c4p7u23 7h3 f149?
`can you capture the flag?`

It's leet speak

---
###### ==FLAG 2 :== 01101100 01100101 01110100 01110011 00100000 01110100 01110010 01111001 00100000 01110011 01101111 01101101 01100101 00100000 01100010 01101001 01101110 01100001 01110010 01111001 00100000 01101111 01110101 01110100 00100001
`lets try some binary out!`

It's Binary

---
###### ==FLAG 3 :==MJQXGZJTGIQGS4ZAON2XAZLSEBRW63LNN5XCA2LOEBBVIRRHOM======
`base32 is super common in CTF's`

It's base32 encoded, generally full caps lock and without 0, 1, 8 and 9 numbers
The supplementaries '=' are for being 8 multiple length

```shell
echo "stringtodecode" | base32 -d
```

---
###### ==FLAG 4 :== RWFjaCBCYXNlNjQgZGlnaXQgcmVwcmVzZW50cyBleGFjdGx5IDYgYml0cyBvZiBkYXRhLg==
`Each Base64 digit represents exactly 6 bits of data.`

It's base62 encoded, same rules as base32 for the supplementaries '='

```shell
echo "stringtodecode" | base64 -d
```

###### ==FLAG 5 :== 68 65 78 61 64 65 63 69 6d 61 6c 20 6f 72 20 62 61 73 65 31 36 3f
`hexadecimal or base16?`

It's hexadecimal

---
###### ==FLAG 6 :== Ebgngr zr 13 cynprf!
`Rotate me 13 places!`

It's Caesar cipher

```sh
echo "Ebgngr zr 13 cynprf!" | tr 'A-Za-z' 'N-ZA-Mn-za-m'
```


#### Spectrogram
Using [this website](https://audioalter.com/spectrogram) to analyze the spectrogram of the .wav

#### Steganography
```shell
steghide extract -sf [file]
```

/home/gaws/.local/bin isn't in my PATH variable
```shell
/home/gaws/.local/bin/stegoveritas [file]
```
