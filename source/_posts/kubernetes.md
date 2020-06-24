---
title: kubernetes记录
date: 2020-06-24 14:38:12
categories: kubernetes
tags: kubernetes
---

## 安装minikube
Minikube是一个快速搭建单节点Kubenetes集群的工具,方便本地单机测试k8s
```sh
# 安装
choco install minikube
# minikube启动一个集群
minikube start --vm-driver=virtualbox --image-mirror-country cn --iso-url=https://kubernetes.oss-cn-hangzhou.aliyuncs.com/minikube/iso/minikube-v1.7.0.iso --image-repository=registry.aliyuncs.com/google_containers --memory 3072
# 如果出现错误，需要执行 minikube delete 删除缓存后再重新执行
# 可以看到，minikube start主要做了这些事：
# 创建了名为minikube的虚拟机，并在虚拟机中安装了Docker容器运行时。（实际就是Docker-machine）
# 下载了Kubeadm与Kubelet工具
# 通过Kubeadm部署Kubernetes集群
# 进行各组件间访问授权、健康检查等工作
# 在用户操作系统安装并配置kubectl，并将运行的集群变量设置为了minkube
# 这里说下，--registry-mirror 和 --image-repository请加上，因为minikube 需要下载一些工具，拉取docker镜像也比较大，不加的话，除非你的梯子很6，否则就慢慢等个几天吧。--memory 需要加上，之前默认2048M内存是报了内存不足，中断了的。
# 并且c盘一定要留够空间，下载的镜像有点多，到时候你进虚拟机看就知道了
```


## 安装kubectl
```sh
# 安装命令
choco install kubernetes-cli
# 查看安装位置
where kubectl
```
> 安装完成之后choco会默认帮你配置好环境变量我们剩下要做的就是配置kubectl 的kubeconfig文件

## 配置




## 参考
* [kubernetes中文社区](https://www.kubernetes.org.cn/k8s)
* [kubernetes中文文档](http://docs.kubernetes.org.cn/)
