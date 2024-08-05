#### SSH
**Generate classic ssh**
```shell
ssh-keygen -t rsa -b 4096
```

**Generate /etc/shadow password**
```shell
openssl passwd -6 -salt 'salt' 'password'
```

#### Keyboard layout

**Config layout location**
`/usr/share/X11/xkb/symbols`

**Layout location**
`/etc/default/keyboard`
`localectl`

**Refresh/set layout**
`setxkbmap be`

#### Fix virtualbox

```shell
sudo apt update
sudo apt install --reinstall linux-headers-$(uname -r) virtualbox-dkms dkms
```
