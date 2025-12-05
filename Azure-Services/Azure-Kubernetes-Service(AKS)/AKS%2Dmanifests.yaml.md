[[_TOC_]]

Framework: .net6 
專案類型: .NET Core MVC
![image.png](/.attachments/image-b2ef530c-e9e2-40e4-8eee-dfe61494fefe.png)


Deployment、Service 命名注意事項:
必須小寫字母數字字元 或 - 組成
正確: walsin-dataplatform-dev
錯誤: walsin.dataplatform-dev


# WebAppAKS
前端APP
## service.yml
```
apiVersion: v1
kind: Service
metadata:
    name: webappaks-portal
spec:
    type: ClusterIP
    ports:
    - port: 80 
    selector:
        app: webappaks-portal
```


## deployment.yml

```
apiVersion : apps/v1
kind: Deployment
metadata:
  name: webappaks-portal
spec:
  replicas: 1
  selector:
    matchLabels:
      app: webappaks-portal
  template:
    metadata:
      labels:
        app: webappaks-portal
    spec:
      containers:
        - name: webappaks 
          image: walsinacrtest01.azurecr.io/webappaks
          ports:
          - containerPort: 80
```



<hr/>

# WebAppAKS.Backend
後端APP
## service.yml

```
apiVersion: v1
kind: Service
metadata:
    name: webappaks-backend
spec:
    type: ClusterIP
    ports:
    - port: 80 
    selector:
        app: webappaks-backend
```

## deployment.yml

```
apiVersion : apps/v1
kind: Deployment
metadata:
  name: webappaks-backend 
spec:
  replicas: 1
  selector:
    matchLabels:
      app: webappaks-backend
  template:
    metadata:
      labels:
        app: webappaks-backend 
    spec:
      containers:
        - name: webappaks-backend 
          image: walsinacrtest01.azurecr.io/webappaks-backend
          ports:
          - containerPort: 80
```





<hr/>

# NGINX Igress 設定
系統路由設定
官網: https://www.nginx.com/

## ingress-router.yaml

```
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: webappaks-ingress
  namespace: az-walsin-webappaks
  annotations:
    nginx.ingress.kubernetes.io/ssl-redirect: "false"
    nginx.ingress.kubernetes.io/use-regex: "true"
    nginx.ingress.kubernetes.io/rewrite-target: /
spec: 
  ingressClassName: nginx
  rules:
  - host: webpppaks-dev.southeastasia.cloudapp.azure.com
    http:
      paths:
      - path: /
        pathType: Prefix
        backend:
          service:
            name: webappaks-portal
            port:
              number: 80
      - path: /webappaks-portal(/|$)(.*)
        pathType: Prefix
        backend:
          service:
            name: webappaks-portal
            port:
              number: 80

---
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: webappaks-backend
  namespace: az-walsin-webappaks
  annotations:
    nginx.ingress.kubernetes.io/ssl-redirect: "false"
    nginx.ingress.kubernetes.io/rewrite-target: /$2
spec:
  ingressClassName: nginx
  rules:
  - host: webpppaks-dev.southeastasia.cloudapp.azure.com
    http:
      paths:
      - path: /webappaks-backend(/|$)(.*)
        pathType: Prefix
        backend:
          service:
            name: webappaks-backend
            port: 
              number: 80
      
```

---
# Deployment 進階設定 - 資源限制
使用 Kubernetes 時，限制和請求是重要的設定。 本文將重點放在兩個最重要的：CPU 和記憶體。

## 資源類型
CPU 以及 memory 為 資源類型 (resource type), 資源類型有其基本單位。 CPU 以 cores 為單位, 而 memory 以 bytes 為單位。

<IMG  src="https://sysdig.com/wp-content/uploads/Kubernetes-Limits-and-Request-04-1-1170x585.png"/>


# Pod 以及 Container 的資源要求以及資源限制
每個 Container 或 Pod 可被指定下列一個或多個條件：

spec.containers[].resources.limits.cpu
spec.containers[].resources.limits.memory
spec.containers[].resources.limits.hugepages-<size>
spec.containers[].resources.requests.cpu
spec.containers[].resources.requests.memory
spec.containers[].resources.requests.hugepages-<size>
儘管 requests 以及 limits 只可被指定再單獨的 Container 上, 但要計算出 Pod 的 request 以及 limits 也很方便。 Pod 對某項特定資源的 request/limit 就等於該 Pod 裡的所有容器的 request/limit 總和。

## CPU 解釋
CPU 資源的 limits 以及 requests 都以 cpu 為單位衡量。 1 cpu, 在 Kubernetes 中, 相當於以下單位:


```
1 AWS vCPU
1 GCP Core
1 Azure vCore
1 IBM vCPU
1 Hyperthread, Hyperthreading 的 Intel bare-metal 處理器
```

小數的資源請求是容許的。 spec.container[].resources.requests.cpu 如果設為 0.5, 那相當於要求 1 CPU 的一半。 0.1 相等於 100m, 100m 也可讀為 "100 millicpu"。 有些人說 "100 millicores", 都可理解為同樣的東西。 有小數點的請求, 像是 0.1, 會被 API 轉換到 100m, 而精度最小為 1m, 所以設定 100m 會是比較理想的

