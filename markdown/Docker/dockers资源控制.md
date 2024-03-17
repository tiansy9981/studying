# dockers资源管理

### **docker容器控制cpu**

Docker通过cgroup来控制容器使用的资源限制，可以对docker限制的资源包括CPU、内存、磁盘

### **指定docker容器可以使用的cpu份额**

```
 #查看配置份额的帮助命令：
 
 [root@xianchaomaster1 ~]# docker run --help | grep cpu-shares
```

cpu配额参数：-c, --cpu-shares    int

CPU shares (relative weight) 在创建容器时指定容器所使用的CPU份额值。cpu-shares的值不能保证可以获得1个vcpu或者多少GHz的CPU资源，**仅仅只是一个弹性的加权值**。

**默认每个docker容器的cpu份额值都是1024**。在同一个CPU核心上，同时运行多个容器时，容器的cpu加权的效果才能体现出来。

例： 两个容器A、B的cpu份额分别为1000和500，结果会怎么样？

情况1：A和B正常运行，占用同一个CPU，在cpu进行时间片分配的时候，容器A比容器B多一倍的机会获得CPU的时间片。

情况2：分配的结果取决于当时其他容器的运行状态。比如容器A的进程一直是空闲的，那么容器B是可以获取比容器A更多的CPU时间片的； 比如主机上只运行了一个容器，即使它的cpu份额只有50，它也可以独占整个主机的cpu资源。

**cgroups**只在多个容器同时争抢同一个cpu资源时，cpu配额才会生效。因此，无法单纯根据某个容器的cpu份额来确定有多少cpu资源分配给它，资源分配结果取决于同时运行的其他容器的cpu分配和容器中进程运行情况。

例1：给容器实例分配512权重的cpu使用份额

参数： --cpu-shares 512

```
 [root@xianchaomaster1 ~]# docker run -it --cpu-shares 512 centos /bin/bash
 
 [root@df176dd75bd4 /]# cat /sys/fs/cgroup/cpu/cpu.shares
 
  #查看结果：
 
 512
```

注：稍后，我们启动多个容器，测试一下是不是只能使用512份额的cpu资源。单独一个容器，看不出来使用的cpu的比例。 因没有docker实例同此docker实例竞争。

**总结：**

通过-c设置的 cpu share 并不是 CPU 资源的绝对数量，而是一个相对的权重值。某个容器最终能分配到的 CPU 资源取决于它的 cpu share 占所有容器 cpu share 总和的比例。通过 cpu share 可以设置容器使用 CPU 的优先级。

比如在 host 中启动了两个容器：

```
 docker run --name "container_A" -c 1024 ubuntu
 
 docker run --name "container_B" -c 512 ubuntu
```

container_A 的 cpu share 1024，是 container_B 的两倍。当两个容器都需要 CPU 资源时，container_A 可以得到的 CPU 是 container_B 的两倍。

需要注意的是，这种按权重分配 CPU只会发生在 CPU资源紧张的情况下。如果 container_A 处于空闲状态，为了充分利用 CPU资源，container_B 也可以分配到全部可用的 CPU。

### **CPU core核心控制**

参数：--cpuset可以绑定CPU

对多核CPU的服务器，docker还可以控制容器运行限定使用哪些cpu内核和内存节点，即使用--cpuset-cpus和--cpuset-mems参数。**对具有NUMA拓扑（具有多CPU、多内存节点）的服务器尤其有用**，可以对需要高性能计算的容器进行性能最优的配置。如果服务器只有一个内存节点，则--cpuset-mems的配置基本上不会有明显效果。

**扩展：**

服务器架构一般分： SMP、NUMA、MPP体系结构介绍

从系统架构来看，目前的商用服务器大体可以分为三类：

1. 即对称多处理器结构(SMP ： Symmetric Multi-Processor) 例： x86 服务器，**双路服务器**。主板上有两个物理cpu
2. 非一致存储访问结构 (NUMA ： Non-Uniform Memory Access) 例： IBM 小型机 pSeries 690
3. 海量并行处理结构 (MPP ： Massive ParallelProcessing) 。 例： 大型机 Z14

