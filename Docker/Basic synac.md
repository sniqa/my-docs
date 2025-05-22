### 导出镜像
```sh
docker save -o  {{path/to/file.tar}} {{image}}:{{tag}}
```

### 添加环境变量
```
docker run -d -e [env] [name] [images name]
```