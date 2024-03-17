# k8s相关知识

### 设置名字空间偏好

你可以永久保存名字空间，以用于对应上下文中所有后续 kubectl 命令。

```bash
kubectl config set-context --current --namespace=<名字空间名称>
# 验证
kubectl config view --minify | grep namespace:
```

### 并非所有对象都在名字空间中

大多数 kubernetes 资源（例如 Pod、Service、副本控制器等）都位于某些名字空间中。 但是名字空间资源本身并不在名字空间中。而且底层资源， 例如[节点](https://www.bookstack.cn/read/kubernetes-1.25-zh/e9abdda192abb168.md)和持久化卷不属于任何名字空间。

查看哪些 Kubernetes 资源在名字空间中，哪些不在名字空间中：

```bash
# 位于名字空间中的资源
kubectl api-resources --namespaced=true
# 不在名字空间中的资源
kubectl api-resources --namespaced=false
```

### 标签选择算符

```
两种标签选择算符都可以通过 REST 客户端用于 list 或者 watch 资源。 例如，使用 kubectl 定位 apiserver，可以使用基于等值的标签选择算符可以这么写：

kubectl get pods -l environment=production,tier=frontend
或者使用基于集合的需求：

kubectl get pods -l 'environment in (production),tier in (frontend)
正如刚才提到的，基于集合的需求更具有表达力。例如，它们可以实现值的或操作：

kubectl get pods -l 'environment in (production, qa)
或者通过exists运算符限制不匹配：

kubectl get pods -l 'environment,environment notin (frontend)
支持基于集合需求的资源
比较新的资源，例如 Job、 Deployment、 ReplicaSet 和 DaemonSet， 也支持基于集合的需求。

selector:
  matchLabels:
    component: redis
  matchExpressions:
    - {key: tier, operator: In, values: [cache]}
    - {key: environment, operator: NotIn, values: [dev]}
matchLabels 是由 {key,value} 对组成的映射。 matchLabels 映射中的单个 {key,value} 等同于 matchExpressions 的元素， 其 key 字段为 “key”，operator 为 “In”，而 values 数组仅包含 “value”。 matchExpressions 是 Pod 选择算符需求的列表。 有效的运算符包括 In、NotIn、Exists 和 DoesNotExist。 在 In 和 NotIn 的情况下，设置的值必须是非空的。 来自 matchLabels 和 matchExpressions 的所有要求都按逻辑与的关系组合到一起 — 它们必须都满足才能匹配。
```

