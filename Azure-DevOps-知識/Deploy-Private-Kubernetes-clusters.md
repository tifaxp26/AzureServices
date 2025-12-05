[[_TOC_]]


       
# Azure 環境設定
因為部署環境 與 各專案 AKS 環境不同、訂閱帳號不同、VNET 也不同...
佈版機環境需要跨到各網段。

## 開通 FW network，來源端 IPGroup 及 目的端 IPGroup
![image.png](/.attachments/image-ce5dba18-f52e-4829-a058-090154ba5478.png)


## AKS 與 ACR 連線測試
az aks check-acr --resource-group rg-l5dip-dev --name aks-l5-dev --acr acrwalsind01.azurecr.io
![image.png](/.attachments/image-e5a89eff-728e-42a9-93d3-9b2690780c15.png)


## 容器登錄 設定
IAM授權
角色: AcrPull
對象: 使用這、群組或服務主體，選 aks-l5-dev-agentpool
![image.png](/.attachments/image-d7f4d9fa-4213-4b4c-84a6-9f309ea9433a.png)
![image.png](/.attachments/image-a4e8be25-cbaa-467f-92f0-753268e48098.png)

---

# Azure DevOps Service 設定
因為使用私人叢集，API 伺服器位址 外部網路是無法查找到的。
![image.png](/.attachments/image-1321383a-83d8-4586-a0cd-55d5e6c72279.png)

因此需要內部網路準備一台VM 作為部署專用。


## 建立(空)環境
![image.png](/.attachments/image-7f54a7e3-ea3c-43cc-85f6-14b31964421c.png)
![image.png](/.attachments/image-7ef3b992-5008-4e2c-bf22-15498564b3e2.png)


## 使用 ARM
到 Project/Service Connetors
![image.png](/.attachments/image-2f8cd37d-38d1-4475-a4b2-f3ede0099b1d.png)
![image.png](/.attachments/image-e708c144-9cb5-4b22-8cf9-7cf2e3836ddb.png)


### 建立 Release (使用環境)
![image.png](/.attachments/image-b1858a88-b6ee-4510-925d-3b2d657256d8.png)


### 建立 Release (使用ARM)
![image.png](/.attachments/image-d25f1612-e77c-4ca7-ab67-204e7832fade.png)

