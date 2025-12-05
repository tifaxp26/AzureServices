[[_TOC_]]


# 在 AKS Azure Kubernetes Service () 中建立和使用 Azure 磁片的磁片區 執行 kubectl get 命令來判斷每個節點可配置的磁片區數目：

永續性磁片區代表布建以供 Kubernetes Pod 使用的儲存體片段。 您可以搭配一或多個 Pod 使用永續性磁片區，並以動態或靜態方式布建它。 本文說明如何在 Azure Kubernetes Service (AKS) 叢集中動態建立具有 Azure 磁片的永續性磁片區。

**注意**
您只能使用存取模式類型 ReadWriteOnce 來掛接 Azure 磁碟，以讓它僅供 AKS 中一個節點使用。 當 Pod 在相同節點上執行時，此存取模式仍允許多個 Pod 存取磁片區。 如需詳細資訊，請參閱 Kubernetes PersistentVolume 存取模式。

本文示範如何：
安裝容器儲存體介面 (CSI) 驅動程式，並動態建立一或多個 Azure 受控磁片以連結至 Pod，以使用動態永續性磁片區 (PV) 。
建立一或多個 Azure 受控磁片或使用現有的磁片，並將其連結至 Pod，以使用靜態 PV。

# 開始之前
1. 您需要 Azure 儲存體帳戶。
1. 請確定您已安裝並設定 Azure CLI 2.0.59 版或更新版本。 執行 az --version 以尋找版本。 如果您需要安裝或升級，請參閱安裝 Azure CLI。
1. Azure 磁片 CSI 驅動程式具有每個節點的磁片區限制。 磁片區計數會根據節點/節點集區的大小而變更。 執行 kubectl get 命令來判斷每個節點可配置的磁片區數目：
`kubectl get CSINode aks-agentpool-14235471-vmss000000 -o yaml`


## 您可以使用 命令來查看預先建立的儲存體類別 kubectl get sc 。 
列範例顯示 AKS 叢集內可用的預先建立儲存體類別：
`kubectl get sc`


## 建立 Azure 磁碟
當您建立與 AKS 搭配使用的 Azure 磁碟時，您可以在節點資源群組中建立磁碟資源。 
此方法可讓 AKS 叢集存取和管理磁碟資源。 
如果您改為在另一個資源群組中建立磁片，
您必須將角色授與叢集 Contributor Azure Kubernetes Service (AKS) 受控識別給磁片的資源群組。
1. 使用 az aks show 命令識別資源組名，並新增 --query nodeResourceGroup 參數。
`az aks show --resource-group rg-walsin-development --name WalsinAKSTest01 --query nodeResourceGroup -o tsv`

1. 使用 az disk create 命令建立磁片。 指定節點資源組名和磁片資源的名稱，例如 myAKSDisk。 
下列範例會建立 20GiB 的磁碟，並在建好後輸出磁碟識別碼。 
如果您需要建立磁碟以搭配 Windows 伺服器容器使用，請新增 --os-type windows 參數以正確格式化磁碟。

```
az disk create \
  --resource-group MC_rg-walsin-development_WalsinAKSTest01_southeastasia \
  --name myAKSDisk \
  --size-gb 5 \
  --query id --output tsv
```



## 將磁碟掛接為磁碟區
1. 建立具有 PersistentVolume 的 pv-azuredisk.yaml 檔案。 
使用上一個步驟的磁碟資源識別碼更新 volumeHandle。


```
apiVersion: v1
kind: PersistentVolume
metadata:
  annotations:
    pv.kubernetes.io/provisioned-by: disk.csi.azure.com
  name: pv-azuredisk
spec:
  capacity:
    storage: 5Gi
  accessModes:
    - ReadWriteOnce
  persistentVolumeReclaimPolicy: Retain
  storageClassName: managed-csi
  csi:
    driver: disk.csi.azure.com
    readOnly: false
    volumeHandle: /subscriptions/fd758007-06fa-47a6-af48-a500d2b9f23f/resourceGroups/MC_rg-walsin-development_WalsinAKSTest01_southeastasia/providers/Microsoft.Compute/disks/myAKSDisk
    volumeAttributes:
      fsType: ext4
```

	  

1. 建立 pvc-azuredisk.yaml 檔案，其具有使用 PersistentVolume 的 PersistentVolumeClaim。

```
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: pvc-azuredisk
spec:
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 5Gi
  volumeName: pv-azuredisk
  storageClassName: managed-csi
```


	  

1. 使用 kubectl apply 命令建立PersistentVolume和PersistentVolumeClaim，並參考您建立的兩個 YAML 檔案。

```
kubectl apply -f pv-azuredisk.yaml
kubectl apply -f pvc-azuredisk.yaml
```

	  
1. 確認已建立PersistentVolumeClaim，並使用 kubectl get pvc 命令系結至PersistentVolume。
`kubectl get pvc pvc-azuredisk`

1. 建立 azure-disk-pod.yaml 檔案，以參考您的 PersistentVolumeClaim。

```
apiVersion: v1
kind: Pod
metadata:
  name: mypod
spec:
  nodeSelector:
    kubernetes.io/os: linux
  containers:
  - image: mcr.microsoft.com/oss/nginx/nginx:1.15.5-alpine
    name: mypod
    resources:
      requests:
        cpu: 100m
        memory: 128Mi
      limits:
        cpu: 250m
        memory: 256Mi
    volumeMounts:
      - name: azure
        mountPath: /mnt/azure
  volumes:
    - name: azure
      persistentVolumeClaim:
        claimName: pvc-azuredisk
```


1. 使用 命令套用 kubectl apply 組態並掛接磁片區。
`kubectl apply -f azure-disk-pod.yaml`


# 清除資源
當您完成本文中建立的資源時，您可以使用 命令加以移除 kubectl delete 。

## Remove the pod
kubectl delete -f azure-pvc-disk.yaml

## Remove the persistent volume claim
kubectl delete -f azure-pvc.yaml


