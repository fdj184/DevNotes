# Commands

## 列出所有 container

```sh
docker ps -a
```

<https://docs.docker.com/engine/reference/commandline/ps/>

## 清理沒在使用的 volume

```sh
docker volume prune
```

<https://docs.docker.com/engine/reference/commandline/volume_prune/>

## 移動 image 存放位置

Docker image 在 Windows 10 的存放路徑預設為 `%USERPROFILE%\AppData\Local\Docker\wsl\data\ext4.vhdx`

1. 結束 Docker Desktop 程式
2. 在 cmd 輸入 `wsl --list -v`，確認 WSL 狀態皆已停止

    ```sh
      NAME                   STATE           VERSION
    * docker-desktop         Stopped         2
      docker-desktop-data    Stopped    
    ```

3. 匯出 docker-desktop-data 備份

    ```sh
    wsl --export docker-desktop-data "D:\Docker\wsl\data\docker-desktop-data.tar"
    ```

4. 從 WSL 註銷 docker-desktop-data，完成後會自動將 `ext4.vhdx` 刪除

    ```sh
    wsl --unregister docker-desktop-data
    ```

5. 匯入 docker-desktop-data

    ```sh
    wsl --import docker-desktop-data "D:\Docker\wsl\data" "D:\Docker\wsl\data\docker-desktop-data.tar" --version 2
    ```

6. 刪除 docker-desktop-data 備份檔 (`D:\Docker\wsl\data\docker-desktop-data.tar`)

ref: [How can I change the location of docker images when using Docker Desktop on WSL2 with Windows 10 Home?](https://stackoverflow.com/questions/62441307)

## 進入 container

```sh
docker exec -ti {containerName} bash
```
