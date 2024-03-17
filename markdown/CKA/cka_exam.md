### 第一题：kubeadm在Ubuntu上安装k8s集群

**解题思路：**

1. 配置网络，主要配置静态IP以及主机名称；

   ```bash
   hostnamectl set-hostname master1
   hostnamectl set-hostname node1
   nmcli connection modify connection_name  ipv4.method manual  ipv4.addresses  *.*.*.*/*
   ```

2. 安装依赖；

   ```bash
    apt-get install -y apt-transport-https ca-certificates curl software-properties-common gnupg2
   ```

3. 安装docker、配置docker并启动；

   ```bash
   curl -fsSL https://mirrors.ustc.edu.cn/docker-ce/linux/ubuntu/gpg | sudo apt-key add - #添加docker-ce的gpg证书
    sudo add-apt-repository \
      "deb [arch=amd64] https://mirrors.ustc.edu.cn/docker-ce/linux/ubuntu/ \
     $(lsb_release -cs) \
     stable"   #添加docker apt源
   apt-get update && apt-get install docker-ce docker-ce-cli containerd.io -y 
   
   #配置docker后台配置
   cat <<EOF | tee /etc/docker/daemon.json
   {
     "exec-opts": ["native.cgroupdriver=systemd"],
     "log-driver": "json-file",
     "log-opts": {
       "max-size": "100m"
     },
     "storage-driver": "overlay2"
   }
   EOF
    mkdir -p /etc/systemd/system/docker.service.d
    systemctl daemon-reload
    systemctl restart docker
    systemctl enable --now docker
   ```

4. 安装并更新k8s的apt源；

   ```bash
   #添加k8s的apt源    其中cat <<EOF >/file可以写成cat >/file <<EOF
   cat <<EOF >/etc/apt/sources.list.d/kubernetes.list
   deb http://mirrors.ustc.edu.cn/kubernetes/apt kubernetes-xenial main
   EOF
   apt-get update  #更新apt源，同时会报错提示缺少公钥
   apt-key adv --recv-keys --keyserver keyserver.ubuntu.com  ****   #添加报错时的公钥
   ```

5. 安装k8s；

   ```bash
   apt-get update && apt-get install -y kubelet=1.23.1-00 kubeadm=1.23.1-00 kubectl=1.23.1-00  #更新apt并安装k8s。
    apt-mark hold kubelet kubeadm kubectl  #标记为hold back，抑制更新
    swapoff -a
    kubeadm init --apiserver-advertise-address “your IP” --image-repository registry.cn-hangzhou.aliyuncs.com/google_containers --pod-network-cidr=10.244.0.0/16 --ignore-preflight-errors=SystemVerification  --kubernetes-version=v1.23.1
    mkdir -p $HOME/.kube
    sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
    sudo chown $(id -u):$(id -g) $HOME/.kube/config
    kubeadm token create --print-join-command #重新打印加入集群的token
    kubeadm join 192.168.10.104:6443 --token 4ispln.u1o336lu7p3kaek8 --discovery-token-ca-cert-hash sha256:18ffc0979dce1e94cd231a7cd3c3a486dbcfe5d5a7048e7131aa7b15e27e2e7f  #node添加至集群
    
    kubeadm  reset #重新安装k8s集群
   ```

6. 安装calico；

   ```bash
   kubectl apply -f  calico.yaml  #主要需要提前下载calico所需的镜像。addon与metrics-server-amd64-0-3-6
   kubectl get nodes #查看集群是非正常
   ```

   

### 第二题：配置rbac的授权

**题目：**

