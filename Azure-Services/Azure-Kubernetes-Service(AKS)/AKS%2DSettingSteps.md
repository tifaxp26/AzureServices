[[_TOC_]]

# 參考文件
- https://learn.microsoft.com/zh-tw/troubleshoot/azure/azure-kubernetes/load-bal-ingress-c/create-unmanaged-ingress-controller?tabs=azure-cli
- https://learn.microsoft.com/zh-tw/azure/aks/ingress-basic?tabs=azure-cli
- https://learn.microsoft.com/zh-tw/azure/aks/kubernetes-helm
- https://kubernetes.io/docs/concepts/services-networking/ingress/
- https://helm.sh/docs/helm/helm_repo_remove/
- https://microsoft.github.io/PartsUnlimited/iac/200.2x-IaCDeployApptoAKS.html


<hr /> 

# 訂閱資訊
訂閱帳號: Sandbox CAF測試_1
資源群組: rg-walsin-development

## 使用 Azure Cloud Shell
--登入
```
az login --tenant 97876bed-bb9a-4617-802b-94c62e7837b5
az account set --subscription fd758007-06fa-47a6-af48-a500d2b9f23f
az aks get-credentials --resource-group rg-walsin-development --name WalsinAKSTest01
```


--建立 namespace
ex:
```
kubectl create namespace az-webappaks
kubectl create namespace az-webappaks-prod
```


# Use Helm to deploy an NGINX ingress controller
--建立 NGINX Ingress Controller
- 下載資源檔
```
helm repo add ingress-nginx https://kubernetes.github.io/ingress-nginx
helm repo update
```


- 建立命名空間

`kubectl create namespace az-webappaks`


- 建立 Ingress Controller
**ingress-nginx 會產生 nginxClass 名稱為 nginx，必須跟路由設定名稱一致!!**


- 範例1
```
helm install ingress-{名稱} ingress-nginx/ingress-nginx \
--create-namespace \
--namespace {名稱} \
--set controller.service.annotations."service\.beta\.kubernetes\.io/azure-load-balancer-health-probe-request-path"=/healthz \
--set controller.replicaCount=1 \
--set controller.nodeSelector."kubernetes\.io/os"=linux \
--set controller.ingressClass={ingressClass} \
--set controller.ingressClassResource.name={ingressClass} \
--set controller.ingressClassResource.controllerValue="k8s.io/{ingressClass}" \
--set controller.ingressClassResource.enabled=true
```

- 範例2

```
helm install ingress-nginx-dataplatform-dev ingress-nginx/ingress-nginx \
--create-namespace \
--namespace az-walsin-dataplatform-dev \
--set controller.service.annotations."service\.beta\.kubernetes\.io/azure-load-balancer-health-probe-request-path"=/healthz \
--set controller.replicaCount=1 \
--set controller.nodeSelector."kubernetes\.io/os"=linux \
--set controller.ingressClass=nginx-dataplatform-dev \
--set controller.ingressClassResource.name=nginx-dataplatform-dev \
--set controller.ingressClassResource.controllerValue="k8s.io/nginx-dataplatform-dev" \
--set controller.ingressClassResource.enabled=true
```

- 範例-舊版
```
helm install ingress-nginx ingress-nginx/ingress-nginx \
--namespace az-webappaks \
--create-namespace \
--set controller.replicaCount=1 \
--set controller.nodeSelector."kubernetes\.io/os"=linux	\
--set controller.service.annotations."service\.beta\.kubernetes\.io/azure-load-balancer-health-probe-request-path"=/healthz \
```



以下參數可忽略
--set controller.ingressClassResource.name=webappaks-nginx \
--set defaultBackend.nodeSelector."kubernetes\.io/os"=linux
--set controller.admissionWebhooks.patch.nodeSelector."kubernetes\.io/os"=linux \



使用內部 IP 位址建立輸入控制器
--set controller.service.loadBalancerIp={內部IP} \
--set controller.service.annotations."service\.beta\.kubernetes\.io/azure-load-balancer-internal"=true \
--set controller.service.annotations."service\.beta\.kubernetes\.io/azure-load-balance-resource-groupt"=MC_rg-walsin-development_WalsinAKSTest01_southeastasia	\

	
# 顯示所有 Ingress class
`kubectl get ingressclass --namespace az-webappaks`
	
# 檢查負載平衡器服務
使用 kubectl get services 檢查負載平衡器服務。
`kubectl get services --namespace az-webappaks -o wide -w nginx-ingress-ingress-nginx-controller`

如果您流覽至此階段的外部 IP 位址，您會看到顯示 404 頁面。 
這是因為您仍然需要設定外部 IP 的連線，這會在下一節中完成。

# 建立輸入路由

```
code ingress-router.yaml
kubectl apply -f ingress-router.yaml --namespace az-webappaks
```


# 取得路由清單

```
kubectl get services --namespace az-webappaks -w
kubectl describe ingress nginx-ingress-ingress-nginx-controller --namespace az-webappaks
```

	
	

# 清除資源
本文使用 Helm 來安裝輸入元件和範例應用程式。 
部署 Helm 圖表時會建立許多 Kubernetes 資源。 這些資源包含 Pod、部署和服務。 
若要清除這些資源，您可以刪除整個範例命名空間或個別資源。

## 刪除範例命名空間和所有資源
如果要刪除整個範例命名空間，請使用kubectl delete命令並指定命名空間的名稱。 
## 命名空間中的所有資源都會刪除。
`kubectl delete namespace ingress-basic`	
	
	
## 個別刪除資源
 或者，更精細的方法就是刪除所建立的個別資源。
 使用helm list命令，列出 Helm 版本。

`helm list --namespace az-webappaks`

## 使用 helm uninstall 命令，解除安裝這些版本。

```
helm uninstall nginx-ingress --namespace az-webappaks 
helm uninstall webappaks-prod-nginx-ingress --namespace az-webappaks
```

	
## 移除兩個範例應用程式。

```
kubectl delete -f aks-helloworld-one.yaml --namespace az-webappaks
kubectl delete -f aks-helloworld-two.yaml --namespace az-webappaks
```


## 移除將流量導向範例應用程式的輸入路由。
`kubectl delete -f ingress-router2.yaml --namespace az-webappaks`
	
## 使用 kubectl delete 命令刪除命名空間，並指定命名空間名稱。
`kubectl delete namespace az-webappaks`
	

## 移除 ingressclass

```
kubectl delete ingressclass webappaks-prod-nginx
kubectl delete ingressclass nginx
```


## 移除 ValidatingWebhookConfiguration

```
kubectl get ValidatingWebhookConfiguration -A
kubectl delete ValidatingWebhookConfiguration webappaks-prod-nginx-ingress-nginx-admission
```


<hr />


# 建立其他 NGINX Ingress Controller
注意事項:
- 定義 命名空間名稱: az-webappaks-prod
- 定義 ingress名稱: webappaks-prod-nginx-ingress， ingress會自動刪除


`kubectl create namespace az-webappaks-prod`


```
helm install webappaks-prod-nginx ingress-nginx/ingress-nginx \
--namespace az-webappaks-prod \
--create-namespace \
--set controller.ingressClassResource.name=webappaks-prod-nginx \
--set controller.replicaCount=1 \
--set controller.nodeSelector."kubernetes\.io/os"=linux	\
--set controller.service.annotations."service\.beta\.kubernetes\.io/azure-load-balancer-health-probe-request-path"=/healthz \
```






