

# Linux IP命令

**[linux命令总结之ip命令](https://www.cnblogs.com/ginvip/p/6367803.html)**

Linux的ip命令和ifconfig类似，但前者功能更强大，并旨在取代后者。使用ip命令，只需一个命令，你就能很轻松地执行一些网络管理任务。ifconfig是net-tools中已被废弃使用的一个命令，许多年前就已经没有维护了。iproute2套件里提供了许多增强功能的命令，ip命令即是其中之一。

![https://images2015.cnblogs.com/blog/1089507/201702/1089507-20170205152617089-1009718040.jpg](https://images2015.cnblogs.com/blog/1089507/201702/1089507-20170205152617089-1009718040.jpg)

要安装ip，请[点击这里](http://www.linuxgrill.com/anonymous/iproute2/NEW-OSDL/)下载iproute2套装工具 。不过，大多数Linux发行版已经预装了iproute2工具。

你也可以使用git命令来下载最新源代码来编译：

```bash
$ git clone <https://kernel.googlesource.com/pub/scm/linux/kernel/git/shemminger/iproute2.git>
```

### **设置和删除Ip地址**

要给你的机器设置一个IP地址，可以使用下列ip命令：

```bash
[root@Gin scripts]# ip addr add 192.168.17.30/24 dev eth0
[root@Gin scripts]# ip addr
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
    inet6 ::1/128 scope host
       valid_lft forever preferred_lft forever
2: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UNKNOWN qlen 1000
    link/ether 00:0c:29:84:0c:21 brd ff:ff:ff:ff:ff:ff
    inet 192.168.17.129/24 brd 192.168.17.255 scope global eth0
    inet 192.168.17.30/24 scope global secondary eth0
    inet6 fe80::20c:29ff:fe84:c21/64 scope link
       valid_lft forever preferred_lft forever
```

请注意IP地址要有一个后缀，比如/24。这种用法用于在无类域内路由选择（CIDR）中来显示所用的子网掩码。在这个例子中，子网掩码是255.255.255.0。

你也可以使用相同的方式来删除IP地址，只需用del代替add。

```bash
[root@Gin scripts]# ip addr del 192.168.17.30/24 dev eth0
[root@Gin scripts]# ip addr
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
    inet6 ::1/128 scope host
       valid_lft forever preferred_lft forever
2: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UNKNOWN qlen 1000
    link/ether 00:0c:29:84:0c:21 brd ff:ff:ff:ff:ff:ff
    inet 192.168.17.129/24 brd 192.168.17.255 scope global eth0
    inet6 fe80::20c:29ff:fe84:c21/64 scope link
       valid_lft forever preferred_lft forever
```

### **列出路由表条目**

ip命令的路由对象的参数还可以帮助你查看网络中的路由数据，并设置你的路由表。第一个条目是默认的路由条目，你可以随意改动它。

在这个例子中，有几个路由条目。这个结果显示有几个设备通过不同的网络接口连接起来。它们包括WIFI、以太网和一个点对点连接。

```bash
[root@Gin scripts]# ip route show
192.168.17.0/24 dev eth0  proto kernel  scope link  src 192.168.17.129
169.254.0.0/16 dev eth0  scope link  metric 1002
default via 192.168.17.2 dev eth0
```

假设现在你有一个IP地址，你需要知道路由包从哪里来。可以使用下面的路由选项（译注：列出了路由所使用的接口等）：

```bash
[root@Gin scripts]# ip route get 192.168.17.130
192.168.17.130 dev eth0  src 192.168.17.129
    cache  mtu 1500 advmss 1460 hoplimit 64
```

### **更改默认路由**

要更改默认路由，使用下面ip命令：

```bash
[root@Gin scripts]# ip route add default via 192.168.17.3
```

### **显示网络统计数据**

使用ip命令还可以显示不同网络接口的统计数据

```bash
[root@Gin scripts]# ip -s link
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    RX: bytes  packets  errors  dropped overrun mcast  
    0          0        0       0       0       0     
    TX: bytes  packets  errors  dropped carrier collsns
    0          0        0       0       0       0     
2: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UNKNOWN qlen 1000
    link/ether 00:0c:29:84:0c:21 brd ff:ff:ff:ff:ff:ff
    RX: bytes  packets  errors  dropped overrun mcast  
    135268     1134     0       0       0       0     
    TX: bytes  packets  errors  dropped carrier collsns
    134830     960      0       0       0       0
```

当你需要获取一个特定网络接口的信息时，在网络接口名字后面添加选项ls即可。使用多个选项-s会给你这个特定接口更详细的信息。特别是在排除网络连接故障时，这会非常有用。

```bash
 $ ip -s -s link ls p2p1
```

![https://images2015.cnblogs.com/blog/1089507/201702/1089507-20170205153808292-1236073679.jpg](https://images2015.cnblogs.com/blog/1089507/201702/1089507-20170205153808292-1236073679.jpg)

### **ARP条目**

地址解析协议（ARP）用于将一个IP地址转换成它对应的物理地址，也就是通常所说的MAC地址。使用ip命令的neigh或者neighbour选项，你可以查看接入你所在的局域网的设备的MAC地址。

```bash
[root@Gin scripts]# ip neighbour
192.168.17.1 dev eth0 lladdr 00:50:56:c0:00:08 DELAY
```

### **监控netlink消息**

也可以使用ip命令查看netlink消息。monitor选项允许你查看网络设备的状态。比如，所在局域网的一台电脑根据它的状态可以被分类成REACHABLE或者STALE。使用下面的命令：

```bash
[root@Gin scripts]# ip monitor all
[NEIGH]192.168.17.2 dev eth0 lladdr 00:50:56:f3:2d:50 REACHABLE
[NEIGH]192.168.17.1 dev eth0 lladdr 00:50:56:c0:00:08 REACHABLE
```

### **激活和停止网络接口**

你可以使用ip命令的up和down选项来激某个特定的接口，就像ifconfig的用法一样。

在这个例子中，当ppp0接口被激活和在它被停止和再次激活之后，你可以看到相应的路由表条目。这个接口可能是wlan0或者eth0。将ppp0更改为你可用的任意接口即可。

```bash
$ ip link set eth0 down
  
$ ip link set eth0 up
```

### **获取帮助**

当你陷入困境，不知道某一个特定的选项怎么用的时候，你可以使用help选项。man页面并不会提供许多关于如何使用ip选项的信息，因此这里就是获取帮助的地方。

比如，想知道关于route选项更多的信息：

```bash
[root@Gin scripts]# ip route help
Usage: ip route { list | flush } SELECTOR
       ip route get ADDRESS [ from ADDRESS iif STRING ]
                            [ oif STRING ]  [ tos TOS ]
       ip route { add | del | change | append | replace | monitor } ROUTE
SELECTOR := [ root PREFIX ] [ match PREFIX ] [ exact PREFIX ]
            [ table TABLE_ID ] [ proto RTPROTO ]
            [ type TYPE ] [ scope SCOPE ]
ROUTE := NODE_SPEC [ INFO_SPEC ]
NODE_SPEC := [ TYPE ] PREFIX [ tos TOS ]
             [ table TABLE_ID ] [ proto RTPROTO ]
             [ scope SCOPE ] [ metric METRIC ]
INFO_SPEC := NH OPTIONS FLAGS [ nexthop NH ]...
NH := [ via ADDRESS ] [ dev STRING ] [ weight NUMBER ] NHFLAGS
OPTIONS := FLAGS [ mtu NUMBER ] [ advmss NUMBER ]
           [ rtt TIME ] [ rttvar TIME ] [reordering NUMBER ]
           [ window NUMBER] [ cwnd NUMBER ] [ initcwnd NUMBER ]
           [ ssthresh NUMBER ] [ realms REALM ] [ src ADDRESS ]
           [ rto_min TIME ] [ hoplimit NUMBER ] [ initrwnd NUMBER ]
TYPE := [ unicast | local | broadcast | multicast | throw |
          unreachable | prohibit | blackhole | nat ]
TABLE_ID := [ local | main | default | all | NUMBER ]
SCOPE := [ host | link | global | NUMBER ]
FLAGS := [ equalize ]
MP_ALGO := { rr | drr | random | wrandom }
NHFLAGS := [ onlink | pervasive ]
RTPROTO := [ kernel | boot | static | NUMBER ]
TIME := NUMBER[s|ms]
```

### **小结**

对于网络管理员们和所有的Linux使用者们，ip命令是必备工具。是时候抛弃ifconfig命令了，特别是当你写脚本时。