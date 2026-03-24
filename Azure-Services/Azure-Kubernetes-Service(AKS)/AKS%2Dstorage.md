[[_TOC_]]


# Azure Kubernetes Service (AKS) 中的應用程式適用的儲存體選項
https://learn.microsoft.com/zh-tw/azure/aks/concepts-storage

本文將介紹為 AKS 中的應用程式提供儲存體的核心概念：

- 磁碟區
- 永續性磁碟區
- 儲存類別
- 永續性磁碟區宣告

Azure Kubernetes Service (AKS) 叢集中的應用程式適用的儲存體選項
![image.png](/.attachments/image-c5fc8adc-2f4d-491c-8afb-e4c270dfbc36.png)





## Azure 磁碟
使用 Azure 磁片 來建立 Kubernetes DataDisk 資源。 磁碟類型包括：

- Ultra 磁碟
- 進階 SSD
- 標準 SSD
- 標準 HDD

提示
針對大部分生產和開發工作負載，請使用進階 SSD。

因為 Azure 磁片會掛接為 ReadWriteOnce，所以只能供單一節點使用。 若要讓 Pod 同時存取多個節點上的儲存體磁片區，請使用 Azure 檔案儲存體。



## Azure 檔案儲存體
使用Azure 檔案儲存體將伺服器訊息區掛接 (SMB) 3.1.1 版共用或網路檔案系統 (NFS) 4.1 版共用，以將 Azure 儲存體帳戶支援的共用掛接至 Pod。 Azure 檔案儲存體能讓您在多個節點和 Pod 之間共用資料：

- 高效能 SSD 支援的 Azure 進階儲存體
- 一般 HDD 支援的 Azure 標準儲存體


## Azure Blob 儲存體
使用 Azure Blob 儲存體建立 Blob 儲存體容器，然後使用 NFS v3.0 通訊協定或 BlobFuse 掛接。

- 區塊 Blob



## 永續性磁碟區
磁碟區會定義並建立為 Pod 生命週期的一部分，且在 Pod 刪除後就不會存在。 如果 Pod 在維護事件期間 (尤其是在 StatefulSet 中) 重新排程於不同的主機上，Pod 通常會預期其儲存體能持續保存。 永續性磁碟區 (PV) 是由 Kubernetes API 建立和管理的儲存體資源，可跨個別 Pod 的存留期持續保存。

您可以使用Azure 磁片或Azure 檔案儲存體來提供 PersistentVolume。 如先前關於磁碟區章節所說明，應選擇 Azure 資料箱磁碟或是檔案儲存體，通常取決於資料是否同時存取或是效能層級的需求。

Azure Kubernetes Service (AKS) 叢集中的永續性磁碟區"
![persistent-volumes.png](/.attachments/persistent-volumes-5aeb91ed-b072-4ede-a5e9-a8d1904b6218.png)


叢集管理員可以 以靜態方式 建立 PersistentVolume，或由 Kubernetes API 伺服器 動態 建立磁片區。 如果已排程 Pod 並要求目前無法使用的儲存體，Kubernetes 可以建立基礎 Azure 磁片或檔案儲存體，並將其連結至 Pod。 動態佈建會使用 StorageClass 來識別需要建立的 Azure 儲存體類型。



## 永續性磁碟區宣告
PersistentVolumeClaim 會要求特定 StorageClass、存取模式和大小的儲存體。 若根據 StorageClass 中的定義，並沒有現有資源可用來履行宣告，Kubernetes API 伺服器可在 Azure 中動態佈建基礎 Azure 儲存體資源。

在磁碟區連線至 Pod 後，Pod 定義即會包含磁碟區掛接。

Azure Kubernetes Service (AKS) 叢集中的永續性磁碟區宣告
![persistent-volume-claims.png](/.attachments/persistent-volume-claims-60bec5dc-dea6-425d-a830-68c0b180e2db.png)

可用的儲存體資源一經指派給發出要求的 Pod 後，PersistentVolume 就會繫結至 PersistentVolumeClaim。 永續性磁碟區是對應至宣告的 1:1。


--- 
# 使用 Azure Blob 儲存體容器儲存體介面 (CSI) 驅動程式

## Azure Blob 儲存體 CSI 驅動程式支援下列功能：
- BlobFuse 和網路檔案系統 (NFS) 版本 3.0 通訊協定


## 開始之前
- 您必須安裝並設定 Azure CLI 2.42 版或更新版本。 執行 az --version 以尋找版本。 如果您需要安裝或升級，請參閱安裝 Azure CLI。

- 如果您之前已安裝 CSI Blob 儲存體開放原始碼驅動程式，請執行此連結中的步驟，從叢集存取 Azure Blob 儲存體。


## 在新的或現有 AKS 叢集上啟用 CSI 驅動程式
使用 Azure CLI，您可以在新的或現有的 AKS 叢集上啟用 Blob 儲存體 CSI 驅動程式，再設定持續性磁片區供叢集中的 Pod 使用。

若要啟用新叢集上的驅動程式，請隨著 az aks create 命令包含 --enable-blob-driver 參數，如下列範例所示：

```
az aks create --enable-blob-driver -n WalsinAKSTest01 -g rg-walsin-development\
az aks update --enable-blob-driver -n WalsinAKSTest01 -g rg-walsin-development
```



```
az aks create --enable-file-driver -n WalsinAKSTest01 -g rg-walsin-developmdent
az aks update --enable-file-driver -n WalsinAKSTest01 -g rg-walsin-developmdent
```




## 停用現有 AKS 叢集上的 CSI 驅動程式
`az aks update --disable-blob-driver -n WalsinAKSTest01 -g rg-walsin-development`

