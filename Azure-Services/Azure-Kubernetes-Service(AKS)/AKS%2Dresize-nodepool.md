[[_TOC_]]


# 在 Azure Kubernetes Service (AKS) 中調整節點集區

由於部署數目增加或執行較大的工作負載，您可能會想要變更虛擬機器擴展集方案或調整 AKS 執行個體的大小。 

不過，根據 AKS 的支援原則：
AKS 代理程式節點在 Azure 入口網站中會顯示為一般的 Azure IaaS 資源。 
但這些虛擬機器會部署到自訂的 Azure 資源群組 (通常前面加上 MC_*)。 
您無法使用 IaaS API 或資源，直接進行任何這些節點自訂。 
未透過 AKS API 完成的任何自訂變更都不會因升級、調整規模、更新或重新啟動而留存。

此持續性不足也適用於調整作業大小，因此不支援以這種方式調整 AKS 執行個體的大小。


**重要**

**此方法專屬於虛擬機器擴展集型的 AKS 叢集。 使用虛擬機器可用性設定組時，每個叢集只能有一個節點集區。**


1. 假設您想要調整現有節點集區的大小，稱為 agentpool，從 SKU 大小Standard_DS2_v5 調整為Standard_DS4_v5。 
2. 若要完成這項工作，您需要使用 Standard_DS4_v2 建立新的節點集區、將工作負載從 agentpool 移至新的節點集區，並且移除 agentpool。 
3. 在此範例中，我們將呼叫這個新的節點集區 agentpool2。

---

# 測試情境 & 步驟說明
- 原SKU: D2s_v5 一般用途 2  8 4 3750 0 支援 $2,632.75
- 新SKU: D4s_v5 一般用途 4 16 8 6400 0 支援 $5,265.50


## 登入 Azure Cloud Shell、取得AKS授權
- 訂閱帳戶: Sandbox CAF測試_1
- 資源群組: rg-walsin-development
- AKS: WalsinAKSTest01


```
az login --tenant 97876bed-bb9a-4617-802b-94c62e7837b5
az account set --subscription fd758007-06fa-47a6-af48-a500d2b9f23f
az aks get-credentials --resource-group rg-walsin-development --name WalsinAKSTest01
```


## 使用所需的 SKU 建立新的節點集區
注意: 根據實際節點(node)數，設定參數--node-count


```
az aks nodepool add \ 
    --resource-group rg-walsin-development \ 
    --cluster-name WalsinAKSTest01 \ 
    --name agentpool2 \ 
    --node-count 1 \ 
    --node-vm-size Standard_DS4_v5 \ 
    --mode System \ 
    --no-wait
```

**注意**

**每個 AKS 叢集必須包含至少一個系統節點集區，且當中至少要有一個節點。 
在上述範例中，我們使用 System 的 --mode，因為假設叢集只具有一個節點集區，則需要 System 節點集區加以取代。 
節點集區的模式可以隨時更新。**
![image.png](/.attachments/image-3732d5d1-326f-4cdb-a0ea-04b986d3282f.png)

## 建立新節點集區(VM)
1. 新增節點集區
![image.png](/.attachments/image-bbde289d-3141-4b06-9477-b1b7b418277d.png)
1. 設定基本項目
![image.png](/.attachments/image-42392000-48b2-486e-badf-a8035c472ffe.png)
1. 選擇性項目
![image.png](/.attachments/image-b9d8647c-2dba-44b6-8458-34c1c29951c2.png)


- AKS 節點集區
![image.png](/.attachments/image-5a1086b5-2abc-4600-b73e-ce2788dc2282.png)
- AKS 節點
![image.png](/.attachments/image-118faa26-1c93-403f-83ed-2e09ae975fec.png)	

- 檢查VMSS (虛擬機器級別集合)
![image.png](/.attachments/image-c2d084ac-d5cb-4d2a-9193-702d26c041e2.png)


## 封鎖現有節點
封鎖將指定的節點標示為不可排程，並防止更多 Pod 新增至節點。

`kubectl cordon aks-agentpool-35671467-vmss000001`
	
		   
## 清空現有節點

**重要**
**若要成功清空節點並收回執行 Pod，請確定任何 PodDisruptionBudgets (PDB) 允許一次移動至少一個 Pod 複本。 
否則，清空/收回作業將會失敗。 
若要檢查這一點，您可以執行 kubectl get pdb -A 並驗證 ALLOWED DISRUPTIONS 至少一或更新版本。**

**重要**

**需要使用 --delete-emptydir-data 來收回 AKS 建立的 coredns 和 metrics-server Pod。 如果未使用此旗標，則預期會發生錯誤。 如需詳細資訊，請參閱下列 emptydir 上的文件。**

`kubectl drain aks-agentpool-35671467-vmss000001 --ignore-daemonsets --delete-emptydir-data`

![image.png](/.attachments/image-a449d6a9-775e-484c-862f-76e6ec14317b.png)

												
清空作業完成後，由精靈集合以外所控制的所有 Pod 都會在新的節點集區上執行：
kubectl get pods -o wide -A


## 作業完成
![image.png](/.attachments/image-97057323-97a0-446f-a1f9-25ec91967b91.png)



## 移除現有的節點集區
az aks nodepool delete \
    --resource-group rg-walsin-development \
    --cluster-name WalsinAKSTest01 \
    --name agentpool

![image.png](/.attachments/image-b354cc8a-7d59-4e4e-b0bf-44ae2a51ba9b.png)

![image.png](/.attachments/image-4838c627-ee24-4020-940e-d54606f365ed.png)

![image.png](/.attachments/image-cde3a38c-26d2-4b87-8bd9-da314f44fb36.png)

