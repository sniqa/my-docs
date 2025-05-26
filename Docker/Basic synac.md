### 导出镜像
```sh
docker save -o  {{path/to/file.tar}} {{image}}:{{tag}}
```

### 添加环境变量
```
docker run -d -e [env] [name] [images name]
```

### Docker cp命令
```
docker cp [OPTIONS] SRC_PATH CONTAINER:DEST_PATH
docker cp [OPTIONS] CONTAINER:SRC_PATH DEST_PATH
```
