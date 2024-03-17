### 安装k8s可视化UI界面dashboard

#### 安装dasboard

把安装kubernetes-dashboard需要的镜像上传到工作节点xianchaonode1和xianchaonode2，手动解压： 

```
[root@xianchaonode1 ~]# docker load -i dashboard_2_0_0.tar.gz

[root@xianchaonode1 ~]# docker load -i metrics-scrapter-1-0-1.tar.gz 

[root@xianchaonode2 ~]# docker load -i dashboard_2_0_0.tar.gz

[root@xianchaonode2 ~]# docker load -i metrics-scrapter-1-0-1.tar.gz
```

**\#安装dashboard组件**

在xianchaomaster1节点操作如下命令:

```
[root@xianchaomaster1 ~]# kubectl apply -f kubernetes-dashboard.yaml
```

**\#查看dashboard的状态**

```
[root@xianchaomaster1 ~]# kubectl get pods -n kubernetes-dashboard
#显示如下，说明dashboard安装成功了

NAME                     READY  STATUS  RESTARTS  AGE

dashboard-metrics-scraper-7445d59dfd-n5krt  1/1   Running  0     66s

kubernetes-dashboard-54f5b6dc4b-mhd2c    1/1   Running  0     66s
```

**\#查看dashboard前端的service**

```
[root@xianchaomaster1 ~]# kubectl get svc -n kubernetes-dashboard

显示如下：

NAME            TYPE    CLUSTER-IP   EXTERNAL-IP  PORT(S)  AGE

dashboard-metrics-scraper  ClusterIP  10.98.221.57  <none>    8000/TCP  

kubernetes-dashboard    ClusterIP  10.97.37.171  <none>    443/TCP
```

**\#修改service type类型变成NodePort**

```
[root@xianchaomaster1 ~]# kubectl edit svc kubernetes-dashboard -n kubernetes-dashboard

```

把type: ClusterIP变成 type: NodePort，保存退出即可。

```
[root@xianchaomaster1 ~]# kubectl get svc -n kubernetes-dashboard

NAME            TYPE    CLUSTER-IP   EXTERNAL-IP  PORT(S)     AGE

dashboard-metrics-scraper  ClusterIP  10.98.221.57  <none>    8000/TCP    

kubernetes-dashboard    NodePort  10.97.37.171  <none>    443:32728/TCP  2m50s

 
```

上面可看到service类型是NodePort，访问任何一个工作节点ip: 32728端口即可访问kubernetes dashboard，在浏览器(使用火狐浏览器)访问如下地址:

https://192.168.40.180:32728/

