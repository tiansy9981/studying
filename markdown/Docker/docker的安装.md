# dokcer的安装

通过安装yum-utils，系统会支持yum-config-manager命令，通过这个命令增加对应的docker-ce的repo文件，重新运行yum缓存即yum makcecache后yum安装docker-ce；

安装docker-ce后，需关闭selinux；还需要完成下面两个操作：

1、修改系统内核开启包转发功能，首先在内核中增加br_ netfiltere模块，br_netfiltere模块主要用于将桥接流量转发至iptables链，开启此模块内核参数需执行命令modprobe br_netfilter，如需要做到此模块下一次开启开机自启动，需在/etc/目录下新建rc. sysinit 文件，文件内容如下：

```bash
#!/bin/bash
for file in /etc/sysconfig/modules/*. modules ; do
[ -x $file ] && $file
done
```

然后在/etc/sysconfig/modules/目录下，创建br_netfilter.modules的文件，然后文件内容为modprobe br_netfilter；同时需对/etc/sysconfig/modules/br_netfilter.modules设置755的权限；

最后在/etc/sysctl.d目录下创建一个docker.conf文件，文件内添加如下配置：

```bash
net.bridge.bridge-nf-call-ip6tables = 1
net.bridge.bridge-nf-call-iptables = 1  
net.ipv4.ip_forward = 1 #开启ipv4包转发功能

#执行命令生效
sysctl -p /etc/sysctl.d/docker.conf
```

2、由于docker镜像库是位于国外，国外访问的速度较慢，为方便日常使用，需配置docker镜像加速器；具体操作如下：首先在阿里云控制台找到镜像加速器，在/etc/docker目录下，新建一个daemon.json的文件，导入内容：

```bash
{
"registry-mirrors": ["https://**"]
}

#如需添加多个镜像加速器
{
"registry-mirrors": ["https://**","https://**","https://**"]
}
#执行命令danmonreload命令，重载配置
systemctl daemon-reload
#重启启动docker
systemctl restart docker
```