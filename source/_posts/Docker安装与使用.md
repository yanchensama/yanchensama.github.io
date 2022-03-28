---
title: Docker安装与使用
tags:
  - docker
  - linux
categories:
  - Docker
  - Linux
abbrlink: b955ee64
date: 2022-03-22 20:58:55
---

## 概念

docker就是不包括内核的虚拟机，在原内核基础上，由docker进程控制一个个容器，里面装着环境（app+lib）

![image-20220322234108919](../assets/img/Docker安装与使用/image-20220322234108919.png)

镜像就像是iso文件，容器就像是虚拟机

Docker倡导 的一个容器一个进程的原则。为了尽量的轻量快捷建议一个容器尽量只有一个进程，即不建议ssh连接（需要sshd进程）

**一次性** ：容器本身是一个无状态，用后即焚的东西，创建后应该是被视为不可改变的

## 安装Docker

> docker 分为 docker-ce（原docker）的社区版和docker-ee（原docker -engine）

> centos8 下yum install docker命令 下的不是docker，而是redhat出版的类似docker的podman（好不好用我不说，但是很狗就是了）

不同版本方法不同

centos8:

```shell
curl https://download.docker.com/linux/centos/docker-ce.repo -o /etc/yum.repos.d/docker-ce.repo
yum install docker-ce -y
```

## 启动服务

systemctl  stop， start ，restart  docker，老三样

## 镜像管理

- 搜索镜像(按start排的)

  ```
  docker search xxx
  ```

- 下载镜像

  ```
  docker pull XXX
  ```

  - 下是不可能下得动的，得换源

    ```
    cd /etc/docker
    vim daemon.json
    ```

    ```
    {
      "registry-mirrors": [
        "https://hub-mirror.c.163.com",
        "https://ustc-edu-cn.mirror.aliyuncs.com",
        "https://ghcr.io",
        "https://mirror.baidubce.com"
      ]
    }
    ```

    ```
    service docker restart
    ```

- 查看已有镜像

  ```
  docker images
  ```

- 显示完整镜像id

  ```
  docker images --no-trunc
  ```

- 移除本地镜像

  ```
  docker rmi [镜像id...]
  ```

- 镜像文件路径：`/var/lib/docker/containers`

## 容器管理

- 新建并启动容器 ： 80 为本地映射端口  (-d 参数后台运行)

  ```shell
  docker run -it --name webdemo -p 80:8080 tomcat /bin/bash
  ```

- 进入正在运行的容器

  ```
  1.命令格式一：docker exec -it [容器名/id] /bin/bash，（好！，起新进程连接，就像是ssh，生产环境最常用这个，exit退的是自己）
  2.命令格式二：docker attach [容器名/id]（不好！，相当于投屏输出，exit退的是容器（ssh连接除外，尽管不推荐ssh连接，很不专业））
  ```

- 查看正在运行的容器

  ```
  docker ps 		
  ```

- 传递文件

  - 向容器内传

    ```
    docker cp XXX.war  [容器名/id]:/usr/local/tomcat/webapps
    ```

  - 向容器外传

    ```
    docker cp [容器名/id]:/usr/local/tomcat/webapps/xxx  .  
    ```

- 启动容器

  ```
  docker start [容器名/容器id]
  ```

- 重启容器

  ```
  docker restart [容器名/容器id]
  ```

- 停止容器

  ````
  docker stop [容器名/容器id]
  docker kill [容器名/容器id] #强制
  ````

- 退出容器(交互式)

  ```
  1.容器停止并退出：exit 
  
  2.容器不停止退出: ctrl+P+Q(非ssh的attach连接只能以这种方式退出)
  ```

- 删除容器

  ```
  docker rm [容器名/容器id]
  ```

## 生成镜像

- 根据某个容器id生成镜像

```
docker commit [容器名/容器id]   name:version
```

- build

```
 docker build -t name:version
# -t : tag标记镜像名和状态
```

- 

## 多容器协作

docker compose是由docker官方制作的，需要单独下载

