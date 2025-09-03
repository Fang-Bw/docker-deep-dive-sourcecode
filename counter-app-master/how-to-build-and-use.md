# 镜像管理
## 1.生成镜像并后台运行
```bash
# -d则是后台启动，此处是将输出直接打印在控制台
docker-compose up -&

# 生成的镜像列表(docker-compose.yml文件生成的镜像名称，项目名-资源名)
counter-app-master-web-fe   latest      96..6ff9e   3 minutes ago   95.9MB
python              3.4-alpine  01..17a02   2 weeks ago     85.5MB
redis               alpine      ed..c83de   5 weeks ago     26.9MB

# 生成的容器列表
docker container ls -a
CONTAINER ID   IMAGE                       COMMAND                   CREATED        STATUS                      PORTS     NAMES
869992dbebbb   redis:alpine                "docker-entrypoint.s…"   22 hours ago   Exited (0) 21 hours ago               counter-app-master-redis-1
6a44e3665e60   counter-app-master-web-fe   "python app.py"           22 hours ago   Exited (0) 21 hours ago               counter-app-master-web-fe-1

# 网络和卷
counter-app-master_counter-net的网络
counter-app-master_counter-vol的卷
```

## 2.使用docker-compose命令
```bash
# docker-compose命令使用时，需要在当前目录下存在docker-compose.yml文件
docker-compose ps -a
# 停止docker-compose运行的容器，不会删除任何东西
docker-compose stop
# 停止docker-compose运行的容器，删除容器和网络
docker-compose down
```

