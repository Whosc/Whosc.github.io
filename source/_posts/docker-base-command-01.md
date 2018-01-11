---
title: docker 常用基础命令！
date: 2018-01-10
tags: [docker]
categories:
- 技术
- docker

---


###  卸载老版本的 docker

```
$ sudo yum remove docker \
                  docker-common \
                  docker-selinux \
                  docker-engine
```
<!--more-->
### 安装docker 
```
yum install docker 
```

### 运行容器 
```
docker run 
-t :在新的容器中制定一个伪终端或终端
-i :允许你对容器内的标准输入 (STDIN) 进行交互。
-d :让容器在后台运行
-P :将容器内部使用的网络端口映射到我们使用的主机上 
-p :绑定指定端口

示例：docker run -d -p 5000:5000 training/webapp python app.py

exit : 从交互式的容器中退出
```

### 停止容器
```
dokcer stop [容器名|容器ID]

停止所有容器
docker stop $(docker ps -a -q)
```
### 查看正在运行的容器
```

docker ps
```
### 查看容器内标准输出
```
docker logs [容器名|容器ID]
```
### 查看容器的网络端口
```
docker port [容器名|容器ID]
```
### 查看容器内运行的进程
```
docker top
```
### 查看Docker的底层信息
```
docker inspect
runoob@runoob:~$ docker inspect determined_swanson
[
    {
        "Id": "7a38a1ad55c6914b360b565819604733db751d86afd2575236a70a2519527361",
        "Created": "2016-05-09T16:20:45.427996598Z",
        "Path": "python",
        "Args": [
            "app.py"
        ],
        "State": {
            "Status": "running",
......
```
### 重启容器
```
docker star [容器名|容器ID]
```
### 删除容器,删除前需要先停止
```
docker rm [容器名|容器id]

docker rm $(docker ps -a -q)
```

### 列出所有镜像
```
docker images
```

### 查找镜像
```
docker search mysql
```




