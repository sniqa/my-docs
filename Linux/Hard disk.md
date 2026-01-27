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
sudo parted /dev/sda mkpart primary 0% 100% #分区使用整个磁盘空间
```
#### 格式化磁盘
```shell
sudo mkfs.ext4 /dev/sda1 #格式化分区sda1,文件格式为ext4
```
#### 挂载分区到目录
```shell
sudo cat >> /etc/fstab << eof
UUID=d8976cbc-ee12-4579-8270-b3995b55dcd4 /data01 ext4    defaults        0 0
eof   #将挂载到data01目录 文件格式为ext4
```
#### 刷新挂载
```shell
sudo mount -a #将/etc/fstab的配置挂载
```


### 硬盘组软raid
#### 1. 组建软raid需要的工具：mdadm
```shell
yum install mdadm
```
#### 2. 创建raid阵列
```shell
mdadm --create --verbose /dev/md0 --level=5 --raid-devices=3 /dev/sdb /dev/sdc /dev/sdd #使用三个硬盘创建raid5名称为md0
```
#### 3. 查看md的状态**必须等待md的状态完成**
```shell
watch -n 1 cat /proc/mdstat
```
#### 4. 查看raid的详细信息
```shell
mdadm -D /dev/md0
```
#### 5. 创建文件系统
```shell
parted /dev/md0 mklabel gpt #将md0改为gpt
parted /dev/md0 mkpart primary 0% 100% #将md0全部容量划为一个分区
mkfs.xfs /dev/md0  #将md0格式化为xfs，需系统支持xfs
```
#### 6. 挂载RAID阵列
```shell
echo '/dev/md0 /data xfs defaults 0 0 | sudo tee -a /etc/fstab' #写入/etc/fstab
mount -a #将/etc/fstab的配置挂载
```
#### 7. 保存RAID配置以确保在启动时加载RAID配置
```shell
mdadm --detail --scan | sudo tee -a /etc/mdadm.conf
```
#### 8. 更新initramfs，以便启动时加载RAID模块
```shell
dracut -force
```
#### 9.禁用启用RAID
```shell
umount /data #卸载
mdadm -S /dev/md0 #停止
mdadm -A /dev/md0 #启用
```
#### 10.模拟/dev/sdc损坏
```shell
mdadm /dev/md5 -f /dev/sdc
mdadm -D /dev/md0 # 查看md0的状态
```
#### 11. 移除RAID里的硬盘
```shell
mdadm /dev/md0 -r /dev/sdc
mdadm -D /dev/md0
lsblk #查看硬盘状态
```
#### 12. 添加到raid成员
```shell
mdadm /dev/md0 -a /dev/sdc
mdadm -D /dev/md0
```
#### 13.变更RAID配置
```shell
mdamd -G /dev/md0 -n 4 -a /dev/sdf #md0成员从3个到4个
e2fsck -f /dev/md0 #检测文件系统完整性
resize2fs /dev/md0 #同步文件系统
```
