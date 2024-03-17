# ansible 通过 kubeadm 部署 Kubernetes

## 1、初始化工作

修改各个主机的 hostname  使用 hostname 模块

修改 host 文件，使用j2 模版，通过 fact 变量将 ip 与主机名对应

关闭防火墙

关闭 swap

配置 swap 永久关闭

修改内核配置

修改 yum 源

安装基础包

安装 containerd

配置时间同步

开启 ipvs