![1651029231822790](https://raw.githubusercontent.com/tiansy9981/images/master/1651029231822790-1680400486984-2.png?token=AEL2C65CDBAXW5YEIBDACK3F62EGQ)

**解题思路：**

切换至指定的集群：

```bash
kubectl config use-context k8s
```

1. 首先创建namespace app-team1

   ```bash
   kubectl create ns app-team1  #题目中可能存在这个名称空间
   ```

2. 创建named cicd-token的sa

   ```bash
   kubectl create sa cicd-token -n app-team1
   ```

3. 创建clusterrole  deployment-clusterrole 并赋予资源访问权限

   ```bash
   kubectl create clusterrole deployment-clusterrole  --verb=create  --resource=deployments,statefulsets,daemonsets 
   ```

4. 使用rollbinding 绑定sa。由于限定了名称空间所以用rollbinding

   ```bash
   kubectl  create rolebingding cicd-token-binding -n app-team1 --clusterrole=deployment-clusterrole  --serviceaccount=app-team1:cicd-token
   #验证
   kubectl describe rollbinding  cicd-token-binding  -n app-team1
   ```








### 第三题： 驱逐node内的pod，并设置node不可用

**题目：**

![8eaafb333008b96b43b94ef4355ee39.png](https://raw.githubusercontent.com/tiansy9981/images/master/1651029556391018-1680400497304-6.png?token=AEL2C66IKLVRWXBG74UPJM3F62DEA)

**解题思路：**

1. 设置node 加入隔离区，设置为cordon状态

   ```bash
   kubectl cordon node1
   ```

2.  将node内的pod驱逐出去

   ```bash
   kubuctl drain node1 --delete-emptydir-data  --ignore-deamonsets --force
   ```

### 第四题： k8s的升级

**题目：**

![image-20230402075653347](https://raw.githubusercontent.com/tiansy9981/images/master/image-20230402075653347-1680400503498-9.png?token=AEL2C63TW7MJ36S65VLSZH3F62DQG)

**解题思路：**

1. 隔离master节点，设置master节点cordon状态

   ```bash
   kubuctl cordon master  #在node节点上执行
   ```

2. 驱逐master节点的pod

   ```bash
   kubectl drain master --delete-emptydir-data  --ignore-daemonsets --force #在node节点上执行
   ```

3. 查看apt源中的k8s版本以及更新apt源

   ```bash
   apt-cache show kubeadm|grep 1.23.2
   apt-get update
   ```
   
4. 执行升级计划

   ```bash
   kubeamd upgrade plan
   kubeadm upgrade apply v1.23.2 --etcd-upgrade=false
   ```

5. 如果需要升级kubelet、kubectl

   ```bash
   apt-get install kubelet=1.23.2-00
   apt-get install kubectl=1.23.2-00
   ```

6. 将隔离区的master节点恢复调度

   ```bash
   kubuctl uncordon master 
   ```



> pod如何在drain之后并使用uncordon恢复pod的调度



### 第五题：etcd数据库的备份与还原 

**题目：**

![3d4b0ff3325346d94f7672354ed4d6d.png](https://raw.githubusercontent.com/tiansy9981/images/master/1651029724653120.png?token=AEL2C63DMJYQGRHSGXHFWQTF62DQU)

**解题思路：**

1. 需要使用etcdctl的命令以及注意登录的节点位置  

   ```bash
   export ETCDCTL_API=3
   mkdir  -p /srv/data 
   ```

2. 备份etcd数据库到指定位置

   ```bash
   ETCDCTL_API=3 etcdctl --endpoints=https://127.0.0.1:2379  --cacert=/opt/KUIN00601/etcd/ca.crt --cert=/opt/KUIN00601/etcd/server.crt --key=/opt/KUIN00601/etcd/server.key  snapshot save /srv/data/etcd-snapshot.db
   #验证
   ll -h /srv/data/etcd-snapshot.db
   ```

3. 还原etcd数据库

   ```bash
   etcdctl --endpoints=https://127.0.0.1:2379  --cacert=/opt/KUIN00601/ca.crt --cert=/opt/KUIN00601/etcd-client.crt --key=/opt/KUIN00601/etcd-client.key  snapshot restore /var/lib/etcd-snapshot-previous.db
   ```

   

### 第六题：网络策略

**题目：**

![image.png](https://raw.githubusercontent.com/tiansy9981/images/master/1651029880274262.png?token=AEL2C6YKDUY5M37LEEG5GA3F62DRU)

**解题思路：**

1. 查看标签 ，是否有internal名称空间

   ```bash
   kubectl  get ns --show-labels  #检查已经存在的名称空间是否在
   ```

2. 创建networkpolicy

   ```yaml
   apiVersion: networking.k8s.io/v1
   kind: NetworkPolicy
   metadata:
    name: allow-port-from-namespace
    namespace: my-app
   spec:
     podSelector:
       matchLabels: {}
     policyTypes:
      - Ingress
     ingress:
      - from:
        - namespaceSelector:
            matchLabels:
             project: echo
        ports:
         - protocol: TCP
           port: 9000
   ```

3. 启动networkpolicy

   ```bash
   kubectl apply -f  *.yaml
   ```






### 第七题：service四层代理

**题目：**

![b66fc3d0618f109ec20f637b53b5364.png](https://raw.githubusercontent.com/tiansy9981/images/master/1651031005707174.png?token=AEL2C62V6YPPTX66B4O2PULF62DTE)

**解题思路：**

1. 编辑deployment front-end

   ```bash
   kubectl edit deployment  front-end
   ```

2. 新建front-end-svc的service

   ```yaml
   kubectl expose deploy front-end --name=front-end-svc --port=80  --target-port=http  --type=NodePort
   ```












### 第八题： ingress代理

**题目：**

![image-20230402075623576](https://raw.githubusercontent.com/tiansy9981/images/master/image-20230402075623576.png?token=AEL2C63K3ALIUK4V4BGR5GLF62DTU)

**解题思路：**

**老的解法**

1. 检查名称空间

   ```bash
   kubectl get ns --show-labels
   #没有就创建
   kubectl create ns ing-internal
   ```

2. 创建ingress

   ```yaml
   apiVersion: networking.k8s.io/v1
   kind: Ingress
   metadata:
     name: pong
     namespace: ing-internal
   spec:
     ingressClassName: "nginx"
     rules: 
     - http:
        paths:
        - path: /hello
          pathType: Prefix
          backend:
            service:
             name: hello
             port: 
              number: 5678
   ```

3. 验证

   ```bash
   kubectl apply -f *.yaml 
   kubectl  get ingress  -n ing-internal
   curl -kL <INTERNAL_IP>/hello
   ```



**新的解法：**

1. 查看有nginx的ingress controller

   ```bash
   kubectl get pod -n ingress-nginx
   ```

2. 新建ingress-class

   ```yaml
   apiVersion: networking.k8s.io/v1
   kind: IngressClass
   metadata:
     labels:
       app.kubernetes.io/component: controller
     name: nginx-example
     namespace: ing-internal
     annotations:
       ingressclass.kubernetes.io/is-default-class: "true"
   spec:
     controller: k8s.io/ingress-nginx
   ```
   
3. 新建ingress的yaml文件

   ```yaml
   apiVersion: networking.k8s.io/v1
   kind: Ingress
   metadata:
     name: pong
     namespace: ing-internal
     annotations:
       nginx.ingress.kubernetes.io/rewrite-target: /
   spec:
     ingressClassName: nginx-example
     rules:
     - http:
         paths:
         - path: /hello
           pathType: Prefix
           backend:
             service:
               name: hello
               port:
                 number: 5678
   ```

4. 验证

   ```bash
   kubectl apply -f *.yaml 
   kubectl  get ingress  -n ing-internal
   curl -kL <INTERNAL_IP>/hello
   ```



### 第九题：deployment 管理pod扩缩容

**题目：**

![a1354d92b164d1922250681434e4c55.png](https://raw.githubusercontent.com/tiansy9981/images/master/1651045196697423.png?token=AEL2C65GXQYXGPIAFB3HA73F62DUA)

**解题思路：**

1. 修改edit  deployment

   ```bash
   kubectl edit deployment  loadbalancer   #将replicas修改成6，保存退出即可
   ```

2. 命令行修改

   ```bash
   kubectl scale --replicas=6 deployment loadbalancer
   ```

   

### 第十题：将pod调度到指定节点

**题目：**

![cfcaf31a4a34aeab56223e383b9a6f7.png](https://raw.githubusercontent.com/tiansy9981/images/master/1651048576121439.png?token=AEL2C64FA3MM37AACIPT6MLF62DUQ)

**解题思路：**

1. 创建pod，并指定nodeselecor

   ```yaml
   apiVersion: v1
   kind: Pod
   metadata:
     name: nginx-kusc00401
   spec:
     containers:
     - name: nginx
       image: nginx
     nodeSelector:
       disk: spinning
   ```

2.  创建pod，并查看

   ```bash
   kubectl apply -f *.yaml 
   kubectl get pods -owide
   ```

   

### 第十一题：检查节点ready状态

**题目：**

![110ee505fb8dfe2e058f7540ee31069.png](https://raw.githubusercontent.com/tiansy9981/images/master/1651048731969789.png?token=AEL2C6644OSUMRMMAPZHANDF62DU4)

**解题思路：**

1. 查看node状态并grep

   ```bash
   kubectl get nodes |grep  -w "Ready"|wc -l
   ```

2. 查看污点是不被调度的node

   ```bash
   kubectl describe nodes  * *    |grep 'Taint'|grep 'NoSchedule'|wc -l
   ```

3. 将计算后的数写入文件中

   ```bash
   echo '*' /opt/KUSC00402/kuscC00402.txt
   ```

   

### 第十二题：pod封装多个容器

**题目：**

![image.png](https://raw.githubusercontent.com/tiansy9981/images/master/1651048877140198.png?token=AEL2C63QGFRC76AAQ7I5J6DF62DVK)

**解题思路：**

```bash
vim  12.yaml
apiVersion: v1
kind: pod
metadata:
  name: kucc1
spec:
  containers:
  - name: nginx
    image: nginx
  - name: redis
    image: redis
  - name: memcached
    image: memcached
  - name: consul
    image: consul
```

```bash
kubectl apply -f 12.yaml
```



### 第十三题：持久化存储

**题目：**

![image.png](https://raw.githubusercontent.com/tiansy9981/images/master/1651048951917070.png?token=AEL2C6YVIN535MOI6KL4GZTF62DV4)

**解题思路：**

```bash
vim 13.yaml
apiVersion: v1
kind:  PersistentVolume
metadata:
  name: app-config
spec:
  capacity:
   storage: 2Gi
  accessModes:
  - ReadWriteMany
  hostPath:
    path: "/srv/app-config"
```

```bash
kubectl apply -f 13.yaml
```



### 第十四题： 持久化卷

**题目：**

![76e66803749578b38271bb65896170a.png](https://raw.githubusercontent.com/tiansy9981/images/master/1651049023128524.png?token=AEL2C6ZRXV7GAYQCAVEMFXLF62DWK)

**解题思路：**

```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: pv-volume
spec:
  storageClassName: "csi-hostpath-sc"
  accessModes: 
   - ReadWriteOnce
  resources:
   requests:
    storage: 10Mi
---
apiVersion: v1
kind: Pod
metadata:
  name: "web-server"
spec:
  volumes:
   - name: pv-volume
     persistentVolumeClaim:
      claimName: pv-volume
  containers:
   - name: nginx
     image:  nginx
     volumeMounts:
      - mountPath: "/usr/share/nginx/html"
        name: pv-volume
     
```

```bash
kubectl -f 14.yaml
kubectl edit pvc pv-volume  --record  #将10Mi改为70Mi
```



### 第十五题：查看pod日志

题目：

![f79d5437b2afd6d912d8b5f6460ac1a.png](https://raw.githubusercontent.com/tiansy9981/images/master/1651049091558016.png?token=AEL2C6YMTHX2JMDDGKCDUUDF62DWY)

**解题思路：**

```bash
kubectl logs foobar |grep unable-access-website  > /opt/KUTR00101/foobar
```



### 第十六题： Sidecar代理

**题目：**

![c7b9da9b901adbbfd137566842adb92.png](https://raw.githubusercontent.com/tiansy9981/images/master/1651049134304598-1680400554548-36.png?token=AEL2C634FMYGE4QXEJBQJ4DF62DXI)

**解题思路：**

1. 获取streamling sidecar容器的yaml文件，删除多余的内容只保留container

   ```bash
   kubectl get po -o yaml > new.yaml
   ```

2. 增加volume，并挂在卷,然后增加一个容器并执行要求的命令

   ```yaml
   apiVersion: v1
   kind: Pod
   metadata:
     name: legacy-app
   spec:
     containers:
     - name: count
       image: busybox:1.28
       args:
       - /bin/sh
       - -c
       - >
         i=0;
         while true;
         do
           echo "$(date) INFO $i" >> /var/log/legacy-app.log;
           i=$((i+1));
           sleep 1;
         done
   
   
       image: busybox
       imagePullPolicy: Always
       name: count
       volumeMounts:
       - name: logs
         mountPath: /var/log
     - name: sidecar
       image: busybox
       args: [ '/bin/sh','-c','tail -n+1 -f /var/log/legacy-app.log']
       volumeMounts:
       - name: logs
         mountPath: /var/log
     volumes:
     - name: logs
       emptyDir: {}
   
   ```

3. 验证

   ```bash
   kubectl delete -f 16.yaml  --force --grace-period=0
   ```

   

### 第十七题：查看pod的cpu使用率

**题目：**

![4b981c2f7728c83d65ae63f9ad34a7b.png](https://raw.githubusercontent.com/tiansy9981/images/master/1651049185364958-1680400551383-34.png?token=AEL2C6332Z6NUC324QWNFADF62DXW)

**解题思路：**

```bash
kubectl top pod -l name=cpu-user --sort-by=cpu -A
#将查出来的pod名字写入文件中
```



### 第十八题：集群故障排查

**题目：**

![3e082262bd5ebda4e4e632e82878363.png](https://raw.githubusercontent.com/tiansy9981/images/master/1651049439141942.png?token=AEL2C65MEELM5UUVRFMIURDF62DYE)

**解题思路：**

```bash
#远程到对应节点上
#查看kubelet的状态
#重启kubelet
#设置kubelet自启动
```

