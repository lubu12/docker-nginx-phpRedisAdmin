# Nginx + phpRedisAdmin (php-fpm) via Docker - Alpine

Run Nginx and phpRedisAdmin (php-fpm) - Alpine via docker-compose

Reference: https://github.com/erikdubbelboer/phpRedisAdmin

Tested at AWS EC2 and RDS - Amazon Linux 2

## Create new user and group
The new user and group will be running the nginx and php-fpm instances. UID and GID for `app:app` are 3000.

Example at EC2 - Amazon Linux 2
```
sudo groupadd -g 3000 app
sudo useradd -s /sbin/nologin -g 3000 -u 3000 app
```

## Configure nginx.conf, php-fpm and phpRedisAdmin configuration files
```
nginx/nginx.conf
phpredisadmin/php-fpm.conf
phpredisadmin/php.ini
phpredisadmin/www.conf
phpredisadmin/config.inc.php
```

## Create config.inc.php under phpredisadmin/ - phpRedisAdmin configuration file
Sample file can be obtained from https://github.com/erikdubbelboer/phpRedisAdmin/blob/master/includes/config.sample.inc.php

## Add the executable permission to docker-entrypoint.sh to fix the permission denied issue
```
chmod +x phpmyadmin/docker-entrypoint.sh
```
Reference: https://github.com/composer/docker/issues/7

## Make sure phpMyAdmin directory does not exist under /var/www/html/
Our phpredisadmin/docker-entrypoint.sh will automatically create phpRedisAdmin directory under /var/www/html/. It may cause unexpected issue if phpRedisAdmin directory exists.

## Build and run with docker-compose
```
docker-compose up -d
```

## Rebuild the docker image after nginx or php-fpm configuration file is changed
```
docker-compose down
docker-compose up --build -d
```
