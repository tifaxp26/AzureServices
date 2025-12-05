
# 什麼是 Azure Kubernetes Service？

Azure Kubernetes Service (AKS) 藉由將作業負荷卸載至 Azure，簡化在 Azure 中部署受控 Kubernetes 叢集的作業。 
由於託管的 Kubernetes 服務，Azure 會處理重要的工作，例如健康情況監控和維護。 
您建立 AKS 叢集時，就會自動建立並設定控制平面。 
此控制平面會免費以受控 Azure 資源的形式提供，使用者無需加以管理。 
您只需支付和管理連結至 AKS 叢集的節點。

您可以使用下列專案建立 AKS 叢集：

- Azure CLI
- Azure PowerShell
- Azure 入口網站

範本驅動的部署選項，例如 Azure Resource Manager 範本 、 Bicep 和 Terraform。
當您部署 AKS 叢集時，您可以指定節點的數目和大小，而 AKS 會部署及設定 Kubernetes 控制平面和節點。 
您可以在部署程式期間設定進階網路 功能、 Microsoft Entra 整合 、 監視 和其他功能。

如需 Kubernetes 基本概念的詳細資訊，請參閱 AKS 的 Kubernetes 核心概念。

參考:
https://learn.microsoft.com/zh-tw/azure/aks/intro-kubernetes


---
# 實際常用案例
利用namespace 規劃各個專案，AKS 會自動共享VM 資源。
如: 
(namespace): 專案1、專案2 、專案3、專案4、專案5....
會自動分配 nodes(vm resouce) 資料


## 甚麼情況新增 Kubernetes 服務(AKS)
一般分為 開發環境、正式環境

## 甚麼情況新增 節點集區(agent Pool) / VMSS
當業務需求越來越多、系統規模增加 或 特殊用途系統，建議新增 或 升級VM SKU。
此時可利用標籤來定義 agent pool
1. set label
1. 建議使用1組 系統pool、其他使用user pool

當硬體設定CPU、Memory、Disk 明顯不足時，
可利用自動擴展方式 auto-scale
此時 AKS 仍為一套不受影響


## 為此節點集區選擇 [系統] 或 [使用者] 模式。

在 Azure Kubernetes Service （AKS）中，相同組態的節點會群組在一起成 節點集區。 
節點集區包含執行應用程式的基礎 VM。 
系統節點集區和用戶節點集區是 AKS 叢集的兩種不同的節點集區模式。 
系統節點集區是裝載重要系統 Pod 的主要用途，例如 CoreDNS 和 metrics-server。 
用戶節點集區是裝載應用程式Pod的主要用途。 
不過，如果您想要在 AKS 叢集中只有一個集區，則可以在系統節點集區上排程應用程式 Pod。 
每個 AKS 叢集必須包含至少一個系統節點集區，且當中至少要有一個節點。

**重要**
`如果您在生產環境中為 AKS 叢集執行單一系統節點集區，建議您針對節點集區使用至少三個節點。`

系統 Pod 會優先使用系統節點集區 (用於讓 AKS 持續執行)，且這類集區有大小及其他限制，以確保其具備足夠的容量來執行這些 Pod。
雖然應用程式 Pod 可能會在系統節點集區上排程，但您的應用程式 Pod 還是會優先使用使用者節點集區。

system node一个集群只需要有一个就行了，一些固有的配置服务（不需要经常做迭代或改动的）可以放在system node上。
应用服务数据库等可以放在user node上。

为了保证服务质量，尽量不要在system node上负载过大，因为它上面要host critical system pods, 例如coredns之类的





## 甚麼情況新增 節點(Node) / VM
AKS 會自動scale node

## 甚麼情況新增 Pod
根據服務訪問量，採用 水平擴展 方式。


## 【如何指定Agent Pool】
for ingress、deployment、service...
1. add lebal
2. set nodeSelect = lebal
3. https://kubernetes.io/docs/concepts/scheduling-eviction/assign-pod-node/



