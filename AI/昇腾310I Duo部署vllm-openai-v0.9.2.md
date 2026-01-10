### 1.vllm-openai:v0.9.2 只有amd64镜像，要在arm64架构上运行，需要QEMU
```shell
docker run --rm --privileged multiarch/qemu-user-static --reset -p yes
```
