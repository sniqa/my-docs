### 查看当前操作系统版本
```shell
cat /proc/version
```
### 后台执行并把输出日志保留
```
nohup ./test.sh > output.log 2>&1 &
```

### 硬盘相关操作
#### 查看硬盘分区与挂载
```shell
sudo lsblk
```
#### 获取硬盘UUID
```shell
sudo blkid | sort
```
#### 使用parted将磁盘模式改为gpt
```shell
sudo parted /dev/sda mklabel gpt
```
#### 设置磁盘分区
```shell
sudo parted /dev/sda mkpart primary 0% 100% //分区使用整个磁盘空间
```
#### 格式化磁盘
```shell
sudo mafs.ext4 /dev/sda1 //格式化分区sda1,文件格式为ext4
```
#### 挂载分区到目录
```shell
sudo cat >> /etc/fstab << eof
UUID=d8976cbc-ee12-4579-8270-b3995b55dcd4 /data01 ext4    defaults        0 0
eof   //将挂载到data01目录 文件格式为ext4
```
#### 挂载
```shell
sudo mount -a
```
