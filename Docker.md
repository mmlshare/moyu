# Docker

## 安装

- 环境

  Centos7.6

- 脚本

  ```shell
  #确保 yum 包更新到最新
  sudo yum update
  
  #执行 Docker 安装脚本,执行这个脚本会添加 docker.repo 源并安装 Docker。
  curl -fsSL https://get.docker.com/ | sh
  #国内源
  curl -fsSL https://get.docker.com | bash -s docker --mirror Aliyun
  
  #启动 Docker 进程
  sudo service docker start
  
  #验证 docker 是否安装成功并在容器中执行一个测试的镜像
  sudo docker run hello-world
  
  # 开机启动
  sudo systemctl enable docker
  
  ```

## Docker下 Redis集群安装

[参考文章](https://juejin.im/post/6844904166469402637)

```shell

# 拉取镜像
docker pull redis:5.0 # 拉取tag为5.0的redis镜像。
# 查看具体版本
docker inspect redis:5.0

#创建存放redis集群相关配置文件和数据的文件夹
mkdir -p /home/docker/redis-cluster

# 创建集群配置 
for port in `seq 6601 6606`; do 
    mkdir -p ./${port}/conf 
    touch ./${port}/conf/redis.conf
    mkdir -p ./${port}/data
    echo "port ${port}" >>./${port}/conf/redis.conf
    echo "protected-mode no"  >>./${port}/conf/redis.conf
    echo "cluster-enabled yes" >>./${port}/conf/redis.conf
    echo "cluster-config-file nodes.conf" >>./${port}/conf/redis.conf
    echo "cluster-node-timeout 5000" >>./${port}/conf/redis.conf
    echo "cluster-announce-ip 127.0.0.1" >>./${port}/conf/redis.conf
    echo "cluster-announce-port ${port}" >>./${port}/conf/redis.conf
    echo "cluster-announce-bus-port 1${port}" >>./${port}/conf/redis.conf
    echo "appendonly yes" >>./${port}/conf/redis.conf
done

# Docker使用配置启动redis
for port in `seq 6601 6606`; do \
  docker run -d --net host --privileged=true \
  -v $PWD/${port}/conf/redis.conf:/usr/local/etc/redis/redis.conf \
  -v $PWD/${port}/data:/data \
  --restart always --name redis-${port} \
  redis:5.0 redis-server /usr/local/etc/redis/redis.conf; \
done
# 解释：
# --net host #指定容器使用host网络模式启动
# -v $PWD/${port}/conf/redis.conf:/usr/local/etc/redis/redis.conf # 把对应的配置文件映射到容器内
# --privileged=true #使容器内的root拥有真正的root权限
# redis-server /usr/local/etc/redis/redis.conf # 使用配置文件启动redis

# 进入其中一台redis
docker exec -it redis-6601 /bin/bash
cd /usr/local/bin
# 建立集群
redis-cli --cluster create 127.0.0.1:6601 127.0.0.1:6602 127.0.0.1:6603 127.0.0.1:6604 127.0.0.1:6605 127.0.0.1:6606 --cluster-replicas 1

```

## Docker下 MongoDb 4.4.0安装

```shell
# 拉取镜像
docker pull mongo

mkdir /home/docker/mongodb
#运行
docker run --name mongodb27017 -p 27017:27017 -e $PWD/db:/data/db -d mongo --auth

#进入mongo命令行
docker exec -it 1b7b83db9d2a mongo admin
#创建用户
db.createUser({ user:'admin',pwd:'123456',roles:[ { role:'userAdminAnyDatabase', db: 'admin'},"readWriteAnyDatabase"]});


```

## Docker下 RabbitMQ安装

```shell
# 拉取镜像，带管理控制台
docker pull rabbitmq:3-management
#启动
docker run -d --hostname rabbitmq --name rabbitmq-manager rabbitmq:3-management

# 进入容器
docker exec -it ea993761e522 /bin/bash
# 创建用户
rabbitmqctl add_user admin admin123
# 设置权限
rabbitmqctl set_permissions -p / admin ".*" ".*" ".*"
# 设置角色
rabbitmqctl set_user_tags admin administrator
```

## Docker下 GitLab安装

```shell
# 拉取镜像
docker pull gitlab/gitlab-ce

#启动
docker run \
    --publish 443:443 --publish 80:80 --publish 23:22 \
    --name gitlab \
    --volume /home/docker/gitlab/config:/etc/gitlab \
    --volume /home/docker/gitlab/logs:/var/log/gitlab \
    --volume /home/docker/gitlab/data:/var/opt/gitlab \
    gitlab/gitlab-ce
```

## Docker下Mysql安装

```shell
#拉取镜像
docker pull mysql

#启动
docker run --name mysql -p 3306:3306 -v $PWD/data:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=123456 -d mysql

#进入容器
docker exec -it mysql bash

#登录mysql
mysql -u root -p
ALTER USER 'root'@'localhost' IDENTIFIED BY '123456';

#添加远程登录用户
CREATE USER 'admin'@'%' IDENTIFIED WITH mysql_native_password BY '123456';
GRANT ALL PRIVILEGES ON *.* TO '123456'@'%';
```



## 关闭防火墙

```shell
systemctl stop firewalld # 关闭
systemctl disable firewalld #禁止开机启动
```



## Docker基本命令

```shell
# 查看正在运行的容器
docker container ls

# 查看所有容器
docker container ls -a

# 查看Docker下的进程
docker ps -a

# 移除
docker rm [id]

# 停止进程
docker stop [id]

# 启动进程
docker start [id]

# 杀死
docker kill [id]

#重启
docker restart [id]

#查看本地镜像列表
docker images

#日志查看
docker logs [id]
```