CPU 都是絕對數字, 不會是相對數字; 舉例來說, 0.1 在 1-core, 2-core, 或 48-core 的機器中代表的量都是一樣的。

## Memory 解釋
Memory 中的 limits 以及 requests 資源管理以 bytes 為單位。 記憶體可被表示為簡單的 integer, 或固定位數的 integer, 可使用以下後綴： E, P, T, G, M, K 。 你也可以使用: Ei, Pi, Ti, Gi, Mi, Ki, 舉例來說, 下面都代表大約相同的值：

`128974848, 129e6, 129M, 123Mi`

這邊有個範例, 下面的 Pod 含有兩個容器。 每個容器都有 resource requests, 0.25 cpu 以及 64MiB 記憶體。 每個容器都有 resource limits 0.5 cpu 以及 128 MiB 記憶體。 可以說, 這個 Pod 有 resource requests 0.5 cpu 以及 128 MiB 記憶體, resource limits 1 cpu 以及 256 MiB 記憶體

範例
```
apiVersion: v1
kind: Pod
metadata:
  name: frontend
spec:
  containers:
  - name: db
    image: mysql
    env:
    - name: MYSQL_ROOT_PASSWORD
      value: "password"
    resources:
      requests:
        memory: "64Mi"
        cpu: "250m"
      limits:
        memory: "128Mi"
        cpu: "500m"
  - name: wp
    image: wordpress
    resources:
      requests:
        memory: "64Mi"
        cpu: "250m"
      limits:
        memory: "128Mi"
        cpu: "500m"
```


參考:
https://medium.com/learn-or-die/kubernetes-resources-limit-%E8%B3%87%E6%BA%90%E9%99%90%E5%88%B6-803bac05c061

---

# 建立輸入控制器
若要建立輸入控制器，請使用 Helm 來安裝 ingress-nginx。 
需要在 Linux 節點上排程輸入控制器。 
Windows Server 節點不應執行輸入控制器。 
您可以使用 --set nodeSelector 參數來指定節點選取器，以告知 Kubernetes 排程器在 Linux 式節點上執行 NGINX 輸入控制器。

為了新增備援，您必須使用 --set controller.replicaCount 參數部署兩個 NGINX 輸入控制器複本。 
為充分享有執行輸入控制器複本的好處，請確定 AKS 叢集中有多個節點。

下列範例會對名為 ingress-basic 的輸入資源建立 Kubernetes 命名空間，並打算在該命名空間內運作。 
視需要為您自己的環境指定命名空間。 
如果您的 AKS 叢集未啟用 Kubernetes 角色型存取控制，請將 --set rbac.create=false 新增至 Helm 命令。

**注意**
如果您想要針對叢集中容器的要求啟用用戶端來源 IP 保留，
請將 --set controller.service.externalTrafficPolicy=Local 新增至 Helm install 命令。 
用戶端來源 IP 儲存在 X-Forwarded-For 下的要求標頭中。 
使用已啟用用戶端來源 IP 保留的輸入控制器時，TLS 傳遞將不會運作。


```
INGRESS_NAME=ingress-nginx-webappaks-internal
NAMESPACE=az-webappaks-internal
INGRESS_CLASS=nginx-webappaks-internal

helm install $INGRESS_NAME ingress-nginx/ingress-nginx \
--create-namespace \
--namespace $NAMESPACE \
--set controller.replicaCount=1 \
--set controller.admissionWebhooks.patch.nodeSelector."kubernetes\.io/os"=linux \
--set controller.ingressClass=$INGRESS_CLASS \
--set controller.ingressClassResource.name=$INGRESS_CLASS \
--set controller.ingressClassResource.controllerValue="k8s.io/$INGRESS_CLASS" \
--set controller.ingressClassResource.enabled=true \
--set controller.service.annotations."service\.beta\.kubernetes\.io/azure-load-balancer-internal"=true \
--set controller.service.annotations."service\.beta\.kubernetes\.io/azure-load-balancer-health-probe-request-path"=/healthz
```

![image.png](/.attachments/image-4d4e5aaa-48fc-4c11-ada1-aa73f559fb05.png)

- Ingress 服務
![image.png](/.attachments/image-21245ad4-df54-44bc-9492-16836b22b2fc.png)

- Kubernetes internal 負載平衡器
![image.png](/.attachments/image-ffc9c859-2c5e-4bf0-89c0-e8b109b327c9.png)

- 工作負載
![image.png](/.attachments/image-724b0f48-ee9c-4b05-bf34-36a39231627a.png)

## 測試內部 IP 位址
1. 進入pod
`kubectl exec -it webappaks-internal-5c4cdd659-p9cgm -n az-webappaks-internal -- /bin/bash`

1. 套件更新

```
apt-get update && apt-get install -y curl
```

1. 服務測試
`curl http://webappaks-internal`



參考
https://learn.microsoft.com/zh-tw/azure/aks/ingress-basic?tabs=azure-cli#create-an-ingress-controller


