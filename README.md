version: '3.8'

services:
  db:
    image: mysql:8.0
    container_name: db
    environment:
      - MYSQL_DATABASE=laravel
      - MYSQL_USER=user
      - MYSQL_PASSWORD=password
      - MYSQL_ROOT_PASSWORD=password
      - TZ='Asia/Tokyo'
    command: mysqld --character-set-server=utf8mb4 --collation-server=utf8mb4_unicode_ci
    volumes:
      - data:/var/lib/mysql
      - ./docker/db/my.cnf:/etc/mysql/conf.d/my.cnf
      - ./docker/db/sql:/docker-entrypoint-initdb.d
    ports:
      - 3306:3306

  nginx:
    build:
      context: .
      dockerfile: ./docker/nginx/Dockerfile
    container_name: nginx
    ports:
      - 80:80
    volumes:
      - ./src:/var/www/app
      - ./docker/nginx/default.conf:/etc/nginx/nginx.conf
    depends_on:
      - app

  app:
    build:
      context: .
      dockerfile: ./docker/php/Dockerfile
    container_name: app
    volumes:
      - ./src:/var/www/app
    depends_on:
      - db
