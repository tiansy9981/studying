# LVS基本配置

## **LVS说明**

**【Linux操作系统核心空间中、一般采用DR构建集群】**

**小结：**Ipvsadm：管理集群服务的命令行工具、Ipvs：内核模块/代码**；**

## **三种负载均衡模式：**

**【NAT：修改IP、**双网卡，RIP指向DIP内网网关、任意操作系统】；

【**DR**直接路由：一个网卡、配置别名、DIP和RIP在同一网关上、修改MAC地址】**、*；

**【TUP隧道：**封装IP报文，异地服务连接**】**

## **LVS主要组成部分为**：

　　负载调度器：它是整个集群对外面的前端机，负责将客户的请求发送到一组服务器上执行，而客户认为服务是来自一个IP地址(虚拟IP地址VIP)上的。

　　服务器池(server pool/ Realserver)，是一组真正执行客户请求的服务器。

　　共享存储(shared storage)，它为服务器池提供一个共享的存储区。

## **安装包安装LVS【ipvsadm-1.26.tar.gz】**

下载ipvsadm：

依赖包：

```
yum install popt popt-devel popt-static  libnl libnl-devel  gcc lib* -y
```

```
wget http://www.linuxvirtualserver.org/software/kernel-2.6/ipvsadm-1.26.tar.gz
```

解压进入后安装：

```
make && make instell
```

安装路径：

```
which  ipvsadm ：/sbin/ipvsadm
```

 

## **LVS负载均衡方式:**

NAT模型：客户端：CIP-VIP–》LVS服务器：CIP-RIP–》Realserver:RIP-CIP–》LVS服务器：VIP-CIP【目标IP-源ＩＰ】

**NAT**是一种最简单的方式，所有的RealServer只需要将自己的网关指向Director即可。通过网络地址转换，调度器重写请求报文的目标地址，根据预设的调度算法，将请求分派给后端的真实服务器；真实服务器的响应报文通过调度器时，报文的源地址被重写，再返回给客户，完成整个负载调度过程。

**LVS NAT模式配置步骤【负载均衡只能是real机器】【配置RIP、VIP；访问DIP】**

**【DIP：**172.16.1.131**、RIP：** 172.16.1.135、172.16.1.136**、VIP：**192.168.50.129**】**

**特点和要求：**

1. LVS（Director）上面需要双网卡：**DIP(内网)和VIP（外网**）
2. 内网的Real Server主机的IP必须和DIP在同一个网络中，并且要求其**网关都需要指向DIP的地址**
3. RIP都是私有IP地址，仅用于各个节点之间的通信

4. Director位于client和Real Server之间，负载处理所有的进站、出站的通信

5. 支持端口映射
6. 通常应用在较大规模的应用场景中，但Director易成为整个架构的瓶颈！

Directorserver【双网卡】: 192.168.50.129 (对外提供服务的VIP),172.16.1.100【DIP内网】

Realserver1:【RIP1、内网】：192.168.50.135

Realserver2: 【RIP2、内网】192.168.50.136

 

#### **在RIP上安装httpd服务并测试**

```
yum install httpd –y  
```

网页内容：

```
echo web2 > /var/www/html/index.html
```

在DIP上安装ipvsadm：

```
yuminstall ipvsadm –y
```

开启网卡间的转发服务修改输入1：

```
echo 1 > /proc/sys/net/ipv4/ip_forward
```

在director主机上操作：

```
ipvsadm-A -t 192.168.50.129:80 -s rr  #【director主机】
```

```
ipvsadm-a -t 192.168.50.129:80 -r 172.16.1.135 -m  #【director主机+real-server1】
```

```
ipvsadm-a -t 192.168.50.129:80 –r 172.16.1..136 –m   #【director主机+real-server2】
```

在浏览器上输入192.168.50.129，轮询显示192.168.50.136/135的httpd服务的首页界面。

如果使用加权轮询的话；比如rs1提供2次，rs2提供1次，这样来提供服务；

```
ipvsadm-E -t 192.168.50.129:80 -s wrr

ipvsadm-e -t 192.168.50.129:80 -r 172.16.1.135 -m -w 2

ipvsadm-e -t 192.168.50.129:80 -r 172.16.1..136 -m -w 1
```

