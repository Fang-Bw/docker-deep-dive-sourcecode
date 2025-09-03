# 镜像管理
## 1.创建镜像
```
# 1.打开docker-desktop
# 2.打开终端，定位到项目目录
# 3.输入以下命令：
docker image build -t bowen9527/images_hub:v1.0 .
```

## 2.推送镜像到镜像仓库
```
# 1.登录镜像仓库
docker login
# 2.推送镜像到镜像仓库(需要有该 bowen9527/images_hub 仓库)
docker push bowen9527/images_hub:v1.0
```

## 3.运行镜像
```bash
# -d 后台运行 c1 容器名称 -p 映射端口，主机的80端口映射到容器的8080端口
docker container run -d --name c1 \
  -p 80:8080 \
  bowen9527/images_hub:v1.0
  
# 访问 localhost:80 即可访问容器内的应用

# 进入容器，alpine基础镜像不包含bash，所以使用sh
docker exec -it 7695586bb306 sh
# 退出容器
exit
```

## 3.运行镜像时指定卷
```bash
# 创建命名卷（基于默认配置）
docker volume create images_hub_volume

# 查看卷信息
docker volume inspect images_hub_volume
# 具体信息
[
    {
        "CreatedAt": "2025-09-01T08:25:40Z",
        "Driver": "local",
        "Labels": null,
        "Mountpoint": "/var/lib/docker/volumes/images_hub_volume/_data",
        "Name": "images_hub_volume",
        "Options": null,
        "Scope": "local"
    }
]

# 运行容器时将卷挂载到容器中的指定目录(/opt/logs)下
docker run -d  --name c2 \
  --mount type=volume,source=images_hub_volume,target=/opt/logs \
  -p 80:8080 \
  bowen9527/images_hub:v1.0

# 进入容器，alpine基础镜像不包含bash，所以使用sh
docker exec -it 7695586bb306 sh

# 创建日志文件
echo "hello world" > /opt/logs/app.log

# 退出容器查看对应的卷下的内容，因为macos上的docker是通过创建一个linux虚拟机来运行docker，所以docker的卷是在linux虚拟机中的，而不是macos的
```

```bash
# 基于上述问题，通过绑定挂载 解决
docker container run -d --name c3 \
  -p 80:8080 \
  -v /Users/bowen/IdeaProjects/docker-deep-dive-sourcecode/psweb-master/volume:/opt/logs \
  bowen9527/images_hub:v1.0
  
# 进入容器，alpine基础镜像不包含bash，所以使用sh
docker exec -it 7695586bb306 sh

# 创建日志文件
echo "hello world" > /opt/logs/app.log
```

## 4.运行镜像时指定网络，实现两个容器之间的网络通信
```bash
docker network ls

docker container run -d --name c4 \
--network bridge \
-p 80:8080 \
bowen9527/images_hub:v1.0

docker container run -d --name c5 \
--network bridge \
-p 81:8080 \
bowen9527/images_hub:v1.0

# 查看该网络下的每个容器的网络
docker network inspect bridge

# 进入c5容器，alpine基础镜像不包含bash，所以使用sh
docker exec -it 7695586bb306 sh

# ping c4 容器的ip
```

# 附录
## 1.常见问题
### 1.1 无法拉取基础镜像
```bash
#更换镜像源
"registry-mirrors": [
"https://registry.docker-cn.com"
]
