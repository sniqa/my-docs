### 查看当前操作系统版本
```shell
cat /proc/version
```
### 后台执行并把输出日志保留
```
nohup ./test.sh > output.log 2>&1 &
```