在浏览器输入DIP：172.16.1.100访问测试

#### 压力测试：

```
#【appache自带】

ab –n 1000 –c 100 [ http://192.168.50.129](http://192.168.50.129/)
```

查看状态：

```
Ipvsadm –L –n stats
```

保存规则 ：

```
service ipvsadm save  
```

保存具体位置：

```
ipvsadm -s >保存路径
```

 

**TUN模型IP隧道**：客户端：CIP-VIP–》LVS服务器DIP：DIP-CIP-VIP-RIP–》Realserver:CIP-VIP–》客户端【目标IP-源IP】

**TUN：IP隧道**(IP tunneling)是将一个IP报文封装在另一个IP报文的技术，这可以使得目标为一个IP地址的数据报文能被封装和转发到另一个IP地址。调度器把请求报文通过IP隧道转发至真实服务器，而真实服务器将响应直接返回给客户，所以调度器只处理请求报文。由于一般网络服务应答比请求报文大许多，采用 TUN技术后，集群系统的最大吞吐量可以提高10倍。【封装IP报文，异地服务连接】

DR模型：客户端：CIP-VIP–》LVS服务器DIP：CIP-VIP[DIPMAC-RIPMAC]–》Realserver:CIP-VIP–》客户端【目标IP-源IP】



**DR：直接路由**，方式是通过改写请求报文中的MAC地址部分来实现的。将请求发送到真实服务器，而真实服务器将响应直接返回给客户。这种方法没有IP隧道的开销，对集群中的真实服务器也没有必须支持IP隧道协议的要求，但是要求调度器与真实服务器都有一块网卡连在同一物理网段上。Director的VIP地址对外可见，而RealServer的VIP对外是不可见的，是别名。RealServer的地址即可以是内部地址，也可以是真实地址。

 

**通告/响应级别是可以通过linux内核级别来调整的【路径**/proc/sys/net/ipv4/conf/eth0或者eth1**】**

```
arp_ignore 接收到别人请求后的响应级别【共有8个值，重点是1】

【0：只要本地网络有此ip则直接通告；1：仅接收在本地接口的ip地址】
```

```
arp_annouce 主动向外通告过自己IP地址和MAC地址对应关系的通告级别【一般选择2】

0：默认值，将本地任何接口上的任何地址向外通告

1：试图仅向目标网络通告与其网络匹配的地址

2：仅向与本地接口上地址匹配的网络进行通告
```

因此，我们所需要的值就是：arp_ignore=1、arp_announce=2



**LVS DR模式配置步骤【都是一个网卡、配置别名、DIP和RIP在同一网关上，建议是桥接模式】**

**关闭网卡ifdown eth1，禁止网卡开机启动在ifcfgeth1里修改ONBOOT=no**

**只在DIP上安装LVS软件，Realserver上开启http服务即可**

**【DIP：**192.168.50.129、**RIP：**192.168.50.135、192.168.50.136、**VIP：**192.168.50.1**】**

配置director：eth0:192.168.50.129

配置别名VIP：

```
ifconfigeth0:0  192.168.50.1【VIP】broadcast【广播地址】 192.168.50.1【VIP】  netmask  255.255.255.255【只能确定在自身的局域网内广播】  up
```

添加转发路由：

```

route add -host 192.168.50.1【VIP】 dev eth0:0
```

 

配置realserver1：eth0：192.168.50.135【先改配置，在配置别名，添加转发路由】

修改配置文件：【逆序21】

```
echo 1 >/proc/sys/net/ipv4/conf/all/arp_ignore    #【realserver修改的数字要一样】

echo 1 >/proc/sys/net/ipv4/conf/eth0/arp_ignore 

echo 2 > /proc/sys/net/ipv4/conf/eth0/arp_announce

echo 2 >/proc/sys/net/ipv4/conf/all/arp_announce
```

配置别名VIP：

```
ifconfig lo:0 192.168.50.1  broadcast  192.168.50.1 netmask  255.255.255.255 up 
```

删除别名：

```
 ifconfig eth0:0 down
```

