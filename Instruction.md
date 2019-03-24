# 1. Install virtual box
```
sudo apt-get update
sudo apt-get upgrade
sudo apt-get install virtualbox
```
# 2. Install Vagrant
```
sudo apt-get install vagrant
```
# 3. Start Vagrant
```
vagrant init
vagrant up
```
# 4.Install development environment

## 4.1 Install repository
```
sudo add-apt-repository -y ppa:ondrej/php
sudo apt-get update -y
```
## 4.2 Install nginx
```
sudo apt-get install -y nginx

sudo systemctl stop nginx.service
sudo systemctl start nginx.service
sudo systemctl enable nginx.service
sudo systemctl reload nginx.service
```
## 4.3 Install mysql
```
sudo apt-get install mariadb-server mariadb-client
```
### On Ubuntu 16.04 LTS
```
sudo systemctl stop mysql.service
sudo systemctl start mysql.service
sudo systemctl enable mysql.service
```
### On Ubuntu 18.04 LTS and 18.10 
```
sudo systemctl stop mariadb.service
sudo systemctl start mariadb.service
sudo systemctl enable mariadb.service

sudo mysql_secure_installation

sudo mysql -u root -p
```
## 4.4 Install PHP 7.2-FPM and Related Modules
```
sudo apt-get install software-properties-common
sudo apt update
sudo apt install php7.2-fpm php7.2-common php7.2-mbstring php7.2-xmlrpc php7.2-soap php7.2-gd php7.2-xml php7.2-intl php7.2-mysql php7.2-cli php7.2-zip php7.2-curl
php -v
```
```
sudo nano /etc/php/7.2/fpm/php.ini
```

```
file_uploads = On
allow_url_fopen = On
memory_limit = 256M
upload_max_filesize = 100M
max_execution_time = 360
date.timezone = America/Chicago
```

## 4.6 Install unzip
```
sudo apt-get install git unzip
```
## 4.7 Install Composer
```
curl -sS https://getcomposer.org/installer -o composer-setup.php
sudo php composer-setup.php --install-dir=/usr/local/bin --filename=composer
```

## 4.8 Install Laravel
```
composer create-project --prefer-dist laravel/laravel blog
```

## 4.6 Site settings
```
sudo nano /etc/nginx/snippets/phpmyadmin.conf
```

```
location /phpmyadmin {
    root /usr/share/;
    index index.php index.html index.htm;
    location ~ ^/phpmyadmin/(.+\.php)$ {
        try_files $uri =404;
        root /usr/share/;
        fastcgi_pass unix:/run/php/php7.2-fpm.sock;
        fastcgi_index index.php;
        fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
        include /etc/nginx/fastcgi_params;
    }

    location ~* ^/phpmyadmin/(.+\.(jpg|jpeg|gif|css|png|js|ico|html|xml|txt))$ {
        root /usr/share/;
    }
}
```

## For Laravel
```
sudo nano /etc/nginx/snippets/default
```

## 4.5 Install phpMyAdmin
```
sudo apt install phpmyadmin
sudo nano /etc/nginx/snippets/phpmyadmin.conf
```

```
location / phpmyadmin {
    root / usr / share /;
    index index.php index.html index.htm;
    location ~ ^ / phpmyadmin / (. + \. php) $ {
        try_files $ uri = 404;
        root / usr / share /;
        fastcgi_pass unix: /run/php/php7.2-fpm.sock;
        fastcgi_index index.php;
        fastcgi_param SCRIPT_FILENAME $ document_root $ fastcgi_script_name;
        include / etc / nginx / fastcgi_params;
    }

    location ~ * ^ / phpmyadmin / (. + \. (jpg | jpeg | gif | css | png | js | ico | html | xml | txt)) $ {
        root / usr / share /;
    }
}
```

sudo nano /etc/nginx/sites-available/example.com.conf
```
server {
    listen 80;
    listen [::]:80;
    root /var/www/html/example.com;
    index  index.php index.html index.htm;
    server_name  example.com www.example.com;

     client_max_body_size 100M;

     autoindex off;
  
     location / {
        try_files $uri $uri/ /index.php?$query_string;
      }

    location ~ \.php$ {
         include snippets/fastcgi-php.conf;
         fastcgi_pass unix:/var/run/php/php7.2-fpm.sock;
         fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
         include fastcgi_params;
    }

   include snippets/phpmyadmin.conf;
}
```
```
sudo ln -s /etc/nginx/sites-available/example.com.conf /etc/nginx/sites-enabled/
sudo systemctl restart nginx.service
sudo systemctl restart php7.2-fpm.service
```

## 4.6 Connecting to phpMyAdmin

http://example.com/phpmyadmin
```
sudo mysql -u root

use mysql;
update user set plugin='' where User='root';
flush privileges;
exit

sudo systemctl restart mariadb.service

https://websiteforstudents.com/install-nginx-mariadb-and-php-7-2-fpm-with-phpmyadmin-on-ubuntu-16-04-18-04-18-10-lemp-phpmyadmin/
```

