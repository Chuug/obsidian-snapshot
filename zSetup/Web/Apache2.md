
Create the directory
```shell
sudo mkdir /var/www/[project-name]
```

Set the permissions
```shell
sudo chown -R www-data:www-data /var/www/[project-name]
```

Create the configuration file
```shell
sudo nano /etc/apache2/sites-available/[project-name].conf
```

Write the configuration file
```apache2
<VirtualHost *:80>
    ServerAdmin webmaster@[project-name]
    ServerName [project-name].local
    [AliasName [host-alias]]
    DocumentRoot /var/www/[project-name]

    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
```

Setup the `/etc/hosts`
```text
127.0.0.1      [project-name].local
```

Activate the vhost
```shell
sudo a2ensite [project-name].conf
```

Restart apache2
```shell
sudo service apache2 restart
```
