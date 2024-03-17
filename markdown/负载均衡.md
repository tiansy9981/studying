

# 负载均衡

首页面对这个标题我们面对的概念就是什么是负载均衡？它能干嘛？哪些是负载均衡？下面我们来一一解答。

## 什么是负载均衡？

通过wiki百科上的说明：负载均衡是一种[电子计算机](https://zh.wikipedia.org/wiki/电子计算机)技术，用来在多个计算机（[计算机集群](https://zh.wikipedia.org/wiki/计算机集群)）、网络连接、CPU、磁盘驱动器或其他资源中分配负载，以达到优化资源使用、最大化吞吐率、最小化响应时间、同时避免过载的目的。 使用带有负载平衡的多个服务器组件，取代单一的组件，可以通过[冗余](https://zh.wikipedia.org/wiki/冗余)提高可靠性。负载平衡服务通常是由专用软件和硬件来完成。 主要作用是将大量作业合理地分摊到多个操作单元上进行执行，用于解决互联网架构中的[高并发](https://zh.wikipedia.org/wiki/并发性)和[高可用](https://zh.wikipedia.org/wiki/高可用性)的问题。

首先：负载均衡它是一种技术，用来解决纵向单机器由于性能瓶颈，采用一种对面横向集群架构，集齐资源整合来解决高IO、高延迟、高负载等问题，同时可通过冗余提高高可用性；

主要设计思想就是将大任务分割为多个小任务由不同的单元去处理，以达到低成本、多节点解决大问题；

## 负载均衡有哪些？

负载均衡可以分为硬件与软件负载均衡两种；

硬件负载均衡的特点：性能好（专用的处理器）、提供多种复杂的负载算法、高吞吐量（支持百万级并发）、提供服务以及最主要的特点：贵；市场上常用的硬件负载均衡有F5、NetScaler、Redware、Array以及深信服，其实华三也有硬件负载均衡器。其中最有名的是F5，而且出了名的贵；具体配置以及对应的算法以后工作接触到了以后再在文档进行更新，这里先略过。

软件负载均衡是本文重点，软件负载根据OSI七层模型，分为四层负载均衡以及七层负载均衡；软件负载均衡的特点：功能同样强大、成本低、有一定学习成本；

四层负载均衡常用的有DNS、LVS、Haproxy；

七层负载均衡有：Haproxy、Nginx；

## DNS的那些东东



### 什么是DNS

DNS——Domain Name System，域名系统，是一种互联网服务；在互联网中，服务常常需要通过P地址加对应服务端口号来实现对服务的访问。而在现实世界中，12位的IP地址虽然不是特别难记，但是当你面对众多的IP地址时，你很难去区分他们的不同。在这种情况下，通过一个对应关系，将IP地址绑定一个大家更容易记住并跟现实有一定联系的名称，同时将这个对应的映射关系记录在数据库中，通过分布式的方式将这个对应关系分享出去让各个机器知晓其中的关系，大家即可通过这个名称去访问对应的服务。这个就是域名系统的基本逻辑。

一个域名可以绑定多个IP，从而达到负载均衡的目的，此种负载均衡的方式是一种轮询的方式。

DNS负载均衡最大的优点就是实现简单，成本低，无需自己开发或维护负载均衡设备；

缺点：负载均衡单一，只支持轮询；支持主机记录的IP地址有限；故障恢复时间长，因为域名服务器之间存在着缓存，无法做到实时更新；



## LVS-- Linux Virtual Server

LVS是由国内开源牛人--章文嵩博士，在1995年成立的项目，后面集成至Linux的内核中。LVS 实际上相当于基于 IP 地址的虚拟化应用，为基于 IP 地址和内容请求分发的负载均衡提出了高效的解决方法。

### LVS的工作原理

LVS由用户空间的ipvsadm和内核空间的IPVS组成，ipvsadm用来定义规则，IPVS利用ipvsadm定义的规则工作。简单来说就是用户通过ipvsadm来配置对应的规则来实现流量的分发。

LVS简单工作原理是用户请求LVS的VIP，LVS根据内核的netfilter，根据转发方式和算法，将请求转发给后端服务器，后端服务器接收到请求，返回给用户。netiflter主要实现数据包过滤、网络地址转换(NAT)以及网络连接跟踪等功能；例如iptalbe防火墙通过netfilter来实现包过滤等防火墙功能；

**在Linux中流量从网卡进入后，在系统内核中是如何流向的？**



### LVS相关术语

DS：Director Server，指的是前端负载均衡器节点

RS：Real Server，后端真实的工作服务器

DIP：Director Server IP，主要用于和内部主机通讯的IP地址

RIP：Real Server IP，后端服务器的IP地址

CIP：Client IP，访问客户端的IP地址

VIP：Virtual IP，向外部直接面向用户请求，作为用户请求的目标的IP地址



### LVS的基本安装

由于ipvs集成于内核，同时需要ipvsadm建立规则，所以只需要安装ipvsadm即可。

```
yum install  ipvsadm  -y
```



### LVS的四种模式

#### DR模式



##### 原理



##### 特点

#### NAT模式



##### 原理



##### 特点

#### FULLNAT模式



##### 原理



##### 特点

#### TUNNEL模式

##### 原理



##### 特点

### LVS的负载均衡算法





### LVS的特点

LVS的性能高，因为基于内核，同时规则是基于hash表，可实现几十万的高并发；

稳定性、可靠性好，自身有完美的热备方案；（如：LVS+Keepalived）；

负载均衡算法多；

基于四层网络，对网络的依赖性较大，功能单一只能做负载均衡；

配置简单就几条命令，但是部署、测试将为困难，需要花较长时间；

基于以上特点LVS一般搭配其他七层负载一起使用。

## Haproxy

### 什么是Haproxy

Haproxy是由c语言编写了一个开源负载均衡工具，它提供了四层、七层代理负载的能力，提供多种负载均衡算法以及丰富的功能，同时社区非常活跃，版本更新快速；最关键的是，HAProxy具备媲美商用负载均衡器的性能和稳定性。

### Haproxy的基本原理

基于事件的非堵塞引擎，专注于使用同一个CPU来处理事物，减少切换CPU而带来的switch context的时间浪费；

HAProxy实现了一种事件驱动, 单一进程模型，此模型支持非常大的并发连接数。多进程或多线程模型受内存限制 、系统调度器限制以及无处不在的锁限制，很少能处理数千并发连接。

事件驱动模型因为在有更好的资源和时间管理的用户空间(User-Space) 实现所有这些任务，所以没有这些问题。此模型的弊端是，在多核系统上，这些程序通常扩展性较差。这就是为什么他们必须进行优化以使每个CPU时间片(Cycle)做更多的工作。

同时haproxy支持多CPU多进程，通过全局配置中的nbproc指定cpu的数量，通过cpu-map使进程绑定对应的cpu。

### Haproxy的基本安装

两种安装方式：**源码包安装**，通过官网下载对应稳定版本的源码包进行二进制安装；**yum安装**

在服务启动前需要修改内核的配置，具体如下：

```shell
net.ipv4.ip_forward = 0  #当跨网段时需要打开转发功能
# Controls source route verification
net.ipv4.conf.default.rp_filter = 1
# Do not accept source routing
net.ipv4.conf.default.accept_source_route = 0
# Controls the System Request debugging functionality of the kernel
kernel.sysrq = 0
# Controls whether core dumps will append the PID to the core filename.
# Useful for debugging multi-threaded applications.
kernel.core_uses_pid = 1
# Controls the use of TCP syncookies
net.ipv4.tcp_syncookies = 1
# Controls the default maxmimum size of a mesage queue
kernel.msgmnb = 65536
# Controls the maximum size of a message, in bytes
kernel.msgmax = 65536
# Controls the maximum shared segment size, in bytes
kernel.shmmax = 68719476736
# Controls the maximum number of shared memory segments, in pages
kernel.shmall = 4294967296
net.ipv4.conf.default.promote_secondaries = 1
net.ipv4.ip_forward = 1
net.ipv4.conf.default.rp_filter = 2
net.ipv4.conf.all.rp_filter = 2
net.ipv4.conf.all.send_redirects = 1
net.ipv4.conf.default.send_redirects = 1
### 表示是否打开TCP同步标签（syncookie），同步标签可以防止一个套接字在有过多试图连接到达时引起过载
net.ipv4.tcp_syncookies = 1
net.ipv4.tcp_tw_reuse = 1
net.ipv4.tcp_tw_recycle = 1
net.ipv4.tcp_fin_timeout = 30
net.ipv4.tcp_keepalive_time = 1200
net.ipv4.tcp_keepalive_probes = 5
net.ipv4.tcp_keepalive_intvl = 15
net.ipv4.ip_local_port_range = 20000 65000
net.ipv4.tcp_max_syn_backlog = 40960
net.ipv4.tcp_max_tw_buckets = 819200
net.core.somaxconn = 262144
net.core.netdev_max_backlog = 262144
net.ipv4.tcp_max_orphans = 262144
net.ipv4.tcp_orphan_retries = 3
net.ipv4.tcp_synack_retries = 3
net.ipv4.tcp_syn_retries = 3
net.ipv4.tcp_sack = 1
net.ipv4.tcp_fack = 1
net.ipv4.tcp_dsack = 1
net.ipv4.icmp_echo_ignore_broadcasts = 1
net.ipv4.icmp_ignore_bogus_error_responses = 1
### 系统中所允许的文件句柄的最大数目
fs.file-max = 65535
### 单个进程所允许的文件句柄的最大数目
fs.nr_open = 65535
kernel.pid_max = 65536
net.ipv4.tcp_rmem = 4096 87380 8388608
net.ipv4.tcp_wmem = 4096 87380 8388608
net.core.rmem_max = 8388608
net.core.wmem_max = 8388608
net.core.netdev_max_backlog = 5000
net.ipv4.tcp_window_scaling = 1
```



### Haproxy配置说明

Haproxy的配置分为全局配置与其他配置

**全局配置**

```
global
         log 127.0.0.1 local3        #定义haproxy日志输出设置
         log 127.0.0.1   local1 notice        
         #log loghost    local0 info #定义haproxy 日志级别
         ulimit-n 82000              #设置每个进程的可用的最大文件描述符
         maxconn 20480               #默认最大连接数
         chroot /usr/local/haproxy   #chroot运行路径
         uid 99                      #运行haproxy 用户 UID
         gid 99                      #运行haproxy 用户组gid
         daemon                      #以后台形式运行harpoxy
         nbproc 1                    #设置进程数量
         pidfile /usr/local/haproxy/run/haproxy.pid  #haproxy 进程PID文件
         #debug                      #haproxy调试级别，建议只在开启单进程的时候调试
         #quiet
```

**defaults配置**

```
defaults
         log    global         #引入global定义的日志格式
         mode    http          #所处理的类别(7层代理http，4层代理tcp)
         maxconn 50000         #最大连接数
         option  httplog       #日志类别为http日志格式
         option  httpclose     #每次请求完毕后主动关闭http通道
         option  dontlognull   #不记录健康检查日志信息
         option  forwardfor    #如果后端服务器需要获得客户端的真实ip，需要配置的参数，
                               可以从http header 中获取客户端的IP
         retries 3             #3次连接失败就认为服务器不可用，也可以通过后面设置
         
         option redispatch  
#《---上述选项意思是指serverID 对应的服务器挂掉后，强制定向到其他健康的服务器, 当使用了cookie时，
haproxy将会将其请求的后端服务器的serverID插入到cookie中，以保证会话的SESSION持久性；而此时，如果
后端的服务器宕掉了,但是客户端的cookie是不会刷新的，如果设置此参数，将会将客户的请求强制定向到另外一个
后端server上，以保证服务的正常---》
         
         stats refresh 30       #设置统计页面刷新时间间隔
         option abortonclose    #当服务器负载很高的时候，自动结束掉当前队列处理比较久的连接
         balance roundrobin     #设置默认负载均衡方式，轮询方式
         #balance source        #设置默认负载均衡方式，类似于nginx的ip_hash      
         #contimeout 5000        #设置连接超时时间
         #clitimeout 50000       #设置客户端超时时间
         #srvtimeout 50000       #设置服务器超时时间
         timeout http-request    10s  #默认http请求超时时间
         timeout queue           1m   #默认队列超时时间
         timeout connect         10s  #默认连接超时时间
         timeout client          1m   #默认客户端超时时间
         timeout server          1m   #默认服务器超时时间
         timeout http-keep-alive 10s  #默认持久连接超时时间
         timeout check           10s  #设置心跳检查超时时间
```

**frontend配置**

```
frontend http_80_in
     bind 0.0.0.0:80    #设置监听端口，即haproxy提供的web服务端口，和lvs的vip 类似
     mode http          #http 的7层模式
     log global         #应用全局的日志设置
     option httplog     #启用http的log
     option httpclose   #每次请求完毕后主动关闭http通道，HAproxy不支持keep-alive模式     
     option forwardfor  #如果后端服务器需要获得客户端的真实IP需要配置此参数，将可以从HttpHeader中获得客户端IP
     default_backend wwwpool   #设置请求默认转发的后端服务池
```

**backend配置**

```
backend wwwpool      #定义wwwpool服务器组。
  mode http           #http的7层模式
  option  redispatch
  option  abortonclose
  balance source      #负载均衡的方式，源哈希算法
  cookie  SERVERID    #允许插入serverid到cookie中，serverid后面可以定义
  option  httpchk GET /test.html   #心跳检测
  server web1 10.1.1.2:80 cookie 2 weight 3 check inter 2000 rise 2 fall 3 maxconn 8
```

**listen配置**

```
listen  admin_status           #Frontend和Backend的组合体,监控组的名称，按需自定义名称 
         bind 0.0.0.0:8888              #监听端口 
         mode http                      #http的7层模式 
         log 127.0.0.1 local3 err       #错误日志记录 
         stats refresh 5s               #每隔5秒自动刷新监控页面 
         stats uri /admin?stats         #监控页面的url访问路径 
         stats realm itnihao\ welcome   #监控页面的提示信息 
         stats auth admin:admin         #监控页面的用户和密码admin,可以设置多个用户名 
         stats auth admin1:admin1       #监控页面的用户和密码admin1 
         stats hide-version             #隐藏统计页面上的HAproxy版本信息  
         stats admin if TRUE            #手工启用/禁用,后端服务器(haproxy-1.4.9以后版本)
```

**常用配置**

```
global

maxconn 100000
chroot /usr/local/haproxy  #设置活动目录，防止攻击
stats socket /var/lib/haproxy/haproxy.sock mode 600 level admin    #注意权限
uid 99       # the nobody user 可以更换其他用户
gid 99       # the nobody group 可以更换其他用户
daemon
#nbproc 4    #config work processor count  and use cpu-map bind cpu 配置多进程以及绑定cpu
#cpu-map 1 0
#cpu-map 2 1
#cpu-map 3 2
#cpu-map 4 3
pidfile /var/lib/haproxy/haproxy.pid
log 127.0.0.1 local3 info    #log level  日志级别  local3是什么意思？


defaults

option http-keep-alive  #haproxy支持4种连接模式：
option forwardfor
maxconn 100000
mode http
timeout connect 300000ms
timeout client  300000ms
timeout server  300000ms


listen stats    #haproxy监控界面 status page
mode http
bind 0.0.0.0:9999
stats enable
log global
stats uri  /haproxy-status      # uri address
stats auth haadmin:q1w2e3r4ys   #username & passwd

#listen web port
#bind 192.168.7.101:80
#mode http
#log global
#127.0.0.1:8080 check inter 3000 fall 2 rise 5server web1 .%
```



### Haproxy负载均衡算法

**算法总结**

| 算法       | 支持的协议 | 静态/动态                        |
| ---------- | ---------- | -------------------------------- |
| static-rr  | tcp/http   | 静态                             |
| first      | tcp/http   | 静态                             |
| roundrobin | tcp/http   | 动态                             |
| leastconn  | tcp/http   | 动态                             |
| random     | tcp/http   | 动态                             |
| source     | tcp/http   | 取决于 hash_type 是否 consistent |
| uri        | http       | 取决于 hash_type 是否 consistent |
| url_param  | http       | 取决于 hash_type 是否 consistent |
| hdr        | http       | 取决于 hash_type 是否 consistent |
| rdp-cookie | http       | 取决于 hash_type 是否 consistent |

> 
>
> `first`  ：根据服务器在列表中的位置，自上而下进行调度(不人为的设定，从上到下各台服务器的 id 会自动增加，第一台服务器的 id 为 1)，但是其只会当第一台服务器的连接数达到上限(maxconn 参数)，新请求才会分配给下一台服务，因此 HAProxy 会忽略服务器的权重值，这样比较适合长会话的协议如：RDP1和 IMAP。
> `source`调度策略为source IP hash(源地址哈希)，基于客户端源地址 hash并将请求转发到后端服务器，默认为静态方式即取模方式，但是可以通过 hash-type支持的选项更改，后续同一个源地址请求将被转发至同一个后端 web 服务器，比较适用于 session 保持/缓存业务等场景。
>
> `uri`调度算法会对 URI 的左侧部分(问号前面的部分)或者对整个 URI 进行哈希，并使用活动服务器的总权重值除以该哈希值。
>
> `url_param` 对用户请求的 url 中的 params 部分中的参数 name 作 hash计算，并除以服务器总权重，根据计算结果派发至某挑出的服务器；通常用于追踪用户，以确保来自同一个用户的请求始终发往同一个 real server。其亦可以使用hash-type指定一致性哈希方法。另外，如果找不到 URL 中问号选项后的参数，则使用 roundrobin 算法。
>
> `hdr`针对用户每个 http 头部(header)请求中的指定信息做 hash，此处由 name 指定的 http 首部将会被取出并做 hash 计算，然后由服务器总权重相除以后派发至某挑出的服务器，假如无有效的值，则会使用默认的轮询调度。
>
> `rdp`(windows 远程桌面协议)-cookie 用于对 windows 远程桌面的反向代理，主要是使用 cookie 保持来会话；



**算法使用场景**

| 算法       | 适用场景                                         |
| ---------- | ------------------------------------------------ |
| first      | 使用较少，节约成本                               |
| static-rr  | 做了 session 共享的 web 集群                     |
| roundrobin | 做了 session 共享的 web 集群                     |
| random     | 做了 session 共享的 web 集群                     |
| leastconn  | 数据库                                           |
| source     | 基于客户端公网 IP 的会话保持                     |
| uri        | 缓存服务器，CDN 服务商，蓝汛、百度、阿里云、腾讯 |
| url_param  | 缓存服务器，CDN 服务商，蓝汛、百度、阿里云、腾讯 |
| hdr        | 基于客户端请求报文头部做下一步处理               |
| rdp-cookie | 很少使用                                         |



### Haproxy的特点

性能强大，由于基于事件非堵塞引擎，haproxy的性能十分强大，可支持10W+的高并发；同时拥有良好的稳定性

健康检查：支持TCP和HTTP两种健康检查模式

会话保持：对于未实现会话共享的应用集群，可通过Insert Cookie/Rewrite Cookie/Prefix Cookie，以及上述的多种Hash方式实现会话保持

SSL：HAProxy可以解析HTTPS协议，并能够将请求解密为HTTP后向后端传输

HTTP请求重写与重定向

监控与统计：HAProxy提供了基于Web的统计信息页面，展现健康状态和流量数据。基于此功能，使用者可以开发监控程序来监控HAProxy的状态

日志通过syslog写入

haproxy -f可支持文件夹

## Nginx

### 什么是Nginx



### Nginx的基本原理





### Nginx的基本安装



### Nginx配置说明





### Nginx负载均衡算法





### Nginx的特点