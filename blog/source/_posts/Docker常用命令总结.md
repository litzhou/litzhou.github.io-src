title: Docker 常用命令总结
date: 2017-12-05 21:28:48
tags: 
    - Docker
---

## 构建Docker镜像
```
FROM hub.c.163.com/xbingo/jdk8
ADD ./app-1.0.jar  /app.jar
EXPOSE 80
CMD ["java","-jar","/app.jar"]
```
* FROM 基础镜像
* ADD 拷贝文件到容器目录
* EXPOSE 80 暴露80端口
* CMD 容器内部执行的命令

<!-- more -->

```
docker build -t jameszhou/app:1.0 .
```
* docker build 构建Docker镜像的命令;
* -t jameszhou/app:1.0 要构建的镜像的名称,1.0表示镜像的tag,如不指定则使用的默认的tag latest;
* “.”表示Dockefile文件所在的路径，这里为当前路径，故用“.”代替;

## 查看镜像列表
```
docker images
```

## 启动容器
常用两种方式
```
docker run -d -p 8080:80 --name app jameszhou/app:1.0   //后台运行
docker run -it -p 8080:80 --name app jameszhou/app:1.0 //交互式运行
```
* docker run 启动Docker容器命令;
* -d 表示后台运行;
* -it 以交互模式启动;
* -p 8080:80,将容器的80端口映射到主机的8080,注意顺序,”主机端口:容器端口”;
* –name app，给容器起个名字;
* jameszhou/app:1.0,拥有jameszhou/app这个镜像，tage为1.0，不指定tag自动找latest；

## 启动容器并挂载目录
```
docker run -d -p 8080:80 --name app -v var/lib/docker/data:/var/lib/docker/data jameszhou/app:1.0   
```
* -v 表示要挂载的目录,格式为“主机目录:容器目录”,前面的是主机目录,后面的是容器目录;

## 查看容器列表

```
docker ps 
docker ps -a
```
* -a 表示查看所有容器列表（包含停止的容器）,不加表示只查看正在运行的容器

## 启动停止的容器

```
docker start app
```

* docker start 启动docker容器命令;
* app 要启动的容器名称，这里可以是容器的Id;

## 查看运行日志

```
docker logs app
```

## 停止正在运行的容器

```
docker stop app
```

* docker stop 停止docker容器命令;
* app 要停止的容器名称，这里可以是容器的Id;

## 删除容器

```
docker rm app   
docker rm -f app 

```
* -f表示强制删除容器，主要用于删除正在运行的容器,不加 “-f” 只能删除停止运行的容器;

## 进入容器SHELL
```
docker exec -it app bash //进入容器的shell
```

## 修改镜像名称

打新的tag（修改镜像名称）

```
docker tag imageid name:tag //修改镜像名称
```
## 删除镜像

```
docker rmi fd484f19954f //删除docker镜像
```

## 私有镜像仓库操作

```
#docker tag
docker tag 545da14ea 127.0.0.1/registry/app:1.0

#登录到私有仓库
docker login 127.0.0.1
UserName:admin
Password:123456
Email:111111@qq.com

#push到私有仓库
dokcer push 127.0.0.1/registry/app:1.0

#从私有仓库pull一个镜像
docker pull 127.0.0.1/registry/app:1.0
```



