# Docker容器持久化存储

## Docker容器的数据卷

### 什么是数据卷？

**数据卷是经过特殊设计的目录，可以绕过统一文件系统（UFS），为一个或者多个容器提供访问，数据卷设计的目的，在于数据的永久存储，它完全独立于容器的生存周期，因此，docker不会在容器删除时删除其挂载的数据卷，也不会存在类似的垃圾收集机制，对容器引用的数据卷进行处理，同一个数据卷可以只支持多个容器的访问。**

#### 数据卷的特点：

**1.**数据卷在容器启动时初始化，如果容器使用的镜像在挂载点包含了数据，这些数据会被拷贝到新初始化的数据卷中

**2.**数据卷可以在容器之间共享和重用

**3.**可以对数据卷里的内容直接进行修改

**4.**数据卷的变化不会影像镜像的更新

**5.**卷会一直存在，即使挂载数据卷的容器已经被删除

#### 数据卷的使用：

1. 为容器添加数据卷

```bash 
docker run -v /datavolume:/data -it centos /bin/bash
```

**如：**

```bash
docker run --name volume -v ~/datavolume:/data -itd centos /bin/bash
```

**注：~/datavolume为宿主机目录，/data为docker启动的volume容器的里的目录**

这样在宿主机的/datavolume目录下创建的数据就会同步到容器的/data目录下

**为数据卷添加访问权限**

```bash
docker run --name volume1 -v ~/datavolume1:/data:ro -itd centos /bin/bash
```

添加只读权限之后在docker容器的/data目录下就不能在创建文件了，为只读权限；在宿主机下的/datavolume1下可以创建东西

2. 使用dockerfile构建包含数据卷的镜像

```xml
dockerfile指令：

volume[“/data”]

cat  dockerfile

FROM centos

VOLUME ["/datavolume3","/datavolume6"]

CMD /bin/bash
```

**使用如下构建镜像**

```bash 
docker build -t="volume" .
```

**启动容器**

`docker run --name volume-dubble -it volume`

会看到这个容器下有两个目录，/datavolume3和/datavolume6

### **Docker的数据卷容器**

#### 什么是数据卷容器：

**命名的容器挂载数据卷，其他容器通过挂载这个容器实现数据共享，挂载数据卷的容器，就叫做数据卷容器**

**挂载数据卷容器的方法**

`docker run --volumes-from [container name]`

**例：**

`docker run --name data-volume -itd volume`

（volume这个镜像是上面创建的带两个数据卷/datavolume3和/ddatavolume6的镜像） 

`docker exec -it data-volume /bin/bash`

（进入到容器中）

`touch /datavolume6/lucky.txt`

退出容器

`exit`

**创建一个新容器挂载刚才data-volume这个容器创建的数据卷**

`docker run --name data-volume2 --volumes-from data-volume -itd centos /bin/bash`

**进入到新创建的容器**

`docker exec -it data-volume2 /bin/bash`

**查看容器的/datavolume6目录下是否新创建了lucky.txt文件**

```bash
cd /datavolume6
```

**可以看见有刚才在上一个容器创建的文件lucky.txt**

#### docker数据卷的备份和还原

**数据备份方法：**

```
docker run --volumes-from [container name] -v $(pwd):/backup centos tar czvf /backup/backup.tar [container data volume] 
```

**例子：**

```
docker run --volumes-from data-volume2 -v /root/backup:/backup --name datavolume-copy centos tar zcvf /backup/data-volume2.tar.gz /datavolume6
```

**数据还原方法：**

```
docker run --volumes-from [container name] -v $(pwd):/backup centos tar xzvf /backup/backup.tar.gz [container data volume] 
```

**例：**

```
docker exec -it data-volume2 /bin/bash

cd /datavolume6

rm -rf lucky.txt

 

docker run --volumes-from data-volume2 -v /root/backup/:/backup centos tar zxvf /backup/data-volume2.tar.gz -C /datavolume6

 

docker exec -it data-volum2 /bin/bash

cd /datavolum6
```

可以看到还原后的数据

# docker容器的网络

**docker run**创建Docker容器时，可以用--net选项指定容器的网络模式，Docker有以下4种网络模式：

- bridge模式：使--net =bridge指定，默认设置；
- host模式：使--net =host指定；
- none模式：使--net =none指定；
- container模式：使用--net =container:NAME orID指定。

## docker 容器的网络基础

### **1、docker0：**

安装docker的时候，会生成一个docker0的虚拟网桥

### **2、Linux虚拟网桥的特点：**

可以设置ip地址，相当于拥有一个隐藏的虚拟网卡

```
docker0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue state UP group default 

  link/ether 02:42:28:ae:c0:42 brd ff:ff:ff:ff:ff:ff

  inet 172.17.0.1/16 brd 172.17.255.255 scope global docker0

​    valid_lft forever preferred_lft forever

inet6 fe80::42:28ff:feae:c042/64 scope link 
```

每运行一个docker容器都会生成一个veth设备对，这个veth一个接口在容器里，一个接口在物理机上。

### **3、安装网桥管理工具：**

