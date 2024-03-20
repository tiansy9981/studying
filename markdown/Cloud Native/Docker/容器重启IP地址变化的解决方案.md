# 容器重启IP地址变化的解决方案

创建了两个容器，做好mysql的主从配置了，重启docker容器之后，发现容器的ip地址变了，这就尴尬了，首先了解到了docker默认采用”bridge”连接，启动容器的时候会按照顺序来获取ip。这就导致启动时候ip不固定的问题， 下面创建自定义网络来解决这个IP不固定的问题， 1.创建自定义网络，指定网段172.17.0.0/16

```
 docker network create --subnet=172.19.0.0/16 bind
```

2.创建容器

```
 docker run -itd --name mysql-master-172.19.0.103  --net bind --ip 172.19.0.103 centos:latest /usr/sbin/init
 docker run -itd --name mysql-slave-172.19.0.104  --net bind --ip 172.19.0.104 centos:latest /usr/sbin/init
```

3.查看两个容器IP：

重启之后查看，发现IP并无变化

下面分享两个个docker批量命令：

```
 #杀死所有正在运行的容器:
 docker kill `docker ps -aq`
 #删除所有停止的容器:
 docker rm `docker ps -aq`
 #强制删除所有的容器 ：
 docker rm -f `docker ps -aq`
```