# docker网络说明

Docker 官方文档对 docker 的几种网络驱动做了介绍，并分别给出了使用方法，先看一下下面摘录自官方文档的内容。

**Network drivers** Docker’s networking subsystem is pluggable, using drivers. Several drivers exist by default, and provide core networking functionality:

- bridge: The default network driver. If you don’t specify a driver, this is the type of network you are creating. Bridge networks are usually used when your applications run in standalone containers that need to communicate. See bridge networks.
- host: For standalone containers, remove network isolation between the container and the Docker host, and use the host’s networking directly. See use the host network.
- overlay: Overlay networks connect multiple Docker daemons together and enable swarm services to communicate with each other. You can also use overlay networks to facilitate communication between a swarm service and a standalone container, or between two standalone containers on different Docker daemons. This strategy removes the need to do OS-level routing between these containers. See overlay networks.
- ipvlan: IPvlan networks give users total control over both IPv4 and IPv6 addressing. The VLAN driver builds on top of that in giving operators complete control of layer 2 VLAN tagging and even IPvlan L3 routing for users interested in underlay network integration. See IPvlan networks.
- macvlan: Macvlan networks allow you to assign a MAC address to a container, making it appear as a physical device on your network. The Docker daemon routes traffic to containers by their MAC addresses. Using the macvlan driver is sometimes the best choice when dealing with legacy applications that expect to be directly connected to the physical network, rather than routed through the Docker host’s network stack. See Macvlan networks.   Macvlan 网络允许您为容器分配 MAC 地址（不指定则自动生成），使其在您的网络上显示为物理设备（相当于和普通物理机接在了同一个交换机上的效果，可以直接通讯）。Docker 守护进程通过容器的 MAC 地址将流量路由到容器。在处理希望直接连接到物理网络而不是通过Docker主机的网络堆栈路由的应用程序时，使用macvlan驱动程序有时是最佳选择。请参阅Macvlan网络。
- none: For this container, disable all networking. Usually used in conjunction with a custom network driver. none is not available for swarm services. See disable container networking.
- Network plugins: You can install and use third-party network plugins with Docker. These plugins are available from Docker Hub or from third-party vendors. See the vendor’s documentation for installing and using a given network plugin.

**Network driver summary**

- 当您需要多个容器在同一个 Docker 主机上进行通信时，用户定义的桥接网络是最佳选择。User-defined bridge networks are best when you need multiple containers to communicate on the same Docker host.
- 当网络堆栈不应与 Docker 主机隔离，但您希望容器的其他方面被隔离时，host 网络是最佳选择。 Host networks are best when the network stack should not be isolated from the Docker host, but you want other aspects of the container to be isolated.
- 当您需要让运行在不通过主机上的容器进行通信时，或者当多个应用程序使用 swarm 服务协同工作时，overlay 网络是最佳选择。 Overlay networks are best when you need containers running on different Docker hosts to communicate, or when multiple applications work together using swarm services.
- 当您需要容器看起来像网络上的物理主机时（从网络角度让 docker 普通主机一样），macvlan 网络是最佳选择，每个主机都有唯一的 MAC 地址。 Macvlan networks are best when you are migrating from a VM setup or need your containers to look like physical hosts on your network, each with a unique MAC address.
- 第三方网络插件允许您将 Docker 与专门的网络堆栈集成。 Third-party network plugins allow you to integrate Docker with specialized network stacks.

我们一般比较常见常用的就是 bridge（默认模式）和 host 两种，overlay 在大型集群中（例如K8S）使用较多，none 一般用来跑不需要任何网络交互的单机容器程序。其中 macvlan 是一种比较独特的模式，它比较适合一些有跨主机通讯需求且主机又不太多的中小型系统（你想做一个 docker 版的 openwrt 旁路由它就特别适合你）。

**特别提醒**

- 以上的几种网络驱动类型，为 docker 的 network 类型，和 docker run 参数中 --network 指定的网络名称是不同的，不要混淆了。
- docker 运行指定的是当前已经存在的网络名称，默认包括：host、bridge、none 三种（安装docker后就默认有这3个），另外还可以使用 container:NAME_or_ID 和 自定义创建的network名称（docker network ls 可以查看）。示例：①docker run --network=host、②docker run --network=none、③docker run --network=container:mysql、④docker run --network=mymacvlan1、⑤docker run --network=bridge。如果不指定 --network 参数则默认值为 bridge。

