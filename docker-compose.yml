#!/bin/bash
touch docker-compose.yml
mkdir -p nginx/
mkdir -p db-data/
mkdir -p logs/nginx/
mkdir -p wordpress/

cat >  "nginx/wordpress.conf" << ENDF
server {
    listen 80;
    server_name wp-hakase.co;
 
    root /var/www/html;
    index index.php;
 
    access_log /var/log/nginx/hakase-access.log;
    error_log /var/log/nginx/hakase-error.log;
 
    location / {
        try_files $uri $uri/ /index.php?$args;
    }
 
    location ~ \.php$ {
        try_files $uri =404;
        fastcgi_split_path_info ^(.+\.php)(/.+)$;
        fastcgi_pass wordpress:9000;
        fastcgi_index index.php;
        include fastcgi_params;
        fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
        fastcgi_param PATH_INFO $fastcgi_path_info;
    }
}
ENDF

cat > "docker-compose.yml" << "EOF"
version: 2
services:
  nginx:
    image: nginx:latest
    ports:
        - '80:80'
    volumes:
        - ./nginx:/etc/nginx/conf.d
        - ./logs/nginx:/var/log/nginx
        - ./wordpress:/var/www/html
    links:
        - wordpress
    restart: always
    
  mysql:
    image: mariadb
    ports:
        - '3306:3306'
    volumes:
        - ./db-data:/var/lib/mysql
    environment:
        - MYSQL_ROOT_PASSWORD=aqwe123
    restart: always
    
  wordpress:
    image: wordpress:4.7.1-php7.0-fpm
    ports:
        - '9000:9000'
    volumes:
        - ./wordpress:/var/www/html
    environment:
        - WORDPRESS_DB_NAME=wpdb
        - WORDPRESS_TABLE_PREFIX=wp_
        - WORDPRESS_DB_HOST=mysql
        - WORDPRESS_DB_PASSWORD=aqwe123
    links:
        - mysql
    restart: always
    EOF
  
