# 在kubernetes集群上部署MasaStack服务

## 按照以下步骤在kubernetes上部署MasaStack.

设置kubernetes时，你需要使用到Helm

# 先决条件

* Docker

* kubernetes集群

* 安装nginx-ingress

* 安装kubectl

* 安装Helm

* 安装Dapr

*先决条件部署参考地址*

* [Installation Guide - NGINX Ingress Controller](https://kubernetes.github.io/ingress-nginx/deploy/)

* [Installing Helm](https://helm.sh/zh/docs/intro/install/)

* [Install Tools  Kubernetes](https://kubernetes.io/docs/tasks/tools/)

* [Installing Kubernetes with deployment tools  Kubernetes](https://kubernetes.io/docs/setup/production-environment/tools/)

* [Deploy Dapr on a Kubernetes cluster Dapr Docs](https://docs.dapr.io/operations/hosting/kubernetes/kubernetes-deploy/)

* [Install Docker Engine on CentOS  Docker Documentation](https://docs.docker.com/engine/install/centos/)

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

3. 在集群的命名空间`masasatck`中安装MasaStack Charts .

```shell
helm upgrade --install masastack masastack/masastack --version 1.0.0 --namespace masastack --create-namespace 
```

*程序启动时间和集群配置相关预计5-10分钟，镜像获取时间和本地带宽有关预计10-15分钟，总耗时15-25分钟就能完成安装*

## 验证安装

安装完成后

> kubectl get pods  --namespace   masastack 

## 在kubernetes上卸载MasaStack

> helm uninstall masastack --namespace  masastack 

### 更多信息

#### 证书使用

* 自有通用证书

* 使用自签证书

`参考链接`：[生成临时的tls证书提供给ingress使用 ](https://masastack.github.io/helm/README_TLS)

> kubectl create secret tls <tls_name> --cert=<cert_file_path> --key=<key_file_path>  -n masastack 
> 
> helm upgrade --install masastack masastack/masastack --namespace  masastack  --create-namespace  --set global.secretName <tls_name> --set global.domain <domain_name>

*注1: <domain_name>为自签证书中的<Common Name>*

*注2:<tls_name>为自定义的secret名称k8s集群内部调用需要指定*

*注3:<cert_file_path>,<key_file_path>主要是自签或者自有安全的通用证书所在的位置，通过手动的方式来添加到部署集群里面来*

#### 常用变量

| 变量名                                                                   | 备注                                                        |
| --------------------------------------------------------------------- | --------------------------------------------------------- |
| global.sqlserver.{ip,id,port,password}                                | 使用外部数据库的时候配置，ip地址，账号，端口和密码                                |
| global.redis.{ip,db,port,password}                                    | 使用外部redis的配置                                              |
| global.elastic.{ip,port}                                              | 使用外部elasticsearch的配置                                      |
| global.prometheus{ip,port}                                            | 使用外部prometheus的配置                                         |
| global.suffix_identity                                                | <env>配置环境变量，针对本地多环境来使用                                    |
| global.volumeclaims.{enabled,storageSize,storageClassName}            | 分别是启动StorageClass存储，指定存储空间大小，指定相应的StorageClass，若无指定使用默认sc |
| middleware-{redis,prometheus,sqlserver,otel,elastic}.service.type     | ClusterIP,NodePort，默认为ClusterIP，主要为服务提供外部方位时修改            |
| middleware-{redis,prometheus,sqlserver,otel,elastic}.service.nodePort | 例如，32200 ；结合type使用，指定需要的端口                                |

其他更多的变量参考values.yaml文件
