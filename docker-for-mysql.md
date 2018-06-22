# docker-for-mysql設定

## 1、docker-compose

    version: "3"  #定義docker-compose版本
    services:
      mysql:
        container_name: mysql_han   #定義container的名稱，方便以後在命令行使用，本例定義為mysql_han
        image: mysql:5.6
        volumes:
          - $PWD/mysql/data/:/var/lib/mysql   #掛載mysql數據卷
          - $PWD/mysql/my.cnf:/etc/my.cnf
        ports:
          - "127.0.0.1:3306:3306"    #定義端口映射，將docker容器的3306端口映射到宿主機3306端口，這樣在宿主機可以直接訪問mysql
        environment:
          - MYSQL_ROOT_PASSWORD=[mysql的root密碼]
          - LANG=C.UTF-8  #定義數據庫字符集
        restart: always   
        privileged: true
