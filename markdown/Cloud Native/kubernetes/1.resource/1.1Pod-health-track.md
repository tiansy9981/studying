# Pod容器健康探测

 

## 1.1 为什么要对容器做探测？

在 Kubernetes 中 Pod 是最小的计算单元，而一个 Pod 又由多个容器组成，相当于每个容器就是一个应用，应用在运行期间，可能因为某些意外情况致使程序挂掉。那么如何监控这些容器状态稳定性，保证服务在运行期间不会发生问题，发生问题后进行重启等机制，就成为了重中之重的事情，考虑到这点 kubernetes 推出了活性探针机制。有了存活性探针能保证程序在运行中如果挂掉能够自动重启，但是还有个经常遇到的问题，比如说，在Kubernetes 中启动Pod，显示明明Pod已经启动成功，且能访问里面的端口，但是却返回错误信息。还有就是在执行滚动更新时候，总会出现一段时间，Pod对外提供网络访问，但是访问却发生404，这两个原因，都是因为Pod已经成功启动，但是 Pod 的的容器中应用程序还在启动中导致，考虑到这点Kubernetes推出了就绪性探针机制。

 

1. 默认的健康检查

[Kubernetes](https://so.csdn.net/so/search?q=Kubernetes&spm=1001.2101.3001.7020)默认的健康检查机制： 每个容器启动时都会执行一个进程， 此进程由Dockerfile的CMD或ENTRYPOINT指定。 如果进程退出时返回码非零， 则认为容器发生故障， Kubernetes就会根据restartPolicy策略决定是否重启容器。

```yaml
[root@xianchaomaster1]# cat check.yaml 
apiVersion: v1
kind: Pod
metadata:
  name: check
  namespace: default
  labels:
    app: check
spec:
  containers:
  - name: check
    image: busybox:1.28
    imagePullPolicy: IfNotPresent
    command:
    - /bin/sh
    - -c
    - sleep 10;exit

```

 

```bash
[root@xianchaomaster1 ~]# kubectl get pods -w
NAME        READY   STATUS    RESTARTS   AGE
check       0/1     Pending   0          0s
check       0/1     Pending   0          0s
check       0/1     ContainerCreating   0          0s
check       0/1     ContainerCreating   0          1s
check       1/1     Running             0          1s
check       0/1     Completed           0          11s
check       1/1     Running             1 (1s ago)   12s
check       0/1     Completed           1 (11s ago)   22s
check       0/1     CrashLoopBackOff    1 (13s ago)   34s
check       1/1     Running             2 (13s ago)   34s
check       0/1     Completed           2 (23s ago)   44s

```

在上面的例子中， 容器进程返回值非零， Kubernetes则认为容器发生故障， 需要重启。 有不少情况是发生了故障， 但进程并不会退出。 比如访问Web服务器时显示500内部错误， 可能是系统超载， 也可能是资源死锁， 此时httpd进程并没有异常退出， 在这种情况下重启容器可能是最直接、 最有效的解决方案。

 k8s 提供了三种来实现容器探测的方法，分别是：

 1、startupProbe：探测容器中的应用是否已经启动。如果提供了启动探测(startup probe)，则禁用所有其他探测，直到它成功为止。如果启动探测失败，kubelet 将杀死容器，容器服从其重启策略进行重启。如果容器没有提供启动探测，则默认状态为成功Success。

  2、livenessprobe：用指定的方式（exec、tcp、http）检测pod中的容器是否正常运行，如果检测失败，则认为容器不健康，那么Kubelet将根据Pod中设置的 restartPolicy策略来判断Pod 是否要进行重启操作，如果容器配置中没有配置 livenessProbe，Kubelet 将认为存活探针探测一直为success（成功）状态。

 3、readnessprobe：就绪性探针，用于检测容器中的应用是否可以接受请求，当探测成功后才使Pod对外提供网络访问，将容器标记为就绪状态，可以加到pod前端负载，如果探测失败，则将容器标记为未就绪状态，会把pod从前端负载移除。

 

可以自定义在pod启动时是否执行这些检测，如果不设置，则检测结果均默认为通过，如果设置，则顺序为startupProbe>readinessProbe和livenessProbe，readinessProbe和livenessProbe是并发关系

真正的启动顺序

官方文档：Caution: Liveness probes do not wait for readiness probes to succeed. If you want to wait before executing a liveness probe you should use initialDelaySeconds or a startupProbe



也就是 Liveness probes 并不会等到 Readiness probes 成功之后才运行，根据上面的官方文档，Liveness 和 readiness 应该是某种并发的关系。

目前LivenessProbe和ReadinessProbe、startupprobe探测都支持下面三种探针：

1、exec：在容器中执行指定的命令，如果执行成功，退出码为 0 则探测成功。

2、TCPSocket：通过容器的 IP 地址和端口号执行 TCP 检 查，如果能够建立 TCP 连接，则表明容器健康。

3、HTTPGet：通过容器的IP地址、端口号及路径调用 HTTP Get方法，如果响应的状态码大于等于200且小于400，则认为容器健康



探针探测结果有以下值：

1、Success：表示通过检测。

2、Failure：表示未通过检测。

3、Unknown：表示检测没有正常进行。

 

```
Pod探针相关的属性：

探针(Probe)有许多可选字段，可以用来更加精确的控制Liveness和Readiness两种探针的行为
initialDelaySeconds：容器启动后要等待多少秒后探针开始工作，单位“秒”，默认是 0 秒，最小值是 0 
periodSeconds： 执行探测的时间间隔（单位是秒），默认为 10s，单位“秒”，最小值是1
timeoutSeconds： 探针执行检测请求后，等待响应的超时时间，默认为1，单位“秒”。
successThreshold：连续探测几次成功，才认为探测成功，默认为 1，在 Liveness 探针中必须为1，最小值为1。
failureThreshold： 探测失败的重试次数，重试一定次数后将认为失败，在 readiness 探针中，Pod会被标记为未就绪，默认为 3，最小值为 1
```

 

 两种探针区别：

```
ReadinessProbe 和 livenessProbe 可以使用相同探测方式，只是对 Pod 的处置方式不同：
readinessProbe 当检测失败后，将 Pod 的 IP:Port 从对应的 EndPoint 列表中删除。
livenessProbe 当检测失败后，将杀死容器并根据 Pod 的重启策略来决定作出对应的措施。
```

 

## 1.2 启动探测startupprobe

**1、exec模式**

```bash
[root@xianchaomaster1]# cat startup-exec.yaml 
apiVersion: v1
kind: Pod
metadata:
  name: startupprobe
spec:
  containers:
  - name: startup
image: xianchao/tomcat-8.5-jre8:v1
imagePullPolicy: IfNotPresent
    ports:
    - containerPort: 8080
    startupProbe:
     exec:
       command:
       - "/bin/sh"
       - "-c"
       - "aa ps aux | grep tomcat"
     initialDelaySeconds: 20 #容器启动后多久开始探测
     periodSeconds: 20 #执行探测的时间间隔
     timeoutSeconds: 10 #探针执行检测请求后，等待响应的超时时间
     successThreshold: 1 #成功多少次才算成功
     failureThreshold: 3 #失败多少次才算失败
```

 

**2、tcpsocket模式**

```bash
[root@xianchaomaster1]# cat startup-tcpsocket.yaml 
apiVersion: v1
kind: Pod
metadata:
  name: startupprobe
spec:
  containers:
  - name: startup
image: xianchao/tomcat-8.5-jre8:v1
imagePullPolicy: IfNotPresent
    ports:
    - containerPort: 8080
    startupProbe:
     tcpSocket:
       port: 8080
     initialDelaySeconds: 20 #容器启动后多久开始探测
     periodSeconds: 20 #执行探测的时间间隔
     timeoutSeconds: 10 #探针执行检测请求后，等待响应的超时时间
     successThreshold: 1 #成功多少次才算成功
     failureThreshold: 3 #失败多少次才算失败
```

 

**3、httpget模式**

```bash
[root@xianchaomaster1]# cat startup-httpget.yaml 
apiVersion: v1
kind: Pod
metadata:
  name: startupprobe
spec:
  containers:
  - name: startup
image: xianchao/tomcat-8.5-jre8:v1
imagePullPolicy: IfNotPresent
    ports:
    - containerPort: 8080
    startupProbe:
      httpGet:
        path: /
        port: 8080
     initialDelaySeconds: 20 #容器启动后多久开始探测
     periodSeconds: 20 #执行探测的时间间隔
     timeoutSeconds: 10 #探针执行检测请求后，等待响应的超时时间
     successThreshold: 1 #成功多少次才算成功
     failureThreshold: 3 #失败多少次才算失败
```

 

## 1.3 存活性探测livenessProbe

 

官网地址：https://kubernetes.io/zh-cn/docs/tasks/configure-pod-container/configure-liveness-readiness-startup-probes/



**Pod探针使用示例：**

1. **LivenessProbe 探针使用示例**

   ​	**通过exec方式做健康探测**
    示例文件 liveness-exec.yaml

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: liveness-exec
  labels:
    app: liveness
spec:
  containers:
  - name: liveness
image: busybox:1.28
imagePullPolicy: IfNotPresent
    args:                       #创建测试探针探测的文件
    - /bin/sh
    - -c
    - touch /tmp/healthy; sleep 30; rm -rf /tmp/healthy; sleep 600
    livenessProbe:
      initialDelaySeconds: 10   #延迟检测时间
      periodSeconds: 5          #检测时间间隔
      exec:
        command:
        - cat
        - /tmp/healthy 
```



容器启动设置执行的命令：

` /bin/sh -c "touch /tmp/healthy; sleep 30; rm -rf /tmp/healthy; sleep 600"`

容器在初始化后，首先创建一个 /tmp/healthy 文件，然后执行睡眠命令，睡眠 30 秒，到时间后执行删除 /tmp/healthy 文件命令。而设置的存活探针检检测方式为执行 shell 命令，用 cat 命令输出 healthy 文件的内容，如果能成功执行这条命令，存活探针就认为探测成功，否则探测失败。在前 30 秒内，由于文件存在，所以存活探针探测时执行 cat /tmp/healthy 命令成功执行。30 秒后 healthy 文件被删除，所以执行命令失败，Kubernetes 会根据 Pod 设置的重启策略来判断，是否重启 Pod。

 

2. 通过HTTP方式做健康探测

`[root@xianchaonode1 ~]# ctr -n k8s.io images import springboot.tar.gz`

`[root@xianchaonode2 ~]# ctr -n k8s.io images import springboot.tar.gz`
 示例文件 liveness-http.yaml

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: liveness-http
  labels:
    test: liveness
spec:
  containers:
  - name: liveness
image: mydlqclub/springboot-helloworld:0.0.1
imagePullPolicy: IfNotPresent
    livenessProbe:
      initialDelaySeconds: 20   #延迟加载时间
      periodSeconds: 5          #重试时间间隔
      timeoutSeconds: 10        #超时时间设置
      httpGet:
        scheme: HTTP
        port: 8081
        path: /actuator/health
```



上面 Pod 中启动的容器是一个 SpringBoot 应用，其中引用了 Actuator 组件，提供了 /actuator/health 健康检查地址，存活探针可以使用 HTTPGet 方式向服务发起请求，请求 8081 端口的 /actuator/health 路径来进行存活判断：

任何大于或等于200且小于400的代码表示探测成功。

任何其他代码表示失败。

如果探测失败，则会杀死 Pod 进行重启操作。


 httpGet探测方式有如下可选的控制字段:

```
scheme: 用于连接host的协议，默认为HTTP。
host：要连接的主机名，默认为Pod IP，可以在http request head中设置host头部。
port：容器上要访问端口号或名称。
path：http服务器上的访问URI。
httpHeaders：自定义HTTP请求headers，HTTP允许重复headers。 
```

3. 通过TCP方式做健康探测

   示例文件 liveness-tcp.yaml

```bash
apiVersion: v1
kind: Pod
metadata:
  name: liveness-tcp
  labels:
    app: liveness
spec:
  containers:
  - name: liveness
image: docker.io/xianchao/nginx:v1
imagePullPolicy: IfNotPresent
    livenessProbe:
      initialDelaySeconds: 15
      periodSeconds: 20
      tcpSocket:
        port: 80
```



TCP 检查方式和 HTTP 检查方式非常相似，在容器启动 initialDelaySeconds 参数设定的时间后，kubelet 将发送第一个 livenessProbe 探针，尝试连接容器的 80 端口，如果连接失败则将杀死 Pod 重启容器。

 

## 1.4就绪性探测readinessProbe

 

1. **ReadinessProbe 探针使用示例** 

Pod 的ReadinessProbe 探针使用方式和 LivenessProbe 探针探测方法一样，也是支持三种，只是一个是用于探测应用的存活，一个是判断是否对外提供流量的条件。这里用一个 Springboot 项目，设置 ReadinessProbe 探测 SpringBoot 项目的 8081 端口下的 /actuator/health 接口，如果探测成功则代表内部程序以及启动，就开放对外提供接口访问，否则内部应用没有成功启动，暂不对外提供访问，直到就绪探针探测成功。

示例文件 readiness-exec.yaml

```yaml
apiVersion: v1
kind: Service
metadata:
  name: springboot
  labels:
    app: springboot
spec:
  type: NodePort
  ports:
  - name: server
    port: 8080
    targetPort: 8080
    nodePort: 31180
  - name: management
    port: 8081
    targetPort: 8081
    nodePort: 31181
  selector:
    app: springboot
---
apiVersion: v1
kind: Pod
metadata:
  name: springboot
  labels:
    app: springboot
spec:
  containers:
  - name: springboot
image: mydlqclub/springboot-helloworld:0.0.1
imagePullPolicy: IfNotPresent
    ports:
    - name: server
      containerPort: 8080
    - name: management
      containerPort: 8081
    readinessProbe:
      initialDelaySeconds: 20   
      periodSeconds: 5          
      timeoutSeconds: 10   
      httpGet:
        scheme: HTTP
        port: 8081
        path: /actuator/health
```

 

## 1.5 ReadinessProbe + LivenessProbe +startupProbe配合

使用示例  

一般程序中需要设置三种探针结合使用，并且也要结合实际情况，来配置初始化检查时间和检测间隔，下面列一个简单的 SpringBoot 项目的例子。

```yaml
[root@xianchaomaster1]# cat start-read-live.yaml 
apiVersion: v1
kind: Service
metadata:
  name: springboot-live
  labels:
    app: springboot
spec:
  type: NodePort
  ports:
  - name: server
    port: 8080
    targetPort: 8080
    nodePort: 31180
  - name: management
    port: 8081
    targetPort: 8081
    nodePort: 31181
  selector:
    app: springboot
---
apiVersion: v1
kind: Pod
metadata:
  name: springboot-live
  labels:
    app: springboot
spec:
  containers:
  - name: springboot
    image: mydlqclub/springboot-helloworld:0.0.1
    imagePullPolicy: IfNotPresent
    ports:
    - name: server
      containerPort: 8080
    - name: management
      containerPort: 8081
    readinessProbe:
      initialDelaySeconds: 20   
      periodSeconds: 5          
      timeoutSeconds: 10   
      httpGet:
        scheme: HTTP
        port: 8081
        path: /actuator/health
    livenessProbe:
      initialDelaySeconds: 20
      periodSeconds: 5
      timeoutSeconds: 10
      httpGet:
        scheme: HTTP
        port: 8081
        path: /actuator/health
    startupProbe:
      initialDelaySeconds: 20
      periodSeconds: 5
      timeoutSeconds: 10
      httpGet:
        scheme: HTTP
        port: 8081
        path: /actuator/health
```

