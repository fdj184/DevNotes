# MySQL

## 建立 root 以外的使用者 (指定 Database)

```yml
# docker-compose.yml

services:
  db:
    image: mysql:8
    environment:
      MYSQL_ROOT_PASSWORD: rootpwd
      MYSQL_DATABASE: default-db
      MYSQL_USER: foo
      MYSQL_PASSWORD: bar
```

ref: [MySQL Official Image](https://hub.docker.com/_/mysql)

## 初始化時執行指令

以下為啟動 container 時，建立 root 以外的使用者 (不指定 Database)，

1. 在本機目錄 (e.g. `./init`) 目錄放入欲執行的 script

    ```sql
    CREATE USER 'foo'@'%' IDENTIFIED WITH mysql_native_password BY 'bar';
    GRANT ALL PRIVILEGES ON *.* TO 'foo'@'%';
    FLUSH PRIVILEGES;
    ```

2. 指定目錄到初始化就會執行的資料夾

    ```yml
    # docker-compose.yml

    services:
      db:
        image: mysql:8
        environment:
          MYSQL_ROOT_PASSWORD: rootpwd
        volumes:
          - ./init:/docker-entrypoint-initdb.d
    ```

## 設定時區

```yml
# docker-compose.yml

services:
  db:
    image: mysql:8
    environment:
      MYSQL_ROOT_PASSWORD: rootpwd
      TZ: Asia/Taipei
```

## 設定預設編碼

```yml
# docker-compose.yml

services:
  db:
    image: mysql:8
    environment:
      MYSQL_ROOT_PASSWORD: rootpwd
    command: ['mysqld', '--character-set-server=utf8mb4', '--collation-server=utf8mb4_0900_ai_ci', '--skip-character-set-client-handshake']
```

## 同時啟動不同分支的 MySQL

1. 事先定義不同名稱的 service

    ```yml
    # docker-compose-main.yml

    services:
      db-main:
        image: mysql:8
        environment:
          MYSQL_ROOT_PASSWORD: rootpwd
    ```

    ```yml
    # docker-compose-sub.yml

    services:
      db-sub:
        image: mysql:8
        environment:
          MYSQL_ROOT_PASSWORD: rootpwd
    ```

2. 啟動 container 時指定不同名稱的專案名稱

    ```sh
    docker-compose -f .\docker-compose.yml -p db-main up -d
    ```

    ```sh
    docker-compose -f .\docker-compose.yml -p db-sub up -d
    ```