添加转发路由：

```
route add -host 192.168.50.1 dev lo:0
```

 

配置realserver2：eth0：192.168.50.135【先改配置，在配置别名，添加转发路由】

修改配置文件：

```
echo 1 >/proc/sys/net/ipv4/conf/all/arp_ignore   #【realserver修改的数字要一样】

echo 1 >/proc/sys/net/ipv4/conf/eth0/arp_ignore   

echo 2 > /proc/sys/net/ipv4/conf/eth0/arp_announce

echo 2 > /proc/sys/net/ipv4/conf/all/arp_announce

```

配置别名VIP：

```
ifconfig eth0:0  192.168.50.1  netmask  255.255.255.255  broadcast  192.168.50.1 up
```

添加转发路由：

```
route add -host 192.168.50.1 dev lo:0
```

 

在director主机上操作：

```
ipvsadm -A -t 192.168.50.1:80-s rr   #【director主机】

ipvsadm -a -t 192.168.50.1:80 -r192.168.50.135 -g –w 1

ipvsadm -a -t 192.168.50.1:80 –r 192.168.50.136–g –w 3
```

查看状态：

```
Ipvsadm –L –n stats
```

在浏览器输入DIP：192.168.50.128【VIP】访问测试

 

**注意：**

把自己的客户端IP添加到ipvsadm里面，当两台realserver服务宕机的时候提供说明界面。

```
ipvsadm-a -t 192.168.50.1:80 –r 127.0.0.1 –g –w 2
```



#### **【说明】**

```
ipvsadm：管理集群服务添加：-A -t|u|f service-address [-s scheduler【程序调度】]

-t:TCP协议的集群 、-u: UDP协议的集群、、-f: FWM: 防火墙标记 

service-address:IP:PORTservice-address: Mark Number

修改：-E

删除：-D -t|u|f service-address

管理集群服务中的RS

添加：-a -t|u|f service-address -r server-address [-g|i|m] [-w weight]

-t|u|fservice-address：事先定义好的某集群服务

-rserver-address: 某RS的地址，在NAT模型中，可使用IP：PORT实现端口映射；

[-g|i|m]:LVS类型  【-g: DR、-i: TUN、-m:NAT】

[-wweight]: 定义服务器权重

修改：-e

删除：-d -t|u|f service-address -r server-address

查看：-L|l【-n: 数字格式显示主机地址和端口、–stats：统计数据、–rate: 速率、–timeout: 显示tcp、tcpfin和udp的会话超时时长、-c: 显示当前的ipvs连接状况】

规则管理：删除所有集群服务-C：清空ipvs规则

保存规则-S ：# ipvsadm -S > /path/to/somefile

载入此前的规则：-R：# ipvsadm -R < /path/form/somefile
```

 

## **三种负载均衡方式比较：**

NAT:

集群节点跟director必须在同一个IP网络中；RIP通常是私有地址，仅用于各集群节点间的通信；director位于client和real server之间，并负责处理进出的所有通信；realserver必须将网关指向DIP；支持端口映射；realserver可以使用任意操作系统，RIP的默认网关是DIP的ip。

DR直接路由: 

集群节点跟director必须在同一个物理网络中；RIP可以使用公网地址，实现便捷的远程管理和监控；director仅负责处理入站请求，响应报文则由realserver直接发往客户端；realserver不能将网关指向DIP；**【MAC控制】**

TUNIP隧道：

远距离控制；RIP必须是公网地址；director仅负责处理入站请求，响应报文则由realserver直接发往客户端；realserver网关不能指向director；只有支持隧道功能的操作系统才能用于realserver；不支持端口映射；

 

# **IPVS调度器实现了如下10种负载调度算法：**

### 轮叫rr（Round Robin）

调度器通过”轮叫”调度算法将外部请求按顺序轮流分配到集群中的真实服务器上，它**均等地对待**每一台服务器，而不管服务器上实际的连接数和系统负载。

### 加权轮叫wrr（Weighted RoundRobin）

调度器通过”加权轮叫”调度算法根据真实服务器的不同处理能力来调度访问请求。这样可以保证处理能力强的服务器处理更多的访问流量。调度器可以自动问询真实服务器的负载情况，并动态地调整其权值。

