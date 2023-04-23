# 在kubernetes集群上部署MasaStack服务

## 按照以下步骤在kubernetes上部署MasaStack.

设置kubernetes时，你需要使用到Helm

# 先决条件

* kubernetes集群

* 安装nginx-ingress

* 安装kubectl

* 安装Helm

* 安装Dapr

# 使用Helm安装

您可以使用Helm3 chart在kubernetes上部署MasaStack

## 添加并安装MasaStack Helm Chart

1. 确保您的机器上安装Helm3

2. 添加Helm仓库并更新

```shell
// 添加helm仓库
helm  repo add masastack https://masastack.github.io/helm/
// 或者添加私有仓库
helm repo add masastack http://helm.custom-domain.com/helm/masastack/ \
    --username=xxx --password=xxx
helm repo update masastack 

# see which chart versions are available
helm search repo masastack --devel --versions
```

3. 在集群的命名空间中安装MasaStack图标`masasatck`.

```shell
helm upgrade --install masastack masastack/masastack  --namespace masastack --create-namespace 
```

*程序启动时间和集群配置相关预计5-10分钟，镜像获取时间和本地带宽有关预计10-15分钟，总耗时15-25分钟就能完成安装*

## 验证安装

安装完成后

> kubectl get pods  --namespace   masastack 