**macvlan 网络结构示范**

![在这里插入图片描述](https://img-blog.csdnimg.cn/17b8f9688c5a4fdab9de66eb4d6fcd4e.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAY2F0b29w,size_20,color_FFFFFF,t_70,g_se,x_16)

**注意事项** 进行操作之前，需要先清楚以下事项：

- 主机物理网卡必须支持且能开启混杂模式，可以通过下面的方法开启混杂模式，并确认是否已经开启。
- docker容器创建时会自动生成mac地址，加上docker容器很容易被我们日常性铲除重建，所以随着次数增多，会导致网络中存在大量的唯一MAC地址，可能对现有网络产生影响。所以**建议在创建容器的时候手工指定MAC地址**。
- 如果在docker容器启动的时候不指定IP地址，则会被自动分配IP。当你为macvlan设置的网段和宿主机同一个网段的情况下，这个自动分配的IP并不是从路由器DHCP服务获取的，所以很容易出现IP冲突问题，**故强烈建议当macvlan的网段和宿主机网段同网段时候要手工指定一个没有给宿主机网络已经占用的IP地址**。
- 如果 bridge 和 overlay 网络方式已经能较好的解决你的问题，那么则不建议你使用 macvlan，不是 macvlan 不好，只是在都可以解决问题的情况下请优先选择前两者。
- **默认创建完成 macvlan 后，当前宿主机和当前宿主机中的docker 容器，他们之间是无法互相访问的。需要手工使用 ip link 命令将 macvlan 和宿主机的网卡连接起来**

**混杂模式配置**

```bash
# 查看网卡信息，不存在PROMISC信息则说明网卡为开启混杂模式
[root@localhost test]# ip a |grep ens192
2: ens192: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc mq state UP group default qlen 1000
    inet 192.168.1.30/22 brd 192.168.3.255 scope global noprefixroute dynamic ens192

# 开启混杂模式
[root@localhost test]# ifconfig ens192 promisc

# 再查看网卡信息，发现已经有了PROMISC属性，表示混杂模式已开启
[root@localhost test]# ip a |grep ens192
2: ens192: <BROADCAST,MULTICAST,PROMISC,UP,LOWER_UP> mtu 1500 qdisc mq state UP group default qlen 1000
    inet 192.168.1.30/22 brd 192.168.3.255 scope global noprefixroute dynamic ens192
```

**macvlan 的使用** 1、使用桥接模式创建一个macvlan

```bash
docker network create -d macvlan \\
  --subnet=192.168.1.0/22 \\
  --ip-range=192.168.1.128/25 \\
  --gateway=192.168.1.1 \\
  --aux-address="my-router=192.168.1.129" \\
  -o parent=ens192 macvlan1
```

参数解释：

- [必须] -d macvlan 为固定值，-d 为 --driver 的简写
- [必须] subnet 设定网段和子网掩码
- [可选] ip-range 代表自动分配IP地址范围
- [必须] gateway 指定网关
- [可选] aux-address 代表从自动分配的IP地址范围中排除掉的IP列表
- [必须] -o parent=ens192，其中 ens192 为你的物理网卡名称，例如 ens33，ens192，eth0 等
- [必须] 最后的 macvlan1 是创建的 macvlan 的名称，自己定义即可

查看docker 所有 network，可见刚刚创建的 macvlan1

```bash
[root@localhost ~]# docker network ls
NETWORK ID     NAME       DRIVER    SCOPE
a6b34821866b   bridge     bridge    local
40c99b4832e1   host       host      local
90e2dad78e12   macvlan1   macvlan   local
bc640d2855ef   none       null      local
```

2、创建一个 docker 容器，指定 IP 和 mac

```bash
docker run -itd --network=macvlan1 \\
  --ip=192.168.1.230 \\
  --mac-address=D0:EE:6A:1D:3C:62 \\
  --name=helloworld-nodejs \\
  xzxiaoshan/helloworld-nodejs:latest
```

```
如果出现警告 WARNING: IPv4 forwarding is disabled. Networking will not work.，则编辑 /usr/lib/sysctl.d/00-system.conf 或 /etc/sysctl.conf，配置文件中追加参数 net.ipv4.ip_forward=1 后重启网络 systemctl restart network && systemctl restart docker（可以使用命令 sysctl net.ipv4.ip_forward 验证修改是否已经生效）。
```

其中 --network 指定刚刚创建的 macvlan 网络名称 macvlan1，IP 和 mac 为主动分配给 docker 的 IP 和 MAC，上文做过详细解释，虽然不是必须项，但是推荐手工指定（实际依自己需求而定）。IP 地址按照 第1步 中的网段指定，MAC 地址可以在线生成一个。

**划重点：**

- 还有一个重点需要检查确认（Linux 服务器为物理机除外）。就是如果你这个运行 docker 的 Linux 服务器是 ESXi 上的一个虚拟机，那么你必须要到 ESXi 上为这个个虚拟机所连接的虚拟交换机开启混杂模式，否则你啥都ping不通也连不上。
- 默认情况下 docker 所在宿主机与当前宿主机之间是无法互相 ping 通的，但是在其他同网络机器上是可以 ping 通的，要想解决宿主机和自身容器这个网络互相访问问题。还需要手工将 macvlan 和 主机的网卡 link 起来（下面有代码示例，这是重点）。

然后在同网段任意电脑上浏览器访问 http://192.168.1.230:8080 就可以看到这个 helloworld 测试页面了（这个测试镜像是8080端口）。

如果你想更清楚的查看 macvlan1 的信息，可以使用命令 docker network inspect macvlan1 查看更多信息。

**连接macvlan和宿主机网卡**

```bash
ip link add macvlan_host link ens192 type macvlan mode bridge
ip addr add 192.168.1.233/22 dev macvlan_host
# ifconfig macvlan_host up 或 ip link set macvlan up 
ifconfig macvlan_host up
# 配置静态路由使宿主机可以访问容器（可以使用具体的IP或者IP段）
ip route add 192.168.1.230 dev macvlan_host
```

其中 macvlan_host 为我们创建的 macvlan 的名称，type 为 macvlan，mode 为 bridge，ip 地址为同网段一个没有被占用的 ip（子网掩同macvlan）。

 `查看 ip link 列表命令，示例：ip link list 删除已经创建的 link 命令，示例：ip link delete macvlan_host`



以上配置后，在宿主机上就可以通过容器ip直接访问了，但是容器中依旧不能正常访问宿主机的，容器需要访问宿主机，通过宿主机macvlan的虚拟IP（192.168.1.233）访问，或者添加 iptables 规则，将宿主机IP和虚拟IP对应上，如下命令所示：

```bash
# 192.168.1.30 是宿主机的 IP，192.168.1.233 是宿主机 macvlan 的虚拟IP
iptables -t nat -A OUTPUT -d 192.168.1.30 -j DNAT --to-destination 192.168.1.233
```

因为macvlan 的IP范围和路由器的DHCP不相干，所以建议在路由器上设置一下DHCP的范围，避免造成ip冲突。

`注：macvlan 不提供 dns 服务，所以默认情况下在基于 macvlan 的容器中是无法进行 dns 解析的，你可以通过挂载 /etc/resolv.conf 文件或其他方式来解决 dns 解析问题。`

**ipvlan** 还有一种类似 macvlan 的技术，多张虚拟网卡共享相同 MAC 地址，但有独立的 IP 地址，它就是 ipvlan。

ipvlan 和 macvlan 类似，都是从一个主机接口虚拟出多个虚拟网络接口。一个重要的区别就是所有的虚拟接口都有相同的 mac 地址，而拥有不同的 ip 地址。因为所有的虚拟接口要共享 mac 地址，所以有需要注意的地方：

- DHCP 协议分配 ip 的时候一般会用 mac 地址作为机器的标识。这个情况下，客户端动态获取 ip 的时候需要配置唯一的 ClientID 字段，并且 DHCP server 也要正确配置使用该字段作为机器标识，而不是使用 mac 地址
- ipvlan 是 linux kernel 比较新的特性，linux kernel 3.19 开始支持 ipvlan，但是比较稳定推荐的版本是 >=4.2（因为 docker 对之前版本的支持有 bug，如果在内核小于 4.2，在创建 ipvlan 的时候会报错提示版本问题）。

如下为一个常规的 ipvlan 的示例：

```bash
docker network create -d ipvlan \\
  --subnet=192.168.1.0/22 \\
  --gateway=192.168.1.1 \\
  -o ipvlan_mode=l2 \\
  -o parent=ens192 ipvlan1

docker run -itd --network=ipvlan1 \\
  --ip=192.168.1.230 \\
  --name=helloworld-nodejs \\
  xzxiaoshan/helloworld-nodejs:latest
```

添加 ip link（你需要和宿主机网络互通才操作）

```bash
ip link add ipvlan1 link ens192 type ipvlan mode l2
ip addr add 192.168.1.231/24 dev ipvlan1
ifconfig ipvlan1 up
```

关于 ipvlan 的详解本文不展开，更多内容可以详见官网。

### **docker容器的网络**

**docker run**创建Docker容器时，可以用--net选项指定容器的网络模式，Docker有以下4种网络模式：

- bridge模式：使--net =bridge指定，默认设置；
- host模式：使--net =host指定；
- none模式：使--net =none指定；
- container模式：使用--net =container:NAME orID指定。

**docker 容器的网络基础**

**1、docker0：**

安装docker的时候，会生成一个docker0的虚拟网桥

**2、Linux虚拟网桥的特点：**

可以设置ip地址

相当于拥有一个隐藏的虚拟网卡

```
 docker0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue state UP group default
 
   link/ether 02:42:28:ae:c0:42 brd ff:ff:ff:ff:ff:ff
 
   inet 172.17.0.1/16 brd 172.17.255.255 scope global docker0
 
 •    valid_lft forever preferred_lft forever
 
 inet6 fe80::42:28ff:feae:c042/64 scope link
```

每运行一个docker容器都会生成一个veth设备对，这个veth一个接口在容器里，一个接口在物理机上。

**3、安装网桥管理工具：**

```
 yum install bridge-utils -y
```

brctl show 可以查看到有一个docker0的网桥设备，下面有很多接口，每个接口都表示一个启动的docker容器，因为我在docker上启动了很多容器，所以interfaces较多，如下所示：

**docker 容器的互联**

下面用到的镜像的dockerfile文件如下：

```
 cd dockerfile/inter-image
 
 vim dockerfile
 
 FROM centos
 
 RUN rm -rf /etc/yum.repos.d/\\*
 
 COPY Centos-vault-8.5.2111.repo /etc/yum.repos.d/
 
 RUN yum install wget -y
 
 RUN yum install nginx -y
 
 RUN sed -i "7s/^/#/g" /etc/nginx/conf.d/default.conf
 
 EXPOSE 80
 
 CMD /bin/bash
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
 curl <http://172.17.0.20>
```

可以访问到test1容器的内容

上述方法假如test1容器重启，那么在启动就会重新分配ip地址，所以为了使ip地址变了也可以访问可以采用下面的方法

**docker link设置网络别名**

**第二种方法：**

可以给容器起一个代号，这样可以直接以代号访问，避免了容器重启ip变化带来的问题

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

**docker容器的网络模式**

**docker run创建docker容器时，可以用--net选项指定容器的网络模式，Docker有以下4种网络模式**：

- bridge模式：使--net =bridge指定，默认设置；
- host模式：使--net =host指定；
- none模式：使--net =none指定；
- container模式：使用--net =container:NAME orID指定。

**none模式**

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

**container模式**

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

**bridge模式**

默认选择bridge的情况下，容器启动后会通过DHCP获取一个地址

```
 #创建桥接网络
 [root@xianchaomaster1~]# docker run --name bridge -it --privileged=true centos bash
 

 
 [root@a131580fb605 /]# ip addr
 
 1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
 
   link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
 
   inet 127.0.0.1/8 scope host lo
 
 •    valid_lft forever preferred_lft forever
 
 64: eth0@if65: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue state UP group default
 
   link/ether 02:42:ac:11:00:0d brd ff:ff:ff:ff:ff:ff link-netnsid 0
 
   inet 172.17.0.13/16 brd 172.17.255.255 scope global eth0
 
 •    valid_lft forever preferred_lft forever
```

**host模式**

**Docker网络host模式是指共享宿主机的网络**

```
 #共享宿主机网络
 
 [root@xianchaomaster1~]# docker run --name host -it --net=host --privileged=true centos bash
 
 
```