```
yum install bridge-utils -y 
```

brctl show 可以查看到有一个docker0的网桥设备，下面有很多接口，每个接口都表示一个启动的docker容器，因为我在docker上启动了很多容器，所以interfaces较多，如下所示：                

#### **docker 容器的互联**

下面用到的镜像的dockerfile文件如下：

```
cd dockerfile/inter-image

vim dockerfile

FROM centos

RUN rm -rf /etc/yum.repos.d/\*

COPY Centos-vault-8.5.2111.repo /etc/yum.repos.d/

RUN yum install wget -y

RUN yum install nginx -y

RUN sed -i "7s/^/#/g" /etc/nginx/conf.d/default.conf

EXPOSE 80

CMD /bin/bash
```

```
[root@xianchaomaster1]# docker build -t="inter-image" .
```

**允许所有容器间互联（也就是访问）**

**第一种方法：**

例：

（1）基于上面的inter-image镜像启动第一个容器test1

```
docker run --name test1 -itd inter-image
```

进入到容器里面启动nginx：

```
/usr/sbin/ngnx 
```

（2）基于上面的inter-image镜像启动第二个容器test2

```
docker run --name test2 -itd inter-image
```

（3）进入到test1容器和test2容器，可以看两个容器的ip，分别是172.17.0.20和172.17.0.21

```
docker exec -it test2 /bin/bash
```

ping 172.17.0.20 可以看见能ping同test1容器的ip

```
curl http://172.17.0.20  
```

可以访问到test1容器的内容 

上述方法假如test1容器重启，那么在启动就会重新分配ip地址，所以为了使ip地址变了也可以访问可以采用下面的方法

 

#### **docker link设置网络别名**

**第二种方法：**

可以给容器起一个代号，这样可以直接以代号访问，==避免了容器重启ip变化带来的问题==

```
--link

docker run --link=[CONTAINER_NAME]:[ALIAS] [IMAGE][COMMAND]
```

**例：**

1.启动一个test3容器

```
docker run --name test3 -itd inter-image /bin/bash 
```

2.启动一个test5容器，--link做链接，那么当我们重新启动test3容器时，就算ip变了，也没关系，我们可以在test5上ping别名webtest

```
docker run --name test5 -itd --link=test3:webtest inter-image /bin/bash
```

3.test3和test5的ip分别是172.17.0.22和172.17.0.24

4.重启test3容器

```
docker restart test3
```

发现ip变成了172.17.0.25

5.进入到test5容器

```
docker exec -it test5 /bin/bash
```

ping test3容器的ip别名webtest可以ping通，尽管test3容器的ip变了也可以通

###  4、**docker容器的网络模式**

**docker run创建docker容器时，可以用--net选项指定容器的网络模式，Docker有以下4种网络模式**：

- bridge模式：使--net =bridge指定，默认设置；

- host模式：使--net =host指定；

- none模式：使--net =none指定；

- container模式：使用--net =container:NAME orID指定。

####  **none模式**

**Docker网络none模式是指创建的容器没有网络地址，只有lo网卡**

```
[root@xianchaomaster1 ~]# docker run -itd --name none --net=none --privileged=true centos 

[root@xianchaomaster1 ~]# docker exec -it none /bin/bash

 

[root@05dbf3f2daaf /]# ip addr

#只有本地lo地址

 

1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000

  link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00

  inet 127.0.0.1/8 scope host lo

  valid_lft forever preferred_lft forever


```

####  **container模式**

**Docker网络container模式是指，创建新容器的时候，通过`--net container`参数，指定其和已经存在的某个容器共享一个 Network Namespace。如下图所示，右方黄色新创建的container，其网卡共享左边容器。因此就不会拥有自己独立的 IP，而是共享左边容器的 IP 172.17.0.2,端口范围等网络资源，两个容器的进程通过 lo 网卡设备通信。**

```
#和已经存在的none容器共享网络

[root@xianchaomaster1 ~]# docker run --name container2 --net=container:none -it --privileged=true centos

 

[root@05dbf3f2daaf /]# ip addr

1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000

  link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00

  inet 127.0.0.1/8 scope host lo

  valid_lft forever preferred_lft forever
```

####  **bridge模式**

默认选择bridge的情况下，容器启动后会通过DHCP获取一个地址

```
#创建桥接网络
[root@xianchaomaster1~]# docker run --name bridge -it --privileged=true centos bash

 

[root@a131580fb605 /]# ip addr

1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000

  link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00

  inet 127.0.0.1/8 scope host lo

​    valid_lft forever preferred_lft forever

64: eth0@if65: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue state UP group default 

  link/ether 02:42:ac:11:00:0d brd ff:ff:ff:ff:ff:ff link-netnsid 0

  inet 172.17.0.13/16 brd 172.17.255.255 scope global eth0

​    valid_lft forever preferred_lft forever 
```

#### **host模式**

**Docker网络host模式是指共享宿主机的网络**

```


#共享宿主机网络

[root@xianchaomaster1~]# docker run --name host -it --net=host --privileged=true centos bash


```

 