```
sudo curl -L https://github.com/docker/compose/releases/download/1.27.4/docker-compose-`uname -s`-`uname -m` > /usr/local/bin/docker-compose

# 国内用户可以使用以下方式加快下载
sudo curl -L https://download.fastgit.org/docker/compose/releases/download/1.27.4/docker-compose-`uname -s`-`uname -m` > /usr/local/bin/docker-compose

sudo chmod +x /usr/local/bin/docker-compose
```

它允许用户通过一个单独的 `docker-compose.yml` 模板文件（YAML 格式）来定义一组相关联的应用容器为一个项目（project）。

例如：web服务容器，和数据库服务容器，有效分离应用逻辑和数据的有效分离

编辑docker_compose.yml 文件

```yml
version: '3'

services:
  web: 
    build: .
    ports:
      - "80:8080"
  db:
    image: "mysql"
    environment:
      MYSQL_DATABASE: dbname
      MYSQL_ROOT_PASSWD: psd
    volumes:
      - my-volumesname:/var/lib/mysql
volumes:
   my-volumesname: 
```

使用

```
docker compose up -d  #启动所有容器
docker compose down   #停止并删除所有容器
docker compose down --volumes  #停止并删除所有容器及数据卷
```

## 部署

- #### 传统方式

  将jar包和Dockerfile放到服务器上同一个文件夹，build，run

- #### idea快速连接

  - ```
    vim /usr/lib/systemd/system/docker.service
    ```

  - 添加以下内容（注释原ExecStart）

    ```
    ExecStart=/usr/bin/dockerd -H tcp://0.0.0.0:2375 -H unix:///var/run/docker.sock 
    ```

  - idea `settings->docker` (连网服务器CA证书最好有)

    ![image-20220324094002427](../assets/img/Docker安装与使用/image-20220324094002427.png)
  - CA证书

    - 创建个CA文件夹

    ```
    mkdir -p /usr/local/ca
    cd /usr/local/ca/
    ```

    - 生成CA公钥和私钥
    
    ```
    openssl genrsa -aes256 -out ca-key.pem 4096 #创建密码
    
    openssl req -new -x509 -days 365 -key ca-key.pem -sha256 -out ca.pem #依次输入密码、国家、省、市、组织名称等
    
    openssl genrsa -out server-key.pem 4096
    
    openssl req -subj "/CN=$HOST" -sha256 -new -key server-key.pem -out server.csr
    
    echo subjectAltName = IP:121.40.176.56,IP:0.0.0.0 >> extfile.cnf
    
     echo extendedKeyUsage = serverAuth >> extfile.cnf
     
     openssl x509 -req -days 365 -sha256 -in server.csr -CA ca.pem -CAkey ca-key.pem \-CAcreateserial -out server-cert.pem -extfile extfile.cnf
    
    openssl genrsa -out key.pem 4096
    
    openssl req -subj '/CN=client' -new -key key.pem -out client.csr
    
    echo extendedKeyUsage = clientAuth >> extfile.cnf
    
    openssl x509 -req -days 365 -sha256 -in client.csr -CA ca.pem -CAkey ca-key.pem \-CAcreateserial -out cert.pem -extfile extfile.cnf
    
    rm -v client.csr server.csr
    
    chmod -v 0400 ca-key.pem key.pem server-key.pem
    
    chmod -v 0444 ca.pem server-cert.pem cert.pem
    
    cp server-*.pem  /etc/docker/
    
    cp ca.pem /etc/docker/
    
    vim /lib/systemd/system/docker.service
    ```
    
  
  ![image-20220324120749266](../assets/img/Docker安装与使用/image-20220324120749266.png)
  
  ```
            --tlsverify --tlscacert=/etc/docker/ca.pem \
            --tlscert=/etc/docker/server-cert.pem \
            --tlskey=/etc/docker/server-key.pem \
            -H tcp://0.0.0.0:2375 -H unix:///var/run/docker.sock 
  ```
  
  ```
  systemctl daemon-reload 
  systemctl restart docker
  
  # scp  ca.pem cert.pem key.pem 到本地docker-ca文件夹
  ```
  
  ![image-20220324121014404](../assets/img/Docker安装与使用/image-20220324121014404.png)完成

## 补充：

要修改端口映射，三个方法：1.删了重建重启动，2.旧docker commit到新启动 ，3该配置，最麻烦的