![image-20230227171049213](https://raw.githubusercontent.com/tiansy9981/images/master/image-20230227171049213.png?token=AEL2C65MLDMZHPXYRSXNJMLFLQWM4)                               

可看到出现了dashboard界面

 ![image-20230227171058089](https://raw.githubusercontent.com/tiansy9981/images/master/image-20230227171058089.png?token=AEL2C66T2RIDHGLMWK3JVI3FLQWNE)

 

备注：安装dashboard之后，通过火狐浏览器访问报错，如下：

 ![image-20230227171108443](https://raw.githubusercontent.com/tiansy9981/images/master/image-20230227171108443.png?token=AEL2C62OMZOLAGBTJG5WVOLFLQWN4)

解决方案：换成google浏览器，通过google浏览器访问看到如下报错：您的连接不是私密链接

 ![image-20230227171118377](https://raw.githubusercontent.com/tiansy9981/images/master/image-20230227171118377.png?token=AEL2C6YZKRFQCSCCWJPXF4LFLQWOK)

在google浏览器中，在上图出现的页面任意位置，直接键盘敲入这11个字符：thisisunsafe，可以直接跳过安全报错进入页面

 ![image-20230227171126309](https://raw.githubusercontent.com/tiansy9981/images/master/image-20230227171126309.png?token=AEL2C6ZFQFHB57EWDAPNNQTFLQWOY)

 

#### 12.2 通过token令牌访问dashboard

**\#通过Token登陆dashboard**

创建管理员token，具有查看任何空间的权限，可以管理所有资源对象

```
[root@xianchaomaster1 ~]# kubectl create clusterrolebinding dashboard-cluster-admin --clusterrole=cluster-admin --serviceaccount=kubernetes-dashboard:kubernetes-dashboard


```

**查看kubernetes-dashboard名称空间下的secret**

```
[root@xianchaomaster1 ~]# kubectl get secret -n kubernetes-dashboard

NAME                TYPE                 DATA  AGE

default-token-n2x5n        kubernetes.io/service-account-token  3   

kubernetes-dashboard-certs     Opaque                0   

kubernetes-dashboard-csrf     Opaque                1   

kubernetes-dashboard-key-holder  Opaque                2   

kubernetes-dashboard-token-ppc8c  kubernetes.io/service-account-token  3   
```

**找到对应的带有token的kubernetes-dashboard-token-ppc8c**

```
[root@xianchaomaster1 ~]# kubectl describe secret kubernetes-dashboard-token-ppc8c -n  kubernetes-dashboard

 

#显示如下：

---

token:   eyJhbGciOiJSUzI1NiIsImtpZCI6IlVNalJOeHBacEYxZ0tieTJKR2ZVY1hvTHZvcVVBU3FPaTU4TGhreDd5VzQifQ.eyJpc3MiOiJrdWJlcm5ldGVzL3NlcnZpY2VhY2NvdW50Iiwia3ViZXJuZXRlcy5pby9zZXJ2aWNlYWNjb3VudC9uYW1lc3BhY2UiOiJrdWJlcm5ldGVzLWRhc2hib2FyZCIsImt1YmVybmV0ZXMuaW8vc2VydmljZWFjY291bnQvc2VjcmV0Lm5hbWUiOiJrdWJlcm5ldGVzLWRhc2hib2FyZC10b2tlbi1wcGM4YyIsImt1YmVybmV0ZXMuaW8vc2VydmljZWFjY291bnQvc2VydmljZS1hY2NvdW50Lm5hbWUiOiJrdWJlcm5ldGVzLWRhc2hib2FyZCIsImt1YmVybmV0ZXMuaW8vc2VydmljZWFjY291bnQvc2VydmljZS1hY2NvdW50LnVpZCI6ImU0NTZlNTgzLWE1MDItNDViNy04YTEwLTE0MThhZmJiZmRlZCIsInN1YiI6InN5c3RlbTpzZXJ2aWNlYWNjb3VudDprdWJlcm5ldGVzLWRhc2hib2FyZDprdWJlcm5ldGVzLWRhc2hib2FyZCJ9.CKSwwXQfwF-HA_giAC1jzTmEnMjA73FtNpMjhI_coLOakc-MBX8f74K4k-TGMfBVFwqcvzBnCBOQJ2OJCgZX-YI109d4PDE9rGO3SK0OGIaLVDafB_9aoMeuMyrsut0PwgZoynUwA_DrwhOFkneNsA93rCIQck1WWOvxbrUtttmhvMDzYSZu6-TcGx6pxEiHwOEQ5dx92Tv6nSIBS_tjHU-CElQMhcHcHsD6AF-WdhSF2QtnvcCGasCcBPXzKF8cqd_QdvZvMgcUuwhUBVRRCEc_MCHDqGQBXZ6EzRE0PoXW_S6Rd1zvzT5SVFrgL3xGikPHTxWP2DurNZuCHmFnMw
```

记住token后面的值，把下面的token值复制到浏览器token登陆处即可登陆：

```
eyJhbGciOiJSUzI1NiIsImtpZCI6IlVNalJOeHBacEYxZ0tieTJKR2ZVY1hvTHZvcVVBU3FPaTU4TGhreDd5VzQifQ.eyJpc3MiOiJrdWJlcm5ldGVzL3NlcnZpY2VhY2NvdW50Iiwia3ViZXJuZXRlcy5pby9zZXJ2aWNlYWNjb3VudC9uYW1lc3BhY2UiOiJrdWJlcm5ldGVzLWRhc2hib2FyZCIsImt1YmVybmV0ZXMuaW8vc2VydmljZWFjY291bnQvc2VjcmV0Lm5hbWUiOiJrdWJlcm5ldGVzLWRhc2hib2FyZC10b2tlbi1wcGM4YyIsImt1YmVybmV0ZXMuaW8vc2VydmljZWFjY291bnQvc2VydmljZS1hY2NvdW50Lm5hbWUiOiJrdWJlcm5ldGVzLWRhc2hib2FyZCIsImt1YmVybmV0ZXMuaW8vc2VydmljZWFjY291bnQvc2VydmljZS1hY2NvdW50LnVpZCI6ImU0NTZlNTgzLWE1MDItNDViNy04YTEwLTE0MThhZmJiZmRlZCIsInN1YiI6InN5c3RlbTpzZXJ2aWNlYWNjb3VudDprdWJlcm5ldGVzLWRhc2hib2FyZDprdWJlcm5ldGVzLWRhc2hib2FyZCJ9.CKSwwXQfwF-HA_giAC1jzTmEnMjA73FtNpMjhI_coLOakc-MBX8f74K4k-TGMfBVFwqcvzBnCBOQJ2OJCgZX-YI109d4PDE9rGO3SK0OGIaLVDafB_9aoMeuMyrsut0PwgZoynUwA_DrwhOFkneNsA93rCIQck1WWOvxbrUtttmhvMDzYSZu6-TcGx6pxEiHwOEQ5dx92Tv6nSIBS_tjHU-CElQMhcHcHsD6AF-WdhSF2QtnvcCGasCcBPXzKF8cqd_QdvZvMgcUuwhUBVRRCEc_MCHDqGQBXZ6EzRE0PoXW_S6Rd1zvzT5SVFrgL3xGikPHTxWP2DurNZuCHmFnMw
```

点击sing in登陆，显示如下，这次就可以看到和操作任何名称空间的资源了

![image-20230227171449698](https://raw.githubusercontent.com/tiansy9981/images/master/image-20230227171449698.png?token=AEL2C65GEJITJA4WCDHF5QTFLQWPK)

#### 通过kubeconfig文件访问dashboard

```
[root@xianchaomaster1 ~]# cd /etc/kubernetes/pki
```

**1、创建cluster集群**

```
[root@xianchaomaster1 pki]# kubectl config set-cluster kubernetes --certificate-authority=./ca.crt --server="https://192.168.40.180:6443" --embed-certs=true --kubeconfig=/root/dashboard-admin.conf

[root@xianchaomaster1 pki]# cat /root/dashboard-admin.conf 

apiVersion: v1

clusters:

- cluster:

  certificate-authority-data: LS0tLS1CRUdJTiBDRVJUSUZJQ0FURS0tLS0tCk1JSUM1ekNDQWMrZ0F3SUJBZ0lCQURBTkJna3Foa2lHOXcwQkFRc0ZBREFWTVJNd0VRWURWUVFERXdwcmRXSmwKY201bGRHVnpNQjRYRFRJeE1EVXdNVEEzTURRd01sb1hEVE14TURReU9UQTNNRFF3TWxvd0ZURVRNQkVHQTFVRQpBeE1LYTNWaVpYSnVaWFJsY3pDQ0FTSXdEUVlKS29aSWh2Y05BUUVCQlFBRGdnRVBBRENDQVFvQ2dnRUJBTXRUCktUYUF0RENySXB3VUtPUnhlbTRUYk9WS0JTM3NZaHBOTjE0TjAyUzI4Tnh5Y2oyUjVhVlF2cTF6aGJZYmVnZloKNm81UHAzMk4ydGEzQW5WMmpmT3h2OTFPR2FYL3NIT2hOZG5GQVpiZXdmZmJWVFhrbURMSnBIRmtYeUVubnltbQpRaDY2Rkl6S1IxSWZaLzFLaXRjM1dSQlpZSjlMRU53UjBWaFcyeUNRbmdwMEhtQ1hBcllZYTFFNzdnOHI5Vk5ZCjdrRm9ma2dla1BZWEhFeGxVS3dQVVcxSDd1ejRkVXlGZEorR1VjTE9WOXJhdWszaWMwazg4dG9ZdlNIelhjR3AKR0ZNS2FrMkhLYTF5RUkveGZqUFJPMDlRRmdvSTdTTFh4RG0yNHNuNUhFWHhzaTgrNFR2a2ZRVHRnZGNtRDNVRApWTzJrNHVFV1I0V1Axb0lYQnlzQ0F3RUFBYU5DTUVBd0RnWURWUjBQQVFIL0JBUURBZ0trTUE4R0ExVWRFd0VCCi93UUZNQU1CQWY4d0hRWURWUjBPQkJZRUZKTUJTREpZdXpIR1JmVWZQMWxBM0dpbGk5ZXlNQTBHQ1NxR1NJYjMKRFFFQkN3VUFBNElCQVFBK2REQnZpTk0rcXFnaFJtZUxNQUw2QUdBM1ZZWlByUkZaajI2RUI0R2FvMU01b1JtTgpLcmZQKzhQNzJtRmkycDRCR2xWTUN2TjhUSy8xZW1DWVc0MkZYS0YzOE5zVUhqVFVmZUlxaHJBSW9WWWVvRDlsCjFOeU42QkVkdWJSWHhjejByV3pMVkZMYUROMzhCcERzUGJMSk5qOGZmWUd0SzJnQlBNRDdRWWxabHpldnNRYzkKclczVGFHUGk2OVNJc0RGbGZvbnV1aEFqTzNqZVJ6VURzQmIxblIzRGpRQUFBMnRITDJOQVFXaW8zWlNBd3Q4Rwo4UFRYL20waWdaWjNiaHA3UjV5cEFNbWR3ODVEak1BMkhxQ0hFRjlvc1MxUnowQWdaK2VXWkNBZUtmclhKWW1kCkZuUjBYNU9TSlUybjhCRkVUbE8wa3N4QTVWVjJyUDNJb1NmNAotLS0tLUVORCBDRVJUSUZJQ0FURS0tLS0tCg==

  server: https://192.168.40.180:6443

 name: kubernetes

contexts: null

current-context: ""

kind: Config

preferences: {}

users: null
```

**2、创建credentials**

创建credentials需要使用上面的kubernetes-dashboard-token-ppc8c对应的token信息

```
[root@xianchaomaster1 pki]# DEF_NS_ADMIN_TOKEN=$(kubectl get secret kubernetes-dashboard-token-ppc8c -n kubernetes-dashboard -o jsonpath={.data.token}|base64 -d)

 

[root@xianchaomaster1 pki]# kubectl config set-credentials dashboard-admin --token=$DEF_NS_ADMIN_TOKEN --kubeconfig=/root/dashboard-admin.conf

 

[root@xianchaomaster1 pki]# cat /root/dashboard-admin.conf 

apiVersion: v1

clusters:

- cluster:

  certificate-authority-data: LS0tLS1CRUdJTiBDRVJUSUZJQ0FURS0tLS0tCk1JSUM1ekNDQWMrZ0F3SUJBZ0lCQURBTkJna3Foa2lHOXcwQkFRc0ZBREFWTVJNd0VRWURWUVFERXdwcmRXSmwKY201bGRHVnpNQjRYRFRJeE1EVXdNVEEzTURRd01sb1hEVE14TURReU9UQTNNRFF3TWxvd0ZURVRNQkVHQTFVRQpBeE1LYTNWaVpYSnVaWFJsY3pDQ0FTSXdEUVlKS29aSWh2Y05BUUVCQlFBRGdnRVBBRENDQVFvQ2dnRUJBTXRUCktUYUF0RENySXB3VUtPUnhlbTRUYk9WS0JTM3NZaHBOTjE0TjAyUzI4Tnh5Y2oyUjVhVlF2cTF6aGJZYmVnZloKNm81UHAzMk4ydGEzQW5WMmpmT3h2OTFPR2FYL3NIT2hOZG5GQVpiZXdmZmJWVFhrbURMSnBIRmtYeUVubnltbQpRaDY2Rkl6S1IxSWZaLzFLaXRjM1dSQlpZSjlMRU53UjBWaFcyeUNRbmdwMEhtQ1hBcllZYTFFNzdnOHI5Vk5ZCjdrRm9ma2dla1BZWEhFeGxVS3dQVVcxSDd1ejRkVXlGZEorR1VjTE9WOXJhdWszaWMwazg4dG9ZdlNIelhjR3AKR0ZNS2FrMkhLYTF5RUkveGZqUFJPMDlRRmdvSTdTTFh4RG0yNHNuNUhFWHhzaTgrNFR2a2ZRVHRnZGNtRDNVRApWTzJrNHVFV1I0V1Axb0lYQnlzQ0F3RUFBYU5DTUVBd0RnWURWUjBQQVFIL0JBUURBZ0trTUE4R0ExVWRFd0VCCi93UUZNQU1CQWY4d0hRWURWUjBPQkJZRUZKTUJTREpZdXpIR1JmVWZQMWxBM0dpbGk5ZXlNQTBHQ1NxR1NJYjMKRFFFQkN3VUFBNElCQVFBK2REQnZpTk0rcXFnaFJtZUxNQUw2QUdBM1ZZWlByUkZaajI2RUI0R2FvMU01b1JtTgpLcmZQKzhQNzJtRmkycDRCR2xWTUN2TjhUSy8xZW1DWVc0MkZYS0YzOE5zVUhqVFVmZUlxaHJBSW9WWWVvRDlsCjFOeU42QkVkdWJSWHhjejByV3pMVkZMYUROMzhCcERzUGJMSk5qOGZmWUd0SzJnQlBNRDdRWWxabHpldnNRYzkKclczVGFHUGk2OVNJc0RGbGZvbnV1aEFqTzNqZVJ6VURzQmIxblIzRGpRQUFBMnRITDJOQVFXaW8zWlNBd3Q4Rwo4UFRYL20waWdaWjNiaHA3UjV5cEFNbWR3ODVEak1BMkhxQ0hFRjlvc1MxUnowQWdaK2VXWkNBZUtmclhKWW1kCkZuUjBYNU9TSlUybjhCRkVUbE8wa3N4QTVWVjJyUDNJb1NmNAotLS0tLUVORCBDRVJUSUZJQ0FURS0tLS0tCg==

  server: https://192.168.40.180:6443

 name: kubernetes

contexts: null

current-context: ""

kind: Config

preferences: {}

users:

- name: dashboard-admin

 user:

  token: eyJhbGciOiJSUzI1NiIsImtpZCI6IlVNalJOeHBacEYxZ0tieTJKR2ZVY1hvTHZvcVVBU3FPaTU4TGhreDd5VzQifQ.eyJpc3MiOiJrdWJlcm5ldGVzL3NlcnZpY2VhY2NvdW50Iiwia3ViZXJuZXRlcy5pby9zZXJ2aWNlYWNjb3VudC9uYW1lc3BhY2UiOiJrdWJlcm5ldGVzLWRhc2hib2FyZCIsImt1YmVybmV0ZXMuaW8vc2VydmljZWFjY291bnQvc2VjcmV0Lm5hbWUiOiJrdWJlcm5ldGVzLWRhc2hib2FyZC10b2tlbi1wcGM4YyIsImt1YmVybmV0ZXMuaW8vc2VydmljZWFjY291bnQvc2VydmljZS1hY2NvdW50Lm5hbWUiOiJrdWJlcm5ldGVzLWRhc2hib2FyZCIsImt1YmVybmV0ZXMuaW8vc2VydmljZWFjY291bnQvc2VydmljZS1hY2NvdW50LnVpZCI6ImU0NTZlNTgzLWE1MDItNDViNy04YTEwLTE0MThhZmJiZmRlZCIsInN1YiI6InN5c3RlbTpzZXJ2aWNlYWNjb3VudDprdWJlcm5ldGVzLWRhc2hib2FyZDprdWJlcm5ldGVzLWRhc2hib2FyZCJ9.CKSwwXQfwF-HA_giAC1jzTmEnMjA73FtNpMjhI_coLOakc-MBX8f74K4k-TGMfBVFwqcvzBnCBOQJ2OJCgZX-YI109d4PDE9rGO3SK0OGIaLVDafB_9aoMeuMyrsut0PwgZoynUwA_DrwhOFkneNsA93rCIQck1WWOvxbrUtttmhvMDzYSZu6-TcGx6pxEiHwOEQ5dx92Tv6nSIBS_tjHU-CElQMhcHcHsD6AF-WdhSF2QtnvcCGasCcBPXzKF8cqd_QdvZvMgcUuwhUBVRRCEc_MCHDqGQBXZ6EzRE0PoXW_S6Rd1zvzT5SVFrgL3xGikPHTxWP2DurNZuCHmFnMw
```

**3、创建context**

```
[root@xianchaomaster1 pki]# kubectl config set-context dashboard-admin@kubernetes --cluster=kubernetes --user=dashboard-admin --kubeconfig=/root/dashboard-admin.conf

 

[root@xianchaomaster1 pki]# cat /root/dashboard-admin.conf 

apiVersion: v1

clusters:

- cluster:

  certificate-authority-data: LS0tLS1CRUdJTiBDRVJUSUZJQ0FURS0tLS0tCk1JSUM1ekNDQWMrZ0F3SUJBZ0lCQURBTkJna3Foa2lHOXcwQkFRc0ZBREFWTVJNd0VRWURWUVFERXdwcmRXSmwKY201bGRHVnpNQjRYRFRJeE1EVXdNVEEzTURRd01sb1hEVE14TURReU9UQTNNRFF3TWxvd0ZURVRNQkVHQTFVRQpBeE1LYTNWaVpYSnVaWFJsY3pDQ0FTSXdEUVlKS29aSWh2Y05BUUVCQlFBRGdnRVBBRENDQVFvQ2dnRUJBTXRUCktUYUF0RENySXB3VUtPUnhlbTRUYk9WS0JTM3NZaHBOTjE0TjAyUzI4Tnh5Y2oyUjVhVlF2cTF6aGJZYmVnZloKNm81UHAzMk4ydGEzQW5WMmpmT3h2OTFPR2FYL3NIT2hOZG5GQVpiZXdmZmJWVFhrbURMSnBIRmtYeUVubnltbQpRaDY2Rkl6S1IxSWZaLzFLaXRjM1dSQlpZSjlMRU53UjBWaFcyeUNRbmdwMEhtQ1hBcllZYTFFNzdnOHI5Vk5ZCjdrRm9ma2dla1BZWEhFeGxVS3dQVVcxSDd1ejRkVXlGZEorR1VjTE9WOXJhdWszaWMwazg4dG9ZdlNIelhjR3AKR0ZNS2FrMkhLYTF5RUkveGZqUFJPMDlRRmdvSTdTTFh4RG0yNHNuNUhFWHhzaTgrNFR2a2ZRVHRnZGNtRDNVRApWTzJrNHVFV1I0V1Axb0lYQnlzQ0F3RUFBYU5DTUVBd0RnWURWUjBQQVFIL0JBUURBZ0trTUE4R0ExVWRFd0VCCi93UUZNQU1CQWY4d0hRWURWUjBPQkJZRUZKTUJTREpZdXpIR1JmVWZQMWxBM0dpbGk5ZXlNQTBHQ1NxR1NJYjMKRFFFQkN3VUFBNElCQVFBK2REQnZpTk0rcXFnaFJtZUxNQUw2QUdBM1ZZWlByUkZaajI2RUI0R2FvMU01b1JtTgpLcmZQKzhQNzJtRmkycDRCR2xWTUN2TjhUSy8xZW1DWVc0MkZYS0YzOE5zVUhqVFVmZUlxaHJBSW9WWWVvRDlsCjFOeU42QkVkdWJSWHhjejByV3pMVkZMYUROMzhCcERzUGJMSk5qOGZmWUd0SzJnQlBNRDdRWWxabHpldnNRYzkKclczVGFHUGk2OVNJc0RGbGZvbnV1aEFqTzNqZVJ6VURzQmIxblIzRGpRQUFBMnRITDJOQVFXaW8zWlNBd3Q4Rwo4UFRYL20waWdaWjNiaHA3UjV5cEFNbWR3ODVEak1BMkhxQ0hFRjlvc1MxUnowQWdaK2VXWkNBZUtmclhKWW1kCkZuUjBYNU9TSlUybjhCRkVUbE8wa3N4QTVWVjJyUDNJb1NmNAotLS0tLUVORCBDRVJUSUZJQ0FURS0tLS0tCg==

  server: https://192.168.40.180:6443

 name: kubernetes

contexts:

- context:

  cluster: kubernetes

  user: dashboard-admin

 name: dashboard-admin@kubernetes

current-context: ""

kind: Config

preferences: {}

users:

- name: dashboard-admin

 user:

  token: eyJhbGciOiJSUzI1NiIsImtpZCI6IlVNalJOeHBacEYxZ0tieTJKR2ZVY1hvTHZvcVVBU3FPaTU4TGhreDd5VzQifQ.eyJpc3MiOiJrdWJlcm5ldGVzL3NlcnZpY2VhY2NvdW50Iiwia3ViZXJuZXRlcy5pby9zZXJ2aWNlYWNjb3VudC9uYW1lc3BhY2UiOiJrdWJlcm5ldGVzLWRhc2hib2FyZCIsImt1YmVybmV0ZXMuaW8vc2VydmljZWFjY291bnQvc2VjcmV0Lm5hbWUiOiJrdWJlcm5ldGVzLWRhc2hib2FyZC10b2tlbi1wcGM4YyIsImt1YmVybmV0ZXMuaW8vc2VydmljZWFjY291bnQvc2VydmljZS1hY2NvdW50Lm5hbWUiOiJrdWJlcm5ldGVzLWRhc2hib2FyZCIsImt1YmVybmV0ZXMuaW8vc2VydmljZWFjY291bnQvc2VydmljZS1hY2NvdW50LnVpZCI6ImU0NTZlNTgzLWE1MDItNDViNy04YTEwLTE0MThhZmJiZmRlZCIsInN1YiI6InN5c3RlbTpzZXJ2aWNlYWNjb3VudDprdWJlcm5ldGVzLWRhc2hib2FyZDprdWJlcm5ldGVzLWRhc2hib2FyZCJ9.CKSwwXQfwF-HA_giAC1jzTmEnMjA73FtNpMjhI_coLOakc-MBX8f74K4k-TGMfBVFwqcvzBnCBOQJ2OJCgZX-YI109d4PDE9rGO3SK0OGIaLVDafB_9aoMeuMyrsut0PwgZoynUwA_DrwhOFkneNsA93rCIQck1WWOvxbrUtttmhvMDzYSZu6-TcGx6pxEiHwOEQ5dx92Tv6nSIBS_tjHU-CElQMhcHcHsD6AF-WdhSF2QtnvcCGasCcBPXzKF8cqd_QdvZvMgcUuwhUBVRRCEc_MCHDqGQBXZ6EzRE0PoXW_S6Rd1zvzT5SVFrgL3xGikPHTxWP2DurNZuCHmFnMw
```

 

4、切换context的current-context是dashboard-admin@kubernetes

```
[root@xianchaomaster1 pki]# kubectl config use-context dashboard-admin@kubernetes --kubeconfig=/root/dashboard-admin.conf

 

[root@xianchaomaster1 pki]# cat /root/dashboard-admin.conf 

apiVersion: v1

clusters:

- cluster:

  certificate-authority-data: LS0tLS1CRUdJTiBDRVJUSUZJQ0FURS0tLS0tCk1JSUM1ekNDQWMrZ0F3SUJBZ0lCQURBTkJna3Foa2lHOXcwQkFRc0ZBREFWTVJNd0VRWURWUVFERXdwcmRXSmwKY201bGRHVnpNQjRYRFRJeE1EVXdNVEEzTURRd01sb1hEVE14TURReU9UQTNNRFF3TWxvd0ZURVRNQkVHQTFVRQpBeE1LYTNWaVpYSnVaWFJsY3pDQ0FTSXdEUVlKS29aSWh2Y05BUUVCQlFBRGdnRVBBRENDQVFvQ2dnRUJBTXRUCktUYUF0RENySXB3VUtPUnhlbTRUYk9WS0JTM3NZaHBOTjE0TjAyUzI4Tnh5Y2oyUjVhVlF2cTF6aGJZYmVnZloKNm81UHAzMk4ydGEzQW5WMmpmT3h2OTFPR2FYL3NIT2hOZG5GQVpiZXdmZmJWVFhrbURMSnBIRmtYeUVubnltbQpRaDY2Rkl6S1IxSWZaLzFLaXRjM1dSQlpZSjlMRU53UjBWaFcyeUNRbmdwMEhtQ1hBcllZYTFFNzdnOHI5Vk5ZCjdrRm9ma2dla1BZWEhFeGxVS3dQVVcxSDd1ejRkVXlGZEorR1VjTE9WOXJhdWszaWMwazg4dG9ZdlNIelhjR3AKR0ZNS2FrMkhLYTF5RUkveGZqUFJPMDlRRmdvSTdTTFh4RG0yNHNuNUhFWHhzaTgrNFR2a2ZRVHRnZGNtRDNVRApWTzJrNHVFV1I0V1Axb0lYQnlzQ0F3RUFBYU5DTUVBd0RnWURWUjBQQVFIL0JBUURBZ0trTUE4R0ExVWRFd0VCCi93UUZNQU1CQWY4d0hRWURWUjBPQkJZRUZKTUJTREpZdXpIR1JmVWZQMWxBM0dpbGk5ZXlNQTBHQ1NxR1NJYjMKRFFFQkN3VUFBNElCQVFBK2REQnZpTk0rcXFnaFJtZUxNQUw2QUdBM1ZZWlByUkZaajI2RUI0R2FvMU01b1JtTgpLcmZQKzhQNzJtRmkycDRCR2xWTUN2TjhUSy8xZW1DWVc0MkZYS0YzOE5zVUhqVFVmZUlxaHJBSW9WWWVvRDlsCjFOeU42QkVkdWJSWHhjejByV3pMVkZMYUROMzhCcERzUGJMSk5qOGZmWUd0SzJnQlBNRDdRWWxabHpldnNRYzkKclczVGFHUGk2OVNJc0RGbGZvbnV1aEFqTzNqZVJ6VURzQmIxblIzRGpRQUFBMnRITDJOQVFXaW8zWlNBd3Q4Rwo4UFRYL20waWdaWjNiaHA3UjV5cEFNbWR3ODVEak1BMkhxQ0hFRjlvc1MxUnowQWdaK2VXWkNBZUtmclhKWW1kCkZuUjBYNU9TSlUybjhCRkVUbE8wa3N4QTVWVjJyUDNJb1NmNAotLS0tLUVORCBDRVJUSUZJQ0FURS0tLS0tCg==

  server: https://192.168.40.180:6443

 name: kubernetes

contexts:

- context:

  cluster: kubernetes

  user: dashboard-admin

 name: dashboard-admin@kubernetes

current-context: dashboard-admin@kubernetes

kind: Config

preferences: {}

users:

- name: dashboard-admin

 user:

  token: eyJhbGciOiJSUzI1NiIsImtpZCI6IlVNalJOeHBacEYxZ0tieTJKR2ZVY1hvTHZvcVVBU3FPaTU4TGhreDd5VzQifQ.eyJpc3MiOiJrdWJlcm5ldGVzL3NlcnZpY2VhY2NvdW50Iiwia3ViZXJuZXRlcy5pby9zZXJ2aWNlYWNjb3VudC9uYW1lc3BhY2UiOiJrdWJlcm5ldGVzLWRhc2hib2FyZCIsImt1YmVybmV0ZXMuaW8vc2VydmljZWFjY291bnQvc2VjcmV0Lm5hbWUiOiJrdWJlcm5ldGVzLWRhc2hib2FyZC10b2tlbi1wcGM4YyIsImt1YmVybmV0ZXMuaW8vc2VydmljZWFjY291bnQvc2VydmljZS1hY2NvdW50Lm5hbWUiOiJrdWJlcm5ldGVzLWRhc2hib2FyZCIsImt1YmVybmV0ZXMuaW8vc2VydmljZWFjY291bnQvc2VydmljZS1hY2NvdW50LnVpZCI6ImU0NTZlNTgzLWE1MDItNDViNy04YTEwLTE0MThhZmJiZmRlZCIsInN1YiI6InN5c3RlbTpzZXJ2aWNlYWNjb3VudDprdWJlcm5ldGVzLWRhc2hib2FyZDprdWJlcm5ldGVzLWRhc2hib2FyZCJ9.CKSwwXQfwF-HA_giAC1jzTmEnMjA73FtNpMjhI_coLOakc-MBX8f74K4k-TGMfBVFwqcvzBnCBOQJ2OJCgZX-YI109d4PDE9rGO3SK0OGIaLVDafB_9aoMeuMyrsut0PwgZoynUwA_DrwhOFkneNsA93rCIQck1WWOvxbrUtttmhvMDzYSZu6-TcGx6pxEiHwOEQ5dx92Tv6nSIBS_tjHU-CElQMhcHcHsD6AF-WdhSF2QtnvcCGasCcBPXzKF8cqd_QdvZvMgcUuwhUBVRRCEc_MCHDqGQBXZ6EzRE0PoXW_S6Rd1zvzT5SVFrgL3xGikPHTxWP2DurNZuCHmFnMw 
```

**5、把刚才的kubeconfig文件dashboard-admin.conf复制到桌面**

浏览器访问时使用kubeconfig认证，把刚才的dashboard-admin.conf导入到web界面，那么就可以登陆了

![image-20230227171742365](https://raw.githubusercontent.com/tiansy9981/images/master/image-20230227171742365.png?token=AEL2C66WR2UJEORENJBL2UDFLQWQC) 

 

把dashboard-admin.conf文件导入

![image-20230227171808213.png](https://raw.githubusercontent.com/tiansy9981/images/master/20231121121038.png?token=AEL2C63XEG4N2ARFASVB6STFLQW7W)

![image-20230227171818340](https://raw.githubusercontent.com/tiansy9981/images/master/image-20230227171818340.png?token=AEL2C673DO6ZCZO55EWYDCLFLQWRI) 

 

 

#### 12.4 通过kubernetes-dashboard创建容器



把nginx.tar.gz镜像压缩包上传到xianchaonode1和xianchaonode2上，手动解压：

```
docker load -i nginx.tar.gz 
```

打开kubernetes的dashboard界面（https://192.168.40.180:32728/），点开右上角红色箭头标注的 “+”，如下图所示：

 ![image-20230227171831105](https://raw.githubusercontent.com/tiansy9981/images/master/image-20230227171831105.png?token=AEL2C6Z6J5TVPGR7JVJRWNTFLQXAW)

  

点击“+”出现如下界面：

![image-20230227171921654](https://raw.githubusercontent.com/tiansy9981/images/master/image-20230227171921654.png?token=AEL2C66W4OSMMF7MT36ZGDTFLQXBY) 

选择Create from form

 ![image-20230227171930413](https://raw.githubusercontent.com/tiansy9981/images/master/image-20230227171930413.png?token=AEL2C6YFTSSKV76XGHKSTA3FLQXBG)

 

上面箭头标注的地方填写之后点击Deploy即可完成Pod的创建，如下：

 ![image-20230227171940197](https://raw.githubusercontent.com/tiansy9981/images/master/image-20230227171940197.png?token=AEL2C635UMRABW42VPTM6J3FLQXCM)

 

在dashboard的左侧选择Services

 ![image-20230227171952140.png](https://raw.githubusercontent.com/tiansy9981/images/master/20231121121213.png?token=AEL2C65ZUFIKAKHXXSXIK53FLQXFW)

上图可看到刚才创建的nginx的service在宿主机映射的端口是31135，在浏览器访问：

192.168.40.180:31135


看到如下界面，说明nginx部署成功了：

 ![image-20230227171959832](https://raw.githubusercontent.com/tiansy9981/images/master/image-20230227171959832.png?token=AEL2C62F5X7RBNX5U64PRKTFLQXGU)

 

注：

应用名称：nginx

容器镜像：nginx

pod数量：2

service： external 外部网络

port：8-

targetport：80



注：表单中创建pod时没有创建nodeport的选项，会自动创建在30000+以上的端口。

关于port、targetport、nodeport的说明：

nodeport是集群外流量访问集群内服务的端口，比如客户访问nginx，apache，

port是集群内的pod互相通信用的端口类型，比如nginx访问mysql，而mysql是不需要让客户访问到的，port是service的的端口

targetport目标端口，也就是最终端口，也就是pod的端口。