### 目标地址散列调度dh（DestinationHashing）

“目标地址散列”调度算法根据请求的目标IP地址，作为散列键（Hash Key）从静态分配的散列表找出对应的服务器，若该服务器是可用的且未超载，将请求发送到该服务器，否则返回空。

### 源地址哈希sh（Source Hashing）

“源地址散列”调度算法根据请求的源IP地址，作为散列键（Hash Key）从静态分配的散列表找出对应的服务器，若该服务器是可用的且未超载，将请求发送到该服务器，否则返回空。

### 最少链接lc（LeastConnections）

调度器通过”最少连接”调度算法动态地将网络请求调度到已建立的链接数最少的服务器上。如果集群系统的真实服务器具有相近的系统性能，采用”最小连接”调度算法可以较好地均衡负载。

### **加权最少链接wlc**（Weighted LeastConnections）【**默认**】

在集群系统中的服务器性能差异较大的情况下，调度器采用”加权最少链接”调度算法优化负载均衡性能，具有较高权值的服务器将承受较大比例的活动连接负载。调度器可以自动问询真实服务器的负载情况，并动态地调整其权值。

### 基于局部性的最少链接LBLC（Locality-BasedLeast Connections）

“基于局部性的最少链接” 调度算法是针对目标IP地址的负载均衡，目前主要用于Cache集群系统。该算法根据请求的目标IP地址找出该目标IP地址最近使用的服务器，若该服务器是可用的且没有超载，将请求发送到该服务器；若服务器不存在，或者该服务器超载且有服务器处于一半的工作负载，则用”最少链接”的原则选出一个可用的服务器，将请求发送到该服务器。

### 带复制的基于局部性最少链接LBLCR（Locality-BasedLeast Connections with Replication）

带复制的基于局部性最少链接”调度算法也是针对目标IP地址的负载均衡，目前主要用于Cache集群系统。它与LBLC算法的不同之处是它要维护从一个目标IP地址到一组服务器的映射，而LBLC算法维护从一个目标IP地址到一台服务器的映射。该算法根据请求的目标IP地址找出该目标IP地址对应的服务器组，按”最小连接”原则从服务器组中选出一台服务器，若服务器没有超载，将请求发送到该服务器，若服务器超载；则按”最小连接”原则从这个集群中选出一台服务器，将该服务器加入到服务器组中，将请求发送到该服务器。同时，当该服务器组有一段时间没有被修改，将最忙的服务器从服务器组中删除，以降低复制的程度。

### NQ永不排队/最少队列调度（NeverQueue Scheduling NQ）

　 无需队列。如果有台 realserver的连接数＝0就直接分配过去，不需要再进行sed运算，保证不会有一个主机很空间。在SED基础上无论+几，第二次一定给下一个，保证不会有一个主机不会很空闲着，不考虑非活动连接，才用NQ，SED要考虑活动状态连接，对于DNS的UDP不需要考虑非活动连接，而httpd的处于保持状态的服务就需要考虑非活动连接给服务器的压力。

### SED：最短延迟调度（Shortest Expected Delay ）

　 在WLC基础上改进，Overhead = （ACTIVE+1）*256/加权，不再考虑非活动状态，把当前处于活动状态的数目+1来实现，数目最小的，接受下次请求，+1的目的是为了考虑加权的时候，非活动连接过多缺陷：当权限过大的时候，会倒置空闲服务器一直处于无连接状态。

 

#### **LVS的调度算法分为静态与动态两类。**

1.静态算法（4种）：只根据算法进行调度而不考虑后端服务器的实际连接情况和负载情况【rr、wrr、dh、sh】

2.动态算法（6种）：前端的调度器会根据后端真实服务器的实际连接情况来分配请求

【lc、**wlc【加权最少连接】**、sed、nq、LBLC、LBLCR】

默认方法：wlc



## **LVS集群的特点**

