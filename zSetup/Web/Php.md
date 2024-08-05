#### Via Nginx

**Install FPM**
`sudo apt-get install libapache2-mod-fcgid php-fpm htop`

**Check FPM status and start**
`sudo service php8.2-fpm status`
`sudo service php8.2-fpm start` 

**Setup nginx file**

```nginx                                    
server {
    listen 80;
    server_name form.test;

    root /var/www/php;
    index index.php index.html index.htm;

    location / {
        try_files $uri $uri/ /index.php?$args;
    }

    location ~ \.php$ {
        include snippets/fastcgi-php.conf;
        fastcgi_pass unix:/run/php/php8.2-fpm.sock; # Adapt with version
        fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
        include fastcgi_params;
    }

    location ~ /\.ht {
        deny all;
    }
}
```

