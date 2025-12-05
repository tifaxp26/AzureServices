[[_TOC_]]

# Azure DevOps CI/CD with AKS
以下教學為 如何設定 CI/CD，自動化建置、部署到 AKS

## 新增 pipeline
![image.png](/.attachments/image-d315252a-06eb-4a22-8eaf-f3e4d41ab852.png)

## 選擇 Repo
![image.png](/.attachments/image-750b8439-e1d6-4d9b-a332-872647b02342.png)

## 選擇 Configure
Deploy to Azure Kubernetes Service
![image.png](/.attachments/image-e2e924bf-476d-4044-919c-4f39b049c1e1.png)

## 選擇 訂閱帳戶
![image.png](/.attachments/image-9787e890-dc9c-48f3-a7ca-0b58b07d3f58.png)

## 提供 AKS 相關資訊
- 叢集名稱
- 命名空間
- ACR 名稱
- Image 名稱
- Port
![image.png](/.attachments/image-99debcea-ada8-426f-831e-ea9445a83800.png)

## DevOps Environments 會自動建立 AKS專案環境
![image.png](/.attachments/image-a06afa3b-72a7-4744-aabd-a8e8feb313c0.png)

## Repos 會自動建立 AKS 佈署 YAML 檔
- deployment.yml
- serivce.yml
- azure-pipelines.yml
![image.png](/.attachments/image-a3ab8568-7eb6-49f4-966c-25b072fdc143.png)


## 執行 & 驗證 CI/CD
![image.png](/.attachments/image-f1aefee5-e658-4a06-a9f4-a1570dacec5a.png)

# 調整項目
為符合專案命名需求，以下說明細項調整:

## 修改 azure-pipelines.yml

- imageRepository: image 名稱
- tags: 加入 latest
- environment: 為 DevOps Environments 名稱
環境命名: 不可有 Walsin-{專案名稱}-dev
![image.png](/.attachments/image-20ed5498-09a8-4683-9b0a-a977b16763d8.png)

![image.png](/.attachments/image-dab8b915-7efe-4d9b-ad8b-22383f99e887.png)


## 修改 deployment.yml
以下可調整為專案名稱
- metadata.name
- matchLabels.App
- labels.app
- containers.name

![image.png](/.attachments/image-c3d580ec-e207-42ad-8548-e11c44b6be85.png)

## 修改 service.yml
以下可調整為專案名稱
- metadata.name
- selector.app

type 改為 ClusterIP
- LoadBalancer: 叢集外部IP
- ClusterIP: 內部服務IP
![image.png](/.attachments/image-212ca127-31c1-442f-87ab-50a3b15f7c06.png)

# 驗證Azure服務
## Azure Container Registry
驗證 存放庫(image)名稱
![image.png](/.attachments/image-3a73299f-888d-4256-9d5c-f1b4bf553caa.png)

## Azure Kubernetes Service
驗證 服務名稱
![image.png](/.attachments/image-f28f5372-d3af-4758-969d-7f3739eff3b2.png)

---

# Pipelines 手動環境設定
## 命名空間定義:
**{專案名稱}-{環境}**

## 建立環境

1. **命名規則: AKS-{命名空間}**
![image.png](/.attachments/image-32be53ee-f0be-408a-9adb-3fc7d64a8b63.png)

1. 建立資源
![image.png](/.attachments/image-d6e2dc51-7961-442d-81f5-1ca3e56bed0c.png)

1. Service connections 會自動產生服務連接
![image.png](/.attachments/image-8a06597f-ac8d-4f98-b8d8-de5d81bda380.png)

## Service connections 
**命名規則: {ACR名稱}-{命名空間}**

- 手動建立 ACR 服務
選擇 Docker Registry
![image.png](/.attachments/image-d73b6345-36a7-41dd-bd5f-ece00b07101d.png)
選擇 Azure Contaner Reistry
![image.png](/.attachments/image-48104b37-b002-4982-beab-9c39e29faa09.png)

![image.png](/.attachments/image-6703923c-b886-40ae-b0c9-90af9969bee2.png)
- 並取得 id
![image.png](/.attachments/image-abff194b-6bd7-4735-9c2c-3cfaf7050e44.png)
- 可用於 azure-pipelines.yml 指定 dockerRegistryServiceConnection
![image.png](/.attachments/image-3439c5cd-48d1-4ed6-8aca-3f4c21e0427b.png)
