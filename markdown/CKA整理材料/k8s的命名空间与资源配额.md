# k8s的命名空间与资源配额

## 1 什么是命名空间？



Kubernetes 支持多个虚拟集群，它们底层依赖于同一个物理集群。 这些虚拟集群被称为命名空间。

命名空间namespace是k8s集群级别的资源，可以给不同的用户、租户、环境或项目创建对应的命名空间，例如，可以为test、devlopment、production环境分别创建各自的命名空间。

 

## 2 namespace应用场景



命名空间适用于存在很多跨多个团队或项目的用户的场景。对于只有几到几十个用户的集群，根本不需要创建或考虑命名空间。



## 3 namespacs使用案例分享

 

**创建一个test命名空间**

`[root@xianchaomaster1~]# kubectl create ns test`

  

## 4 namespace资源限额

**如何对namespace资源做限额呢？**

```yaml
[root@xianchaomaster1~]# vim namespace-quota.yaml

apiVersion: v1
kind: ResourceQuota
metadata:
 name: mem-cpu-quota
 namespace: test
spec:
 hard:
  requests.cpu: "2"
  requests.memory: 2Gi
  limits.cpu: "4"
  limits.memory: 4Gi
```

`[root@xianchaomaster1]# kubectl apply -f namespace-quota.yaml`

**创建的ResourceQuota对象将在test名字空间中添加以下限制：**

每个容器必须设置内存请求（memory request），内存限额（memory limit），cpu请求（cpu request）和cpu限额（cpu limit）。

  所有容器的内存请求总额不得超过2GiB。

  所有容器的内存限额总额不得超过4 GiB。

  所有容器的CPU请求总额不得超过2 CPU。

  所有容器的CPU限额总额不得超过4CPU。



**ResouceQuota 对象是在我们的名称空间中创建的，并准备好控制该名称空间中的所有容器的总请求和限制。让我们看看** 

**创建pod时候必须设置资源限额，否则创建失败，如下：**

```yaml
[root@xianchaomaster1 ~]# vim pod-test.yaml

apiVersion: v1
kind: Pod
metadata:
 name: pod-test
 namespace: test
 labels:
  app: tomcat-pod-test
spec:
 containers:
 - name: tomcat-test
  ports:
  - containerPort: 8080
  image: xianchao/tomcat-8.5-jre8:v1
  imagePullPolicy: IfNotPresent
  resources:
   requests:
    memory: "100Mi"
    cpu: "500m"
   limits:
    memory: "2Gi"
    cpu: "2"
```

`[root@xianchaomaster1 ~]# kubectl apply -f pod-test.yaml`