/mnt/secrets-store/[[_TOC_]]


# 具有秘密存放區 CSI 驅動程式的 Azure 金鑰保存庫 提供者

## 1 - 設定秘密存放區 CSI 驅動程式

在 Azure Kubernetes Service （AKS） 叢集中使用適用于秘密存放區 CSI 驅動程式的 Azure 金鑰保存庫 提供者

適用于秘密存放區 CSI 驅動程式的 Azure 金鑰保存庫 提供者可讓您透過 CSI 磁片區將 Azure 金鑰保存庫 整合為秘密存放區與 Azure Kubernetes Service （AKS） 叢集 。

功能
- 使用 CSI 磁片區將秘密、金鑰和憑證掛接至 Pod。
- 支援 CSI 內嵌磁片區。
- 支援掛接多個秘密，以單一磁片區的形式儲存物件。
- 支援使用 CRD 的 SecretProviderClass Pod 可攜性。
- 支援 Windows 容器。
- 與 Kubernetes 秘密同步。
- 支援自動調整掛接的內容和同步處理的 Kubernetes 秘密。
- 限制
- 使用 subPath 磁片區掛接 的容器在輪替時不會收到秘密更新。 如需詳細資訊，請參閱 秘密存放區 CSI 驅動程式的已知限制 。

必要條件
如果您沒有 Azure 訂用帳戶，請在開始前建立免費帳戶。
檢查您的 Azure CLI 版本為 2.30.0 或更新版本。 如果是舊版 ， 請安裝最新版本。
如果您要將輸入限制為叢集，請確定埠 9808 和 8095 已開啟。
建議的最低 Kubernetes 版本是以滾動 Kubernetes 版本支援視窗 為基礎 。 請確定您執行的是 N-2 版或更新版本 。


使用適用于秘密存放區 CSI 驅動程式支援的 Azure 金鑰保存庫 提供者建立 AKS 叢集

使用 az group create 命令建立 Azure 資源群組。
`az group create -n myResourceGroup -l eastus2`

使用 az aks create 命令建立具有適用于秘密存放區 CSI 驅動程式功能的 Azure 金鑰保存庫 提供者的 AKS 叢集，並啟用 azure-keyvault-secrets-provider 附加元件。

**注意**
如果您想要使用 Microsoft Entra 工作負載 ID，您也必須使用 --enable-oidc-issuer 和 --enable-workload-identity 參數，如下列範例所示：

`az aks create -n myAKSCluster -g myResourceGroup --enable-addons azure-keyvault-secrets-provider --enable-oidc-issuer --enable-workload-identity`

`az aks create -n myAKSCluster -g myResourceGroup --enable-addons azure-keyvault-secrets-provider`


使用適用于秘密存放區 CSI 驅動程式支援的 Azure 金鑰保存庫 提供者升級現有的 AKS 叢集
使用 az aks enable-addons 命令使用 Azure 金鑰保存庫 Store CSI 驅動程式功能的 Azure 金鑰保存庫 提供者升級現有的 AKS 叢集，並啟用 azure-keyvault-secrets-provider 附加元件。 附加元件會建立使用者指派的受控識別，以用來向金鑰保存庫進行驗證。

`az aks enable-addons --addons azure-keyvault-secrets-provider --name myAKSCluster --resource-group myResourceGroup`


確認秘密存放區 CSI 驅動程式安裝的 Azure 金鑰保存庫 提供者
使用 命令確認安裝已完成 kubectl get pods ，此命令會列出 kube-system 命名空間中具有 secrets-store-csi-driver 和 secrets-store-provider-azure 標籤的所有 Pod。

`kubectl get pods -n kube-system -l 'app in (secrets-store-csi-driver,secrets-store-provider-azure)'`



## 2 - 提供 Azure 金鑰保存庫 存取
提供身分識別，以存取 Azure Kubernetes Service 中秘密存放區 CSI 驅動程式的 Azure 金鑰保存庫 提供者 （AKS）

Azure Kubernetes Service 上的秘密存放 CSI 驅動程式 （AKS） 提供各種身分識別型存取方式，以身分識別為基礎的 Azure 金鑰保存庫。 本文概述這些方法，以及如何使用這些方法從 AKS 叢集存取金鑰保存庫及其內容。

您可以使用下列其中一個存取方法：
- Microsoft Entra 工作負載 ID
- 使用者指派的受控識別
必要條件
開始之前，請確定您已遵循在 Azure Kubernetes Service （AKS） 叢集中使用適用于秘密存放區 CSI 驅動程式的 Azure 金鑰保存庫 提供者中的步驟 ，建立具有適用于秘密存放區 CSI 驅動程式支援的 Azure 金鑰保存庫 提供者的 AKS 叢集 。





# 實際案例 - 啟用Azure KeyVault Provider Deriver

```
az aks enable-addons --addons azure-keyvault-secrets-provider --name {AKS名稱} --resource-group {資源群組名稱}

範例:
az aks enable-addons --addons azure-keyvault-secrets-provider --name AZ-AKSD01 --resource-group rg-l5-dev
```


```
az aks addon update -g {資源群組名稱} -n {AKS名稱} -a azure-keyvault-secrets-provider --enable-secret-rotation

範例:
az aks addon update -g rg-l5-dev -n AZ-AKSD01 -a azure-keyvault-secrets-provider --enable-secret-rotation
```

---

# 取得 provider clientID
`az aks show -g rg-walsin-development -n WalsinAKSTest01 --query addonProfiles.azureKeyvaultSecretsProvider.identity.clientId -o tsv`
訂閱帳戶ID
`az keyvault show --name akv-walsin-dev01 --query id -o tsv`


