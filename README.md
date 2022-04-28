# docker-php8.1.5-fpm-xdebug-git

based on https://github.com/docker-library/php

added git support

Dockerfile for PHP 8.1.5 fpm

* with PDO (MySQL client)
* with xdebug (client_host=127.0.0.1, client_port=9003, idekey="PHPSTORM")
* with git (clone in composer.phar)

## docker hub link

https://hub.docker.com/repository/docker/petranek80/php8.1.5-fpm-xdebug-git

### 1. add PHP docker image to docker-compose.yml file

```
  php:
    image: petranek80/php8.1.5-fpm-xdebug-git
    volumes:
      - ".:/var/www/html"
      - "./etc/php/php.ini:/usr/local/etc/php/conf.d/custom.php.ini"
```
      

### 2. create PHP config file in project folder

/etc/php/php.ini
```
[pdo_mysql]
pdo_mysql.default_socket=/var/run/mysqld/mysqld.sock
```
### 3. configure PHP in nginx server [OPTIONAL]

add to nginx config file (etc. default.template.conf)

```
server {

...

    location ~ \.php$ {
        fastcgi_buffers 16 16k;
        fastcgi_buffer_size 32k;
        fastcgi_split_path_info ^(.+\.php)(/.+)$;
        fastcgi_pass php:9000;
        fastcgi_index index.php;
        include fastcgi_params;
        fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
        fastcgi_param PATH_INFO $fastcgi_path_info;
    }
}    
```    
