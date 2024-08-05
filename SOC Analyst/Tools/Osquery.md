[Schema documentation](https://osquery.io/schema/5.12.1)

---
**Start osquery**
```shell
osqueryi
```

**List tables**
```shell
.table
```

**List tables beginning by `user`**
```shell
.table user
```

**List table schema**
```shell
.table users
```

**Select query**
```shell
select gid, uid, description, username, directory from users; 
```

