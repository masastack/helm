# 部署demo环境

## 1. 创建storage class
>  https://kubernetes.io/docs/concepts/storage/storage-classes/
## 2. 部署dapr  (masastack 的服务不能和dapr在同一个namespace 不然daprd会有注入异常的情况出现)
>  kubectl create ns dapr
>  https://docs.dapr.io/operations/hosting/kubernetes/kubernetes-deploy/
## 3. 创建Masastack 需要的namespace
> kubectl create masastack
## 4. 创建tls证书 (如果有默认赠书可以不创建，)
>  参考具体内容[tls_create](./README_TLS.md)
## 5. 私有镜像仓库设置
### 1. 创建相关的dockerconfigjson
> kubectl create secret docker-registry masastack  --docker-server=DOCKER_REGISTRY_SERVER --docker-username=DOCKER_USER --docker-password=DOCKER_PASSWORD  --docker-email=DOCKER_EMAIL
### 2. 配置引用，需要在values.yaml进行配置
>  global.imagePullSecrets: ["masastack","masastack01"] 
## 6. 


