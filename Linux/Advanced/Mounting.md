**List devices**
<small>List block devices</small>
```shell
lsblk
```

**Firstly, umount partition**
```shell
sudo umount /dev/[partition]
```

**Secondly, eject device**
```shell
sudo eject /dev/[device]
```

**Example**
```shell
sudo umount /dev/sdc1
sudo umount /dev/sdc2
sudo eject /dev/sdc
```

**List processus using a partition**
<small>In case of - target is busy - error</small>
```shell
sudo lsof /dev/[partition]
```