1. 功能：有实现三种IP负载均衡技术和10种连接调度算法的IPVS软件。
2. 适用性：后端服务器可运行任何支持TCP/IP的操作系统，负载调度器能够支持绝大多数的TCP和UDP协议
3. 无需对客户机和服务器作任何修改，可适用大多数Internet服务。
4. 性能：LVS服务器集群系统具有良好的伸缩性，可支持几百万个并发连接。配置100M网卡，采用VS/TUN或VS/DR调度技术，集群系统的吞吐量可高达1Gbits/s；如配置千兆网卡，则系统的最大吞吐量可接近10Gbits/s。
5. 可靠性：LVS服务器集群软件已经在很多大型的、关键性的站点得到很好的应用，所以它的可靠性在真实应用得到很好的证实。
6. 软件许可证：LVS集群软件是按GPL（GNU Public License）许可证发行的自由软件



## **【LVS的DR模式自动化脚本】Director脚本:**

### 【配置别名，添加规则】

```shell
#!/bin/bash

#LVS script for VS/DR

./etc/rc.d/init.d/functions #【调用公式之类的】

#定义变量

VIP=192.168.0.210

RIP1=192.168.0.221

RIP2=192.168.0.222

PORT=80  #【端口】

case”$1″ in

start)

/sbin/ifconfigeth0:1 $VIP broadcast $VIP netmask 255.255.255.255 up #【配置别名VIP】 

/sbin/routeadd -host $VIP dev eth0:1  #【添加路由VIP】

echo1 > /proc/sys/net/ipv4/ip_forward  #【开启转发设置】

/sbin/iptables-F  #【Clearall iptables rules.】  #【清空规则链】

/sbin/iptables–Z  #【Resetiptables counters.】  #【计算器清空】

/sbin/ipvsadm –C    #【Clear all ipvsadm rules/services.】

/sbin/ipvsadm -A -t $VIP:80 -s wlc  #【配置负载均衡调度器】

/sbin/ipvsadm -a -t $VIP:80 -r $RIP1 -g -w 1

/sbin/ipvsadm -a -t $VIP:80 -r $RIP2 -g -w 2

/bin/touch/var/lock/subsys/ipvsadm &> /dev/null   #【添加锁文件】

;; 

stop)

echo0 > /proc/sys/net/ipv4/ip_forward   #【# Stop forwarding packets停止转发协议】

/sbin/ipvsadm–C    #【#Reset ipvsadm清除规则】

/sbin/ifconfigeth0:1 down    #【关闭网络】

/sbin/routedel $VIP   #【清除路由】

/bin/rm-f /var/lock/subsys/ipvsadm   #【删除锁文件】

echo”ipvs is stopped…”    #【输出结果说明】

;;

status)

 if [ ! -e /var/lock/subsys/ipvsadm ]; then

  echo “ipvsadm is stopped …”

 else

  echo “ipvs is running …”

  ipvsadm -L –n     #【-n数字显示网络和端口】

 fi

;;

*)

 echo “Usage: $0{start|stop|status}”

;;

esac
```

### **RealServer自动化脚本:**

```shell
#!/bin/bash

.  /etc/rc.d/init.d/functions

VIP=192.168.0.219

host=`/bin/hostname`   #【取用户名】

case”$1″ in

start)

       # Start LVS-DR real server on thismachine.

        /sbin/ifconfig lo down    #【127.0.0.1网络】

        /sbin/ifconfig lo up

        echo 1 >/proc/sys/net/ipv4/conf/lo/arp_ignore   #【通告限制级别1】

        echo 2 >/proc/sys/net/ipv4/conf/lo/arp_announce    #【响应级别2】

        echo 1 >/proc/sys/net/ipv4/conf/all/arp_ignore

        echo 2 > /proc/sys/net/ipv4/conf/all/arp_announce

        /sbin/ifconfig lo:0 $VIP broadcast $VIPnetmask 255.255.255.255 up  #【别名】

        /sbin/route add -host $VIP dev lo:0  #【添加转发路由】

;;

stop)

        # Stop LVS-DR real server loopbackdevice(s).

        /sbin/ifconfig lo:0 down

        echo 0 >/proc/sys/net/ipv4/conf/lo/arp_ignore

        echo 0 >/proc/sys/net/ipv4/conf/lo/arp_announce

        echo 0 >/proc/sys/net/ipv4/conf/all/arp_ignore

       echo 0 > /proc/sys/net/ipv4/conf/all/arp_announce

;;

status)

        # Status of LVS-DR real server.

        islothere=`/sbin/ifconfig lo:0 | grep$VIP`

        isrothere=`netstat -rn | grep”lo:0″ | grep $VIP`

        if [ ! “$islothere” -o !”isrothere” ];then

#【-o逻辑或，一真为真，否则为假、! 逻辑否，条件为假，结果为真】

            # Either the route or the lo:0device not found.

            echo “LVS-DR real serverStopped.”

        else

            echo “LVS-DR real serverRunning.”

       fi

;;

*)

            # Invalid entry.

            echo “$0: Usage: $0{start|status|stop}”

            exit 1

;;

esac
```

 

