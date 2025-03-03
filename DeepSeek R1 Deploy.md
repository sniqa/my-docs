### 视频参考地址

```cardlink
url: https://www.bilibili.com/video/BV1atfQYQE9P/?spm_id_from=333.1387.favlist.content.click
title: "DeepSeek R1 推理模型 一键包 完全本地部署 保姆级教程 断网运行 无惧隐私威胁 大语言模型推理时调参 CPU GPU 混合推理 32B 轻松本地部署_哔哩哔哩_bilibili"
description: "DeepSeek R1 推理模型的蒸馏模型通过LM Studio完全断网地本地部署，并使用Windows防火墙禁用入站出站规则，完全禁用网络，本地运行。使用Huggingface和LM Studio下载更多模型和Q4 Q8量化精度，使用LM Studio进行CPU GPU混合推理，让你的笔记本轻松支持32B 大模型！使用我的一键包，跟着我的教程做，再对模型推理调参提高性能，帮助你探索、科研、探索二, 视频播放量 1291418、弹幕量 1814、点赞数 50051、投硬币枚数 38814、收藏人数 107087、转发人数 12844, 视频作者 NathMath, 作者简介 让科技散发人性之光 只做世界的开源者，相关视频：清华大佬终于把DeepSeek讲明白了！适合所有人学习，如何入门到精通？少走99%的弯路！存下吧！很难找全的！，【喂饭教程】20分钟教会你本地部署DeepSeek-R1，并搭建自己的知识库（附Windows安装包+使用技巧），【摆脱卡顿】DeepSeek全网最全的实战技巧！建议收藏～，DeepSeek R1 推理模型 性能调优 收官之作 完全本地部署 保姆级教程 无惧隐私威胁 使用正确的参数 让你的R1快上2倍，4K | 本地部署DeepSeek-R1后，搭建自己的知识库，全网最详细！DeepSeek本地部署教程，手机和电脑都能用 | 建议收藏~，可能是最简单的DeepSeek R1本地运行教程，小学生都能搞定的deepseek，本地部署做游戏辅导？，【DeepSeek保姆级教程】20分钟教会你本地部署DeepSeek-R1，完全免费本地部署+联网搜索，丝滑无卡顿，1秒响应，轻松本地部署，老奶奶都能学会！，10大隐藏提示词，教你把Deepseek训练成精！"
host: www.bilibili.com
image: https://i2.hdslb.com/bfs/archive/a8713ad3f12f15ba66d15aaaa7e5d959cbcbb032.jpg@100w_100h_1c.png
```

[文字流程参考地址](https://zhuanlan.zhihu.com/p/22270028614)

[本地部署deepseek模型保姆级教程 - 封尘博客](https://blog.lovefc.cn/archives/start.html)

### 1.下载Ollama https://ollama.com/
### 2.使用Ollama 运行以下命令
```
ollama run deepseek-r1:7b #7b参数
```
#### windows下ollama下载的模型存放路径: 
``C:\Users\user\.ollama\``
### 3.安装webui界面
安装open-webui作为Deepseek 的web ui页面
[🏡 Home | Open WebUI](https://docs.openwebui.com/)
安装Nextjs-ollama-llm-ui作为UI界面
[jakobhoeg/nextjs-ollama-llm-ui: Fully-featured web interface for Ollama LLMs](https://github.com/jakobhoeg/nextjs-ollama-llm-ui)
### 4. Deepseek安装要求

![[Deepseek requirement.jpg]]