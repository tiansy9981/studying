# Linux内核namespace与cgroup

**Linux Namespace** 是 Kernel 的一个功能，它可以隔离一系列的系统资源，比如 PID( Process ID ）、 User ID 、 Network 等。Namespace 也可以在一些资源上，将进程隔离 起来，这些资源包括进程树、网络接口、挂载点等。

当前 Linux 一共实现了 6 种不同类型的 Namespace :

- Mount Namespace,例如当一个路径下存在文件时，在此路径重新挂载一个新的空文件夹，当前路径下的文件将会被隔离,同时文件相互之间并不影响读写.
- UTS Namespace主要用来隔离 nodename 和 domainname 两个系统标识。 在 UTS Namespace里面 ，每个 Namespace 允许有自己的 hostname。例如在当前bash中hostname -b new 去修改主机名称，hostname查看时主机名会是new。当再次启动一个新的bash时hostname还是为原来的主机名。
- IPCNamespace 用来隔离 System V IPC 和 POSIX message queues 。 每一个 IPC Namespace 都有自己的 System V IPC 和 POSIX message queue。例如在当前shell中执行ipcs -q，查看ipcs是为空，使用ipcmk生成一个ipcs(ipcmk -Q)，查看ipcs里面存在数据；当执行bash进入另一层shell是ipcs -q，看不到宿主机上己经创建的 message queue，说明 IPC Namespace 创建成功，IPC 已经被隔离。
- PID Namespace 是用来隔离进程 ID 的 。同样一个进程在不同的 PID Namespace 里可 以拥有不同的 PID 。例如在宿主机上docker的pid为一个为1的值，在docker中的PID为1。
- User Namespace 主要是隔离用户的用户组 ID 。也就是说，一个进程的User ID和 Group ID在User Namespace内外可以是不同的。比较常用的是，在宿主机上以一个非 root 用户运行创建一个User Namespace，然后在User Namespace里面却映射成root用户。
- Network Namespace 是用来隔离网络设备、 IP 地址端口等网络械的 Namespace。 Network Namespace 可以让每个容器拥有自己**独立的（虚拟的）网络设备**，而且容器内的应用可以绑定到自己的端口，每个 Namespace 内的端口都不会互相冲突。在宿主机上搭建网桥后，就能很方 便地实现容器之间的通信，而且不同容器上的应用可以使用相同的端口 。

**Linux Cgroups (Control Groups ）**提供了对一组进程及将来子进程的资源限制、控制和统计的能力，这些资源包括 CPU、内存、存储、网络等 。 通过 Cgroups，可以方便地限制某个进 程的资源占用，并且可以实时地监控进程的监控和统计信息 。

**Cgroups 中的 3 个组件**

cgroup是对进程分组管理的一种机制，一个 cgroup 包含一组进程，井可以在这cgroup 上增加 Linux subsystem 的各种参数配置，将一组进程和一组subsystem的系统参数关联起来。

subsystem 是一组资源控制的模块，一般包含如下几项 ：

- blkio 设置对块设备（比如硬盘）输入输出的访问控制 ；
- cpu设置cgroup中进程的 CPU 被调度的策略；
- cpuacct 可以统计cgroup中进程的CPU占用 ；
- cpuset 在多核机器上设置 cgroup 中进程可以使用的 CPU 和内存（此处内存仅使用于
- NUMA 架构）；
- devices控制 cgroup 中进程对设备的访问；
- freezer用于挂起（ suspend ）和恢复（ resume) cgroup 中的进程；
- memory用于控制 cgroup 中进程的内存占用；
- net_cls用于将 cgroup 中进程产生的网络包分类，以便Linux的tc(traffic con位oller）可
- 以根据分类区分出来自某个 cgroup 的包并做限流或监控；
- net_prio设置 cgroup 中进程产生的网络流量的优先级 ；
- ns这个subsystem 比较特殊，它的作用是使cgroup中的进程在新的Namespace中fork新进程（ CNEWNS）时，创建出一个新的cgroup，这个cgroup包含新的Namespace 中的进程。

```bash
# lssubsys -a

cpuset 
cpu, cpuacct 
blkio 
memory 
devices 
freezer 
net_cls , net_prio 
perf 
event 
hugetlb 
pids
```

hierarchy 的功能是把一组cgroup串成一个树状的结构， 一个这样的树便是一个hierarchy，通过这种树状结构，Cgroups可以做到继承。

**三个组件相互的关系**

- 系统在创建了新的hierarchy之后，系统中所有的进程都会加入这个hierarchy的、cgroup 根节点，这个cgroup根节点是 hierarchy 默认创建的，这个 hierarchy 中创建的cgroup都是这个cgroup根节点的子节点。
- 一个subsystem只能附加到一个hierarchy上面。
- 一个hierarchy 可以附加多个subsystem 。
- 一个进程可以作为多个cgroup的成员，但是这些cgroup必须在不同的hierarchy中；
- 一个进程fork出子进程时，子进程是和父进程在同一个cgroup中的，也可以根据需要将其移动到其他 cgroup 中 。