### **CPU配额控制参数的混合使用**

在上面这些参数中，cpu-shares控制只发生在容器竞争同一个cpu的时间片时有效。

如果通过cpuset-cpus指定容器A使用cpu 0，容器B只是用cpu1，在主机上只有这两个容器使用对应内核的情况，它们各自占用全部的内核资源，cpu-shares没有明显效果。

如何才能有效果？

容器A和容器B配置上cpuset-cpus值并都绑定到同一个cpu上，然后同时抢占cpu资源，就可以看出效果了。

例1：测试cpu-shares和cpuset-cpus混合使用运行效果，就需要一个压缩力测试工具stress来让容器实例把cpu跑满。

如何把cpu跑满？

如何把4核心的cpu中第一和第三核心跑满？可以运行stress，然后使用taskset绑定一下cpu。

**先扩展：stress命令**

概述：linux系统压力测试软件Stress 。

```
 [root@xianchaomaster1 ~]# yum install -y epel-release
 
 [root@xianchaomaster1 ~]# yum install stress -y
 stress参数解释
 
 -?    显示帮助信息
 
 -v    显示版本号
 
 -q    不显示运行信息
 
 -n    显示已完成的指令情况
 
 -t    --timeout N 指定运行N秒后停止
 
 •      --backoff  N  等待N微妙后开始运行
 
 -c    产生n个进程 ：每个进程都反复不停的计算随机数的平方根，测试cpu
 
 -i    产生n个进程 ：每个进程反复调用sync()，sync()用于将内存上的内容写到硬盘上，测试磁盘io
 
 -m   --vm n 产生n个进程,每个进程不断调用内存分配malloc（）和内存释放free（）函数 ，测试内存
 
 •     --vm-bytes B 指定malloc时内存的字节数 （默认256MB）
 
 •     --vm-hang N  指定在free栈的秒数
 
 -d  --hadd n 产生n个执行write和unlink函数的进程
 
 •     -hadd-bytes B 指定写的字节数
 
 •     --hadd-noclean 不unlink
 
 #注：时间单位可以为秒s，分m，小时h，天d，年y，文件大小单位可以为K，M，G
```

例1：产生2个cpu进程，2个io进程，20秒后停止运行

```
 [root@xianchaomaster1]# stress -c 2 -i 2 --verbose --timeout 20s
 
 #如果执行时间为分钟，改20s 为1m
 
 #查看：
 
 top
```

![image-20230325090141716](E:\BaiduNetdiskWorkspace\docker相关知识\assets\image-20230325090141716-1679706107194-1.png)

例1：测试cpuset-cpus和cpu-shares混合使用运行效果，就需要一个压缩力测试工具stress来让容器实例把cpu跑满。 当跑满后，会不会去其他cpu上运行。 如果没有在其他cpu上运行，说明cgroup资源限制成功。

实例3：创建两个容器实例:docker10 和docker20。 让docker10和docker20只运行在cpu0和cpu1上，最终测试一下docker10和docker20使用cpu的百分比。实验拓扑图如下：

![image-20230325090159147](E:\BaiduNetdiskWorkspace\docker相关知识\assets\image-20230325090159147.png)

运行两个容器实例

```
 [root@xianchaomaster1 ~]# docker run -itd --name docker10 --cpuset-cpus 0,1 --cpu-shares 512 centos /bin/bash
 
 #指定docker10只能在cpu0和cpu1上运行，而且docker10的使用cpu的份额512
 
 #参数-itd就是又能打开一个伪终端，又可以在后台运行着docker实例
 
 
 
 [root@xianchaomaster1 ~]# docker run -itd --name docker20 --cpuset-cpus 0,1 --cpu-shares 1024 centos /bin/bash
 
 #指定docker20只能在cpu0和cpu1上运行，而且docker20的使用cpu的份额1024，比dcker10多一倍
```

测试1： 进入docker10，使用stress测试进程是不是只在cpu0,1上运行：

```
 [root@xianchaomaster1 ~]# docker exec -it  docker10  /bin/bash
 
 [root@d1a431815090 /]# yum install -y epel-release #安装epel扩展源
 
 [root@d1a431815090 /]# yum install stress -y   #安装stress命令
 
 [root@d1a431815090 /]#  stress -c 2 -v  -t 10m #运行2个进程，把两个cpu占满
```

