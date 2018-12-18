---
date: 2018-12-10 16:56
status: public
title: Kubernetes
categories: Linux
tags: K8s
---

## Kubernetes

下载kubernetes client或kubernetes server
url：` https://github.com/kubernetes/kubernetes/blob/master/CHANGELOG-1.11.md#server-binaries `


用途： 容器的编排

K8s集群架构
![k8s](http://wx4.sinaimg.cn/large/007fPWmPly1fy41o16pa1j30jm0br0t9.jpg)


#### Master Node
Master 提供集群的管理控制中心，调度，控制集群的资源，包含：
- API Server： 任何的资源请求/调用操作都是通过 Kube-apiserver 提供的接口进行
- Controller： 运行管理控制器，它们是集群中处理常规任务的后台线程。
- Schedule：调度Node的Pod，为Pod选择一个Node。优先级队列的选择

#### Node： 运行容器，运行服务的节点
Kubelet：是主要的节点代理，它会监视已分配给节点的pod，具体功能：
- 安装Pod所需的volume。
- 下载Pod的Secrets。
- Pod中运行的 docker（或experimentally，rkt）容器。
- 定期执行容器健康检查。
等等...


#### 部署Deployment和 副本集Replicaset

![k8s1](http://wx4.sinaimg.cn/large/007fPWmPly1fy41o1fdfhj30bc0ai3ys.jpg)

- Deployment部署应用：让应用程序在集群上运行：

1. 包含Replica Set
2. 包含版本信息用于升级/回滚

- Replicaset副本集，创建Pod的多个副本集，可扩容/缩容，实现负载均衡。


#### Service
**服务Service**：
- 提供外界访问的接口，关联一组Pod， 可以用kubectl get services 命令查看应用被映射到节点的哪个端口，eg 8080:30253
- Service是Pods的逻辑抽象，体现对一个虚拟IP和端口，可供外部访问

**Service的几种类型**
- ClusterIP：会创建k8s cluster内可以访问的cluster ip，集群内调用者可通过该IP访问该服务
- NodePort：可以通过该cluster的任意一个node的外部IP来访问，NodePort的端口范围为30000-32767
- LoadBalancer：会调用iaas的服务创建load balancer的VIP，集群外调用者可通过此外IP访问


#### Namespace
在一个名字空间内，资源的名字必须保证unique，但是不同名字空间内，可以相同
- 创建namespace`kubectl create namespace [名字] `
- 删除namespce `kubectl delete namespaces [名字]` 

> Tips: 删除namespcace后，改namespace对应的所有集群资源都删除了。

#### Label 
一对 key/value， 被关联到对象上例如pod（一个对象可以有多个label）
service 是通过label关联Deployment的 （在yaml file里）
![k8s2](http://wx4.sinaimg.cn/large/007fPWmPly1fy41o1rh1pj30nk0bkdiw.jpg) 


#### Kubectl常用命令
```sh
$ kubectl version
$ kubectl help
$ kubectl cluster-info
$ kubectl create namespace [名字]
$ kubectl get nodes --namespace=[名字]
$ kubectl get pods
$ kubectl get deployments
$ kubectl get services
$ kubectl delete namespaces [名字]
$ kubectl describe xxx #查看pod或node细节
$ kubectl logs xxx #查看日志文件
```

#### kubectl demo
```sh
$ kubectl create namespace [名字]  #创建namespace
$ kubectl run kubernetes-bootcamp --image=hub.baidubce.com/xxx/mynode:1.0.0 --port=8080 --namespace=[名字]  #配置pod并运行
$ kubectl get pods --namespace=xxx  #查看运行的pod
$ kubectl describe pods/[pod name] --namespace=xxx #查看pod详细配置
$ $kubectl expose deployment/kubernetes-bootcamp --type="NodePort" --port 8080 --namespace=[名字]  #service的配置
$curl $VM_IP:$NODE_PORT  #运行pod中的程序
```

> k8s 扩容/缩容， 版本更新， 小流量， A/B测试 demo 参考ppt

#### 使用yaml file
运行yaml文件： `kubectl create -f xxx.yaml`

namespace yaml文件：
```yaml
apiVersion: v1
kind: Namespace
metadata:
  name: linxubin
```

Deployment yaml文件:
```yaml
apiVersion: extensions/v1beta1
kind: Deployment
metadata:
  name: kubernetes-bootcamp
  namespace: linxubin
spec:
  replicas: 3
  template:
    metadata:
      labels:
        app: linode
        track: stable
        version: 1.0.0
    spec:
      containers:
        - name: linxubin
          image: "hub.baidubce.com/bootcamp_6/linode:1.0.0"
          ports:
            - name: http
              containerPort: 8080
```

Service的yaml文件：
```yaml
apiVersion: v1
kind: Service
metadata:
  name: linxubin
  labels:
    app: linode
spec:
  ports:
  - port: 8080
    targetPort: 8080
  type: NodePort
  selector:
    app: linode   #对应Deployment的label
```



