# docker-for-mysql設定

## 1、docker-compose

compose文件以.yaml後綴

    version: "3"                                 #定義docker-compose版本
    services:
      mysql:
        container_name: mysql_han                #定義container的名稱，方便以後在命令行使用，本例定義為mysql_han
        image: mysql:5.6                         #下載mysql5.6 image
        volumes:
          - $PWD/mysql/data/:/var/lib/mysql      #掛載mysql數據卷，容器與數據分離，保證容器重啓之後數據不丟失。$PWD為項目實際路徑
          - $PWD/mysql/my.cnf:/etc/my.cnf        #掛載mysql配置文件，方便在容器外修改重啓後不丟失，mysql第一個配置文件位置/etc/my.cnf
        ports:
          - "127.0.0.1:3306:3306"                #定義端口映射，將docker容器的3306端口映射到宿主機3306端口，這樣在宿主機可以直接訪問mysql
        environment:
          - MYSQL_ROOT_PASSWORD=[mysql的root密碼]
          - LANG=C.UTF-8                         #定義數據庫字符集
        restart: always                          #容器掛掉之後會自動重啓
        privileged: true                         #解決訪問權限問題
        
## 2、docker-compose基本命令

在docker-compose文件目錄下執行命令

    docker-compose up -d   #創建並啟動容器，如果image不存在自動去docker hub抓取，並在後台守護運行
    docker-compose down    #關閉當前容器，同時刪除image，network，container、volumes
    docker-compose ps      #列出當前運行的容器
    docker-compose rm      #停止並刪除容器
    
## 3、docker與數據庫交互

docker命令行導入mysql數據庫

    docker exec -i mysql_han mysql -uusername -ppassword databasename < $PWD/databasename.sql   #注意不能加-t參數，會報錯

進入mysql容器

    dokcer exec -it mysql_han bash
    
查看容器運行log

    docker logs --follow mysql_han
    
拷貝文件到容器內

    docker cp filename mysql_han:/root/   #容器名:容器內文件夾路徑
    
macos與容器mysql同步時間

    1. ssh進入容器 docker exec -it mysql_han bash
    2. $ dpkg-reconfigure tzdata
    3. $ date