# Azure Keyvault IAM
將 provider 授權為 Key Vault 系統管理員
服務主體名稱: azurekeyvaultsecretsprovider
![image.png](/.attachments/image-b64b30cc-fb20-428f-a6b5-77d4da5b7bab.png)
![image.png](/.attachments/image-4db55e32-4908-4f49-9361-71e7d5e08357.png)

# 授權 provider 管理員 或 秘密使員


# 套用到叢集
kubectl apply -f akv.yml -n az-walsin-dataplatform-dev
```
# This is a SecretProviderClass example using user-assigned identity to access your key vault
apiVersion: secrets-store.csi.x-k8s.io/v1
kind: SecretProviderClass
metadata:
  name: azure-kvname-user-msi
spec:
  provider: azure
  parameters:
    usePodIdentity: "false"
    useVMManagedIdentity: "true"          # Set to true for using managed identity
    userAssignedIdentityID: <client-id>   # Set the clientID of the user-assigned managed identity to use
    keyvaultName: <key-vault-name>        # Set to the name of your key vault
    cloudName: ""                         # [OPTIONAL for Azure] if not provided, the Azure environment defaults to AzurePublicCloud
    objects:  |
      array:
        - |
          objectName: secret1
          objectType: secret              # object types: secret, key, or cert
          objectVersion: ""               # [OPTIONAL] object versions, default to latest if empty
        - |
          objectName: key1
          objectType: key
          objectVersion: ""
    tenantId: <tenant-id>                 # The tenant ID of the key vault
```

確認有沒有起來可執行 
`kubectl get SecretProviderClass -n az-walsin-dataplatform-dev`
![image.png](/.attachments/image-3e8ad7ed-6d2e-46f1-bffe-ace5073ab041.png)


套用到pod
`kubectl apply -f akv-pod.yml -n az-walsin-dataplatform-dev`
```
# This is a sample pod definition for using SecretProviderClass and the user-assigned identity to access your key vault
kind: Pod
apiVersion: v1
metadata:
  name: busybox-secrets-store-inline-user-msi
spec:
  containers:
    - name: busybox
      image: registry.k8s.io/e2e-test-images/busybox:1.29-4
      command:
        - "/bin/sleep"
        - "10000"
      volumeMounts:
      - name: secrets-store01-inline
        mountPath: "/mnt/secrets-store"
        readOnly: true
  volumes:
    - name: secrets-store01-inline
      csi:
        driver: secrets-store.csi.k8s.io
        readOnly: true
        volumeAttributes:
          secretProviderClass: "azure-kvname-user-msi"
```

![image.png](/.attachments/image-8bf2986b-e15d-44f7-82fb-a3932e7d3044.png)


# 驗證秘密 

## 列List
`kubectl exec busybox-secrets-store-inline-user-msi -n az-walsin-dataplatform-dev -- ls /mnt/secrets-store/`
![image.png](/.attachments/image-ae351ac7-32d1-45e9-9438-db13c806fdf6.png)

## 查看內容
```
kubectl exec busybox-secrets-store-inline-user-msi -n az-walsin-dataplatform-dev -- cat /mnt/secrets-store/UserKey
kubectl exec busybox-secrets-store-inline-user-msi -n az-walsin-dataplatform-dev -- cat /mnt/secrets-store/db-conn-sqlserver-walsin-dev01
```

![image.png](/.attachments/image-5a8d8972-83b4-44f7-912a-b2445640e5e3.png)


** 更新 2025/01/21
- 列List

```
kubectl exec {podname} -n {namespace} -- ls {mountPath}
kubectl exec l5dip-a3module-5f896b88b5-xlwpt -n az-walsin-l5dip-dev -- ls /mnt/secrets-store
```


- 列VALUE

```
kubectl exec {podname} -n {namespace} -- cat {mountPath/{keyname}}
kubectl exec l5dip-a3module-5f896b88b5-xlwpt -n az-walsin-l5dip-dev -- cat /mnt/secrets-store/ClientSecret
kubectl exec azure-kvname-user-msi -n az-walsin-l5dip-dev -- cat /mnt/secrets-store/db-conn-sqlserver-walsin-dev01
```





# 在現有的 AKS 叢集上停用秘密存放區 CSI 驅動程式的 Azure 金鑰保存庫 提供者
 注意
停用附加元件之前，請確定 未 SecretProviderClass 使用中。 
嘗試在 存在時 SecretProviderClass 停用附加元件會導致錯誤。

使用 az aks disable-addons 命令搭配 azure-keyvault-secrets-provider 附加元件，
停用現有叢集中秘密存放 CSI 驅動程式功能的 Azure 金鑰保存庫 提供者。
`az aks disable-addons --addons azure-keyvault-secrets-provider -g myResourceGroup -n myAKSCluster`


# C# 如何取運資料
概念: AKS 會將資料存成檔案放在 volumeMounts 
使用 `System.IO.File.ReadAllText(@"{volumes-name}/{key-name}");`
ex: `System.IO.File.ReadAllText(@"/mnt/secrets-store/UserKey");`

![image.png](/.attachments/image-ee70c95c-2c10-43f4-9264-95fc7fa130cb.png)
![image.png](/.attachments/image-b36cd1b7-a388-46f6-837e-ce36db41e1c3.png)



---
# 參考
- https://learn.microsoft.com/en-gb/azure/aks/csi-secrets-store-driver