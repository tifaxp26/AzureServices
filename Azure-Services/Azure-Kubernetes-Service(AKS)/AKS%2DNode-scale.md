[[_TOC_]]

# 關於叢集自動調整器
若要調整以變更應用程式需求，例如工作日和晚上或週末之間，叢集通常需要自動調整的方式。 
AKS 叢集可以透過下列方式進行調整：

## 叢集自動調整程式 
會定期檢查因為資源限制而無法在節點上排程的 Pod。 叢集接著會自動增加節點數目。 如需詳細資訊，請參閱 相應增加如何運作？ 。
## 水準 Pod 自動調整程式 
會使用 Kubernetes 叢集中的計量伺服器來監視 Pod 的資源需求。 如果應用程式需要更多資源，Pod 數目會自動增加以符合需求。
## 垂直 Pod 自動調整程式 （預覽） 
會根據過去的使用量，自動設定每個工作負載容器的資源要求和限制，以確保 Pod 排程到具有所需 CPU 和記憶體資源的節點。


<IMG  src="https://learn.microsoft.com/zh-tw/azure/aks/media/autoscaler/cluster-autoscaler.png"  alt="Screenshot of how the cluster autoscaler and horizontal pod autoscaler often work together to support the required application demands."/>


![image.png](/.attachments/image-24ce5e01-873c-4bf3-a92f-0df0c57491ca.png)



# AKS Cluster
## Agentpool: 節點集區，即VMSS(virtual-machine-scale-sets)
![image.png](/.attachments/image-b57e6803-3bb3-45a2-b3bc-77f0fc5bda1c.png)

## node: 節點，對應VM，可設定VM SKU
![image.png](/.attachments/image-ba17a1b1-62ef-4349-88bb-d03922d2edba.png)



<hr />

水準 Pod 自動調整程式會視需要調整 Pod 複本數目，而叢集自動調整程式會視需要調整節點集區中的節點數目。 叢集自動調整程式會在一段時間後未使用的容量時減少節點數目。 叢集自動調整程式移除之節點上的任何 Pod 都會安全地排程在叢集中的其他地方。

雖然垂直 Pod 自動調整程式或水準 Pod 自動調整程式可用來自動調整工作負載中的 Kubernetes Pod 數目，但節點數目也必須能夠調整以符合 Pod 的計算需求。 叢集自動調整程式可解決需要相應增加和相應減少 Kubernetes 節點的問題。 為節點啟用叢集自動調整程式，以及 Pod 的垂直 Pod 自動調整程式或水準 Pod 自動調整程式是常見的做法。

叢集自動調整程式和水準 Pod 自動調整程式可以一起運作，而且通常會部署在叢集中。 結合時，水準 Pod 自動調整程式會執行符合應用程式需求所需的 Pod 數目，而叢集自動調整程式會執行支援排程 Pod 所需的節點數目。

啟用叢集自動調整程式時，當節點集區大小低於最小或大於套用調整規則的最大值時。 接下來，自動調整程式會等候生效，直到節點集區中需要新的節點，或直到節點可以從目前的節點集區安全地刪除為止。


如果 Pod 無法移動，叢集自動調整器可能無法縮小，如下列情況所示：

直接建立的 Pod 不受控制器物件支援，例如部署或複本集。
Pod 中斷預算 (PDB) 限制太多，而且不允許低於特定閾值的 Pod 數目。
如果排程在不同節點上，則 Pod 會使用節點選取器或無法接受的反親和性。

---

# 針對 節點Node scale
參考以下設定:
1. 選擇節點集區
![image.png](/.attachments/image-c9ad17e0-f697-4b34-8290-c725bd8cc2f4.png)
1. 選擇調整節點集區
![image.png](/.attachments/image-cf9cbbbf-6efb-40c2-bd30-8eeadebd041a.png)
1. 設定手動 or 自動 scale node
![image.png](/.attachments/image-cfeb3d0f-18a7-4f21-8df1-ffd54ef650a6.png)