**RS健康状态检查脚本示例**：

可以记录日志

```shell
#!/bin/bash

VIP=192.168.10.3

CPORT=80  #【集群端口】

FAIL_BACK=127.0.0.1

RS=(“192.168.10.7″”192.168.10.8”)   #【定义数组】

declare-a RSSTATUS   #【声明数组变量RSSTATUS】

RW=(“2″”1”)

RPORT=80    #【Realserver端口】

TYPE=g   #【DR直接路由模型】

CHKLOOP=3   #【检查的次数】

LOG=/var/log/ipvsmonitor.log   #【日志文件】

 

addrs(){
 

  ipvsadm -a -t $VIP:$CPORT -r $1:$RPORT -$TYPE-w $2    #【客户端添加服务器】

  [ $? -eq 0 ] && return 0 || return 1   #【成功返回0，失败返回1】

}

delrs(){
 

  ipvsadm -d -t $VIP:$CPORT -r $1:$RPORT    #【客户端删除服务器】

  [ $? -eq 0 ] && return 0 || return 1

}

#【判断Realserver是否在线】

checkrs(){
 

  local I=1

  while [ $I -le $CHKLOOP ]; do    #【端口小于等于80】

if curl –connect-timeout I http://$I&> /dev/null; then    #【检查是否在线】

#【curl –connect-timeout <seconds>  指定尝试连接的最大时长】

      return 0

    fi

    let I++

  done

  return 1

}

#【实现对Realserver的健康状态进行检查，检查的结果在线为1，否则为0，生成一个状态值，保存在RSSTATUS[$COUNT]数组中】

initstatus(){  

  local I

  local COUNT=0;

  for I in ${RS[*]}; do    #【引用RS所有数组】

if checkrs $I;then

   RESSSTATUS[$COUNT]=1

else

   RESSSTATUS[$COUNT]=0

fi

  let COUNT++

  done

}

#【执行这个函数给没一个Realserver设定一个初始值，而后进行上面的循环。】

initstatus

while:; do

  let COUNT=0

  for I in ${RS[*]}; do

    if checkrs $I; then    #【在线】

      if [ ${RSSTATUS[$COUNT]} -eq 0 ]; then

         addrs $I ${RW[$COUNT]}

         [ $? -eq 0 ] &&RSSTATUS[$COUNT]=1 && echo “`date +’%F %H:%M:%S’`, $I isback.” >> $LOG

      fi

    else

      if [ ${RSSTATUS[$COUNT]} -eq 1 ]; then   #【不在线】

         delrs $I

         [ $? -eq 0 ] &&RSSTATUS[$COUNT]=0 && echo “`date +’%F %H:%M:%S’`, $I isgone.” >> $LOG

      fi

    fi

    let COUNT++

  done 

  sleep 5

done
```

 

**ipvs的持久连接：【PPC、PCC、PNMPP】**

持久连接主要是将同一用户的不同请求分发至一台服务器，简单来讲就是一内存存储空间，是一段保存在内存中的哈希表，保存内容无非就是键值对。

启动这种功能的时候lvs调度的过程为：

1.用户请求80服务于是根据算法选择了realserver1，并且将这对应关系保存在哈希表里

