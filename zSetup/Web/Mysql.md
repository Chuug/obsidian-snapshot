**Connect to mysql as root**
```shell
sudo mysql
```

**Connect to mysql as user with password**
```shell
mysql -u [user] -p
```

**Change root password**
```SQL
ALTER USER 'root'@'localhost' IDENTIFIED BY '[password]';
```
##### Users and privileges ([Doc](https://www.digitalocean.com/community/tutorials/how-to-create-a-new-user-and-grant-permissions-in-mysql))

**Create user with password**
```sql
CREATE USER '[user]'@'[server]' IDENTIFIED BY '[password]';
```

**Change user password**
```sql
ALTER USER '[user]'@'[server]' IDENTIFIED BY '[new-password]';
```

**Grant all privileges to an user**
```sql
GRANT ALL PRIVILEGES ON [database].[table] TO '[user]'@'[server]';
```

**Refresh privileges**
```sql
FLUSH PRIVILEGES;
```

##### Database manipulation

**Dump database**
```shell
mysqldump -u [user] -p [database] > [file].sql
```

**Import database**
```shell
cat [file].sql | mysql -u [user] -p [database]
```
##### [Basic SQL commands](https://dev.mysql.com/doc/mysql-getting-started/en/)
