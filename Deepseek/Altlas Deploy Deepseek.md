### 参考链接
[1. 使用昇腾Ascend 300I Pro 310P芯片 单卡运行DeepSeek-R1-Distilled-Qwen-7B_昇腾300i pro](https://blog.csdn.net/aosudh/article/details/146044037)
[2. 鲲鹏+昇腾部署DeepSeek-R1-Distill-Qwen-32B+Open-webui【信创国产化】（详细存档版）_atlas 300i duo部署deepseek](https://blog.csdn.net/mizhiakk/article/details/145651471?utm_medium=distribute.pc_relevant.none-task-blog-2~default~baidujs_baidulandingword~default-0-145651471-blog-145604232.235^v43^pc_blog_bottom_relevance_base6&spm=1001.2101.3001.4242.1&utm_relevant_index=2)

### 部署Deepseek大致步骤
- 安装AI芯片的固件驱动
- 安装Docker，用Docker运行MindIE（Mind Inference Engine，昇腾推理引擎）
- 下载大模型文件
- 启动和配置大模型文件
#### 安装固件与驱动
[https://support.huawei.com/enterprise/zh/doc/EDOC1100422575/289e2d2d](https://support.huawei.com/enterprise/zh/doc/EDOC1100422575/289e2d2d "https://support.huawei.com/enterprise/zh/doc/EDOC1100422575/289e2d2d")
#### 安装docker
[Get Docker | Docker Docs](https://docs.docker.com/get-started/get-docker/)
#### 下载MindlE Docker镜像
在华为MindIE官网中，通过申请后即可下载镜像[https://www.hiascend.com/developer/ascendhub/detail/af85b724a7e5469ebd7ea13c3439d48f](https://www.hiascend.com/developer/ascendhub/detail/af85b724a7e5469ebd7ea13c3439d48f "https://www.hiascend.com/developer/ascendhub/detail/af85b724a7e5469ebd7ea13c3439d48f")
#### 模型下载
在huggingface或者魔塔社区中，可以下载到原版的safetensor模型权重文件
[huggingface](https://huggingface.co/deepseek-ai/DeepSeek-R1-Distill-Qwen-7B/tree/main)
[魔搭社区](https://www.modelscope.cn/models/deepseek-ai/DeepSeek-R1-Distill-Qwen-7B)
下载好后解压到对应文件夹，给文件夹中所有文件赋予权限
### 示例(Altlas 300V Pro视频推理卡)
#### 1. 前期准备
在宿主机输入
```sh
lspci -n -D #该命令查询本机的device pci id
```
300I 系列310V 芯片pcie device id 为 d500，300T 系列 910B 芯片pcie device id 为 d801，可以得出下载的固件驱动和MindIE镜像需要支持d500芯片
**本次略过安装固件和Docker**
安装好固件驱动后可以使用以下命令来查询是否安装正确
```sh
npu-smi info #查询当前安装Altlas芯片情况
```
安装好Docker**可以使用以下命令来查询是否安装正确
```sh
docker --version #查询当前安装的docker版本
```
#### 2. 下载支持d500的MindIE镜像
https://tools.obs.cn-south-292.ca-aicc.com/tools/mindie_docker_images/mindie_1.0.0_300IDuo.tar
#### 3. 下载大模型文件(32b)
[deepseek-ai (DeepSeek)](https://huggingface.co/deepseek-ai)在这个找到32B这个文件并点击
![[HuggingFace-Deepseek-ai.png]]
![[HuggingFace-Deepseek-ai-32B.jpg]]
![[HuggingFace-Deepseek-ai-32B-1.png]]
点击git clone后面的复制按钮，在本地cmd上运行下载(需要git-bash)
#### Docker启动镜像
##### docker加载上述下载的镜像
```sh
docker load -i mindie_1.0.0_300IDuo.tar
```
##### 修改下载后的大模型的精度
如上文所述，在大模型推理应用中 310P 芯片仅支持FP16精度，并不支持BF16或INT8等数据类型，因此需要到模型权重文件中修改config.json
```sh

```
修改dtype为：float16后保存

##### 查询Altlas的卡id
```sh
```
##### 运行容器
```sh
docker run -itd --ipc=host \
--name=mindie_1.0.0 \
--privileged=true \
--net=host \
--entrypoint=/bin/bash \
--device=/dev/davinci_manager \
--device=/dev/devmm_svm \
--device=/dev/hisi_hdc \
-v /etc/hccn.conf:/etc/hccn.conf \
-v /etc/localtime:/etc/localtime \
-v /usr/local/Ascend/driver:/usr/local/Ascend/driver \
-v /usr/local/Ascend/add-ons/:/usr/local/Ascend/add-ons/ \
-v /usr/local/sbin/:/usr/local/sbin/ \
-v /usr/local/sbin/npu-smi:/usr/local/sbin/npu-smi \
-v /var/log/npu/conf/slog/slog.conf:/var/log/npu/conf/slog/slog.conf \
-v /var/log/npu/slog/:/var/log/npu/slog \
-v /var/log/npu/profiling/:/var/log/npu/profiling \
-v /var/log/npu/dump/:/var/log/npu/dump \
-v /var/log/npu/:/usr/slog \
-v {主机中权重文件的绝对路径}:{主机中权重文件的绝对路径} \
{本地镜像名}
```
##### 配置
1. 查询容器id
```sh
docker ps
```
2. 进入容器
```sh
docker exec -it ${容器id} bash
```
3. 进入环境后，在 ~/.bashrc 中添加自动环境变量使能语句
```sh
source /usr/local/Ascend/ascend-toolkit/set_env.sh
source /usr/local/Ascend/nnal/atb/set_env.sh 
source /usr/local/Ascend/atb-models/set_env.sh
source /usr/local/Ascend/mindie/set_env.sh
```
4. 修改配置文件
```
vim /usr/local/Ascend/mindie/1.0.0/mindie-service/conf/config.json
```
npuDeviceIds：单卡推理，所以填[[0]]，假如是多卡，则根据卡数量填[[0,1,2,3][4,5,6,7]]等
truncation：false
modelName：自定义的模型名
modelWeightPath：模型的路径
worldSize：这里填实际推理要使用的卡的数量
httpsEnabled" : false 这个不改会报错
ip：[] 默认是127.0.0.1，如果不该，只能本地访问
maxIterTimes与maxBatchSize： 如果需要输入长上下文，则按需改大这两个位置

保存后退出

5. 启动
```
cd /usr/local/Ascend/mindie/latest/mindie-service/bin
./mindieservice_daemon
```
当出现Daemon start success时，即为服务启动成功