2.于是第二次同一个请求不管是否是同一客户端的请求到达后，不是将通过算法调度，而是先去查找内存哈希表，查看是否曾经被分配至某个realserver，不管使用的哪个服务都需要查找内存空间的哈希表。

3.如果此前曾经被分配过某个realserver，那么哪怕其访问的其他服务，都统统被分发到此realserver，**所以在实现lvs持久链接的时候，先去查内存空间哈希表，只要表中匹配则进行分配，**那么对于非常繁忙的lvs来说 持久链接就存在瓶颈，如果有N个客户端那么就意味着要建立N个条目，也就意味着内存空间需要充足能容纳我们的记录的条目才可以，所以在必要的时候要去调整模板的空间大小

 

**如果想让lvs工作在持久连接的模型下，方法也很简单：**

在添加ipvs规则的时候加入参数**-p timeout**

-p：表示此连接为持久连接

Timeout：表示维持此持久连接的时间。默认6分钟。当超过这个时间后，如果网页还没有关掉，仍处于激活状态，重新复位时间为2分钟。

 

**LVS持久连接类型**

PPC（persistent【持久的】 port connections）【持久的端口连接】将来自于同一个客户端对同一个服务(端口)的请求，始终定向至此前选定的RS。【缺点：将同一个客户端请求的所有集群服务定向至同一个RS，这种持久性只能对一个服务（端口）持久，如果想实现多个只能定义多个集群】

例如：来自同一个IP的用户第一次访问集群的80端口分配到RealServer1，433号端口分配到Real Server2。当之后这个用户继续访问80端口仍然分配到Real Server1，433号端口仍然分配到Real Server2。

**配置PPC：**配置Director时加上-p timeout选项即可：

```
ipvsadm -A -t192.168.1.100:80 -s wrr–p 300
```

 

PCC（persistent clientconnections）【客户端连接】将来自于同一个客户端的所有请求统统定向至此前选定的RS；也就是只要IP相同，分配的服务器始终相同。

例如，来自同一个IP的用户访问集群的80端口分配到RealServer1，然后用户访问433号端口仍然分配到RealServer1。但如需要SSH：22连接管理Director时，也被分配到Real Server就不好了，下面的PNMPP可以解决这个问题。

**配置PCC**：配置Director时使用0号端口，加上-p timeout选项，这样把所有端口统统定义为集群服务，全部向Real Server转发：

```bash
  ipvsadm -A -t 192.168.1.100:0 -swrr–p 300

  ipvsadm -a -t 192.168.1.100:0 -r192.168.1.102 -g -w 1

  ipvsadm -a -t 192.168.1.100:0 -r192.168.1.103 -g -w 3
```

 

PNMPP（Persistent Netfilter Marked Packet Persistence）【基于防火墙标记的持久性连接】这种防火墙标记仅在数据包在分发器上时有影响，数据包一旦离开Director，就不再被标记。

需要用到iptables的mangle表为数据包设置Mark标记，mangle表主要用于修改数据包的TOS（Type Of Service，服务类型）、TTL（Time To Live，生存周期）指以及为数据包设置Mark标记。

```bash
iptables-t mangle -A PREROUTING -d 192.168.1.100 -eth0 -p tcp –dport 80 -j MARK–set-mark 10
```

它可以将两个毫不相干的端口定义为一个集群服务，例如：合并http的80端口和https的443端口定义为同一个集群服务，而不会出现上面PCC据说的问题。

 

**配置PNMPP：**配置Director时先配置iptables的mangle表为数据包设置Mark标记，下面设置80和22（SSH）端口的数据包都加上标记10，然后ipvs配置就可用-f mask选项，将两个毫不相干的端口定义为一个集群服务：

```bash
iptables-t mangle -A PREROUTING -d 192.168.1.100 -i eth0 -p tcp –dport 80 -j MARK–set-mark 10

iptables-t mangle -A PREROUTING -d 192.168.1.100 -i eth0 -p tcp –dport 22 -j MARK–set-mark 10

ipvsadm-A -f 10 -s wrr -p 600

ipvsadm-a -f 10 -r 192.168.1.102 -g -w 1

ipvsadm-a -f 10 -r 192.168.1.103 -g -w 3
```

