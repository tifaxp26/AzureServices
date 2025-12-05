[[_TOC_]]


您會相應放大應用程式中的 Pod、嘗試 Pod 自動調整，
以及調整 Azure VM 節點的數目，以變更叢集裝載工作負載的容量。

1. 調整 Kubernetes 節點。
1. 手動調整執行應用程式的 Kubernetes Pod。
1. 設定執行應用程式前端的自動調整 Pod。


# 手動調整 Pod
1. 使用 命令檢視叢集中的 kubectl get Pod。
`kubectl get pods --namespace az-webappaks`

1. 使用 kubectl scale 命令，手動變更存放區前端 部署中的 Pod 數目。

```
kubectl scale --replicas=2 deployment.apps/webappaks-portal --namespace az-webappaks
or
kubectl autoscale deployment.apps webappaks-portal --min=1 --max=2 --cpu-percent=80 --namespace az-webappaks
```


![image.png](/.attachments/image-f8b4da44-f3cf-46a1-a004-37c99add39ac.png)

1. 確認已使用 kubectl get pods 命令建立其他 Pod。
`kubectl get pods --namespace az-webappaks`



# 自動調整 Pod
若要使用水準 Pod 自動調整程式，所有容器和 Pod 都必須已定義 CPU 要求和限制。 
在部署中 aks-store-quickstart ，前端 容器會要求 1000 萬 CPU 的限制為 1000m CPU。

1. 使用資訊清單檔案自動調整 Pod
建立資訊清單檔來定義自動調整程式列為和資源限制，
如下列壓縮範例資訊清單檔 webappaks-portal-hpa.yaml 所示：

webappaks-portal-hpa.yaml
```
apiVersion: autoscaling/v1
kind: HorizontalPodAutoscaler
metadata:
  name: webappaks-portal-hpa
spec:
  maxReplicas: 2  # define max replica count
  minReplicas: 1  # define min replica count
  scaleTargetRef:
    apiVersion: apps/v1
    kind: Deployment
    name: webappaks-portal
  targetCPUUtilizationPercentage: 50 # target CPU utilization
```

# 如何 查詢/ 套用 / 刪除

```
kubectl get hpa --namespace az-webappaks
kubectl apply -f webappaks-portal-hpa.yaml --namespace az-webappaks
kubectl delete -f webappaks-portal-hpa.yaml --namespace az-webappaks
kubectl delete hpa webappaks-portal --namespace az-webappaks
```
![image.png](/.attachments/image-50a24d9d-2439-4f33-99ae-3d8c1f0c58fa.png)

  
1. 使用 kubectl get hpa 命令檢查自動調整程式的狀態。
`kubectl get hpa`


![image.png](/.attachments/image-74513d87-01c8-4dbc-9622-531b5e59c69f.png)

幾分鐘後，Azure Store Front 應用程式的負載最少，
Pod 複本數目會減少到三個。 
您可以再次使用 kubectl get pods 來查看移除不需要的 Pod。




<hr/>

# concepts-scale
## 手動調整 Pod 或節點
您可以手動調整複本或 Pod，以及節點，以測試應用程式如何回應可用資源和狀態的變更。 
手動調整資源可讓您定義一組資源，以維護固定成本，例如節點數目。 
若要手動調整，您需要定義複本或節點計數。 然後 Kubernetes API 會根據該複本或節點計數排程建立其他 Pod 或清空節點。

縮小節點時，Kubernetes API 會呼叫繫結至叢集所用計算類型的相關 Azure 計算 API。 
例如，針對建置在 虛擬機器擴展集 上的叢集，選取要移除之節點的邏輯取決於虛擬機器擴展集 API。 
若要深入瞭解如何選取節點以在相應減少時移除，請參閱虛擬機器擴展集常見問題。