在物理机另外一个虚拟终端上运行top命令，按1快捷键，查看每个cpu使用情况：

![image-20230325090224845](E:\BaiduNetdiskWorkspace\docker相关知识\assets\image-20230325090224845.png)

可看到正常。只在cpu0,1上运行

测试2： 然后进入docker20，使用stress测试进程是不是只在cpu0,1上运行，且docker20上运行的stress使用cpu百分比是docker10的2倍

```
[root@xianchaomaster1 ~]# docker exec -it  docker20  /bin/bash

[root@d1a431815090 /]# yum install -y epel-release #安装epel扩展源

[root@d1a431815090 /]# yum install stress -y

[root@f24e75bca5c0 /]# stress  -c 2 -v -t 10m
```

在另外一个虚拟终端上运行top命令，按1快捷键，查看每个cpu使用情况：

![image-20230325090251545](E:\BaiduNetdiskWorkspace\docker相关知识\assets\image-20230325090251545.png)

注：两个容器只在cpu0,1上运行，说明cpu绑定限制成功。而docker20是docker10使用cpu的2倍。说明--cpu-shares限制资源成功。

### **docker容器控制内存**

Docker提供参数-m, --memory=""限制容器的内存使用量。

例1：允许容器使用的内存上限为128M：

```
[root@xianchaomaster1 ~]# docker run -it -m 128m centos

#查看：

[root@40bf29765691 /]# cat /sys/fs/cgroup/memory/memory.limit_in_bytes

134217728

#注：也可以使用tress进行测试，到现在，我可以限制docker实例使用cpu的核心数和权重，可以限制内存大小。
```

例2：创建一个docker，只使用2个cpu核心，只能使用128M内存

```
[root@xianchaomaster1 ~]# docker run -it --cpuset-cpus 0,1 -m 128m centos
```

### **docker容器控制IO**

```
[root@xianchaomaster1 ~]# docker run --help | grep write-b

   --device-write-bps value   Limit write rate (bytes per second) to a device (default [])

#限制此设备上的写速度（bytes per second），单位可以是kb、mb或者gb。

--device-read-bps value  #限制此设备上的读速度（bytes per second），单位可以是kb、mb或者gb。
```

情景：防止某个 Docker 容器吃光你的磁盘 I / O 资源

例1：限制容器实例对硬盘的最高写入速度设定为 2MB/s。

- -device参数：将主机设备添加到容器

```
[root@xianchaomaster1 ~]# mkdir -p /var/www/html/

[root@xianchaomaster1 ~]# docker run -it -v /var/www/html/:/var/www/html --device /dev/sda:/dev/sda --device-write-bps /dev/sda:2mb centos /bin/bash

[root@bd79042dbdc9 /]# time dd if=/dev/sda of=/var/www/html/test.out bs=2M count=50 oflag=direct,nonblock
```

**注：dd 参数：**

direct：读写数据采用直接IO方式，不走缓存。直接从内存写硬盘上。

nonblock：读写数据采用非阻塞IO方式，优先写dd命令的数据

50+0 records in

50+0 records out

52428800 bytes (52 MB) copied, 50.1831 s, 2.0 MB/s

real 0m50.201s

user 0m0.001s

sys 0m0.303s

注： 发现1秒写2M。 限制成功。

### **docker容器运行结束自动释放资源**

```
[root@xianchaomaster1 ~]# docker run --help | grep rm

   --rm 参数：         Automatically remove the container when it exits

#作用：当容器命令运行结束后，自动删除容器，自动释放资源
```

例：

```
[root@xianchaomaster1 ~]# docker run -it --rm --name xianchao centos sleep 6
```

物理上查看：

```
[root@xianchaomaster1 ~]# docker ps -a | grep xianchao

6c75a9317a6b    centos       "sleep 6"      6 seconds ago    Up 4 seconds              mk
```

等5s后，再查看：

```
[root@xianchaomaster1 ~]# docker ps | grep xianchao #自动删除了
```