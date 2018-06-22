#docker-for-mysql設定

##1、docker-compose

version: "3"

services:
  mysql:
    container_name: mysql_han
    image: mysql:5.6
    volumes:
      - /Users/hanwei/Documents/mysql/data/:/var/lib/mysql
      - /Users/hanwei/Documents/mysql/my.cnf:/etc/my.cnf
    ports:
      - "127.0.0.1:3306:3306"
    environment:
      - MYSQL_ROOT_PASSWORD=han123
      - LANG=C.UTF-8
    restart: always
    privileged: true