## 水平 Pod 自動調整程式
Kubernetes 使用水準 Pod 自動調整程式 (HPA) 來監視資源需求，並自動調整 Pod 數目。 
根據預設，HPA 每隔 15 秒檢查計量 API 是否有複本計數的任何必要變更，而 Metrics API 每隔 60 秒就會從 Kubelet 擷取資料。 
因此，HPA 每隔 60 秒更新一次。 需要進行變更時，複本的數目會據以增加或減少。 
HPA 適用于已部署 Kubernetes 1.8+ 計量伺服器的 AKS 叢集。

<IMG  src="https://learn.microsoft.com/zh-tw/azure/aks/media/concepts-scale/horizontal-pod-autoscaling.png"  alt="Kubernetes 水平 Pod 自動調整"/>

當您為指定的部署設定 HPA 時，您可以定義可執行檔複本數目下限和最大值。 
您也會根據任何調整決策定義要監視的計量，例如 CPU 使用量。


## 調整事件中的冷卻時間
由於 HPA 會每隔 60 秒有效更新一次，因此在進行另一次檢查之前，先前的縮放事件可能尚未順利完成。 
此行為可能會導致 HPA 變更先前調整事件前的複本數目，才能接收應用程式工作負載，以及據以調整的資源需求。

若要將競爭事件降到最低，則會設定延遲值。 
這個值會定義 HPA 必須在調整事件之後等候多久，才能觸發另一個縮放事件。 此
行為可讓新的複本計數生效，並讓計量 API 反映分散式工作負載。 
從 Kubernetes 1.12 起，相應增加事件沒有任何延遲，不過，相應縮小事件的預設延遲為5 分鐘。

## 叢集自動調整程式
為了回應變更的 Pod 需求，Kubernetes 叢集自動調整程式會根據節點集區中要求的計算資源來調整節點數目。 根據預設，叢集自動調整程式每隔 10 秒會檢查計量 API 伺服器，找出節點計數中任何必要的變更。 
如果叢集自動調整程式判斷需要變更，AKS 叢集中的節點數目會隨之增加或減少。 
叢集自動調整程式會使用已啟用 Kubernetes RBAC 功能且執行 Kubernetes 1.10.x 或更高版本的 AKS 叢集。

<IMG  src="https://learn.microsoft.com/zh-tw/azure/aks/media/concepts-scale/cluster-autoscaler.png"  alt="Kubernetes 叢集自動調整程式"/>

叢集自動調整程式通常會與 水準 Pod 自動調整程式一起使用。
結合時，水準 Pod 自動調整程式會根據應用程式需求增加或減少 Pod 數目，而叢集自動調整程式會調整節點數目以執行其他 Pod。

## 擴增事件
如果節點沒有足夠計算資源可執行所需的 Pod，則該 Pod 將無法完成排程的程序。 
除非節點集區內有額外的計算資源可以使用，否則無法啟動 Pod。

當叢集自動調整程式注意到因節點集區資源限制而無法排程 Pod 時，會增加節點集區內的節點數目，以提供額外的計算資源。 
當這些額外的節點已成功部署且可供在節點集區內使用時，就會將 Pod 排程在節點上執行。

如果您必須快速調整應用程式，某些 Pod 會維持在等候排程的狀態，直到由叢集自動調整程式部署的其他節點可以接受排程的 Pod 為止。 
對於具有高高載需求的應用程式，您可以使用虛擬節點和Azure 容器執行個體進行調整。

## 縮減事件
叢集自動調整程式也會針對最近未收到新排程要求的節點，監視 Pod 排程狀態。 
此案例表示節點集區具有比必要更多的計算資源，而且可以減少節點數目。 
根據預設，已排定 10 分鐘不再需要閾值的節點進行刪除。 
當這種情況發生時，Pod 會排程在節點集區內的其他節點上執行，且叢集自動調整程式會減少節點的數目。

因為叢集自動調整程式在減少節點的數目時，Pod 會排程在不同節點上，所以您的應用程式可能會遇到一些中斷情況。 
若要盡可能不中斷，請避免使用單一 Pod 執行個體的應用程