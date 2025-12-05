[[_TOC_]]


# 主要參考文件

[APIOps - Documentation](https://azure.github.io/apiops/apiops/3-apimTools/apimtools-azdo-2-3-new.html)


# 必要條件
- DevOps 專案管理原
- 能夠建立 應用程式註冊的角色
 Azure 應用程式系統管理員 或 Azure 雲端應用系統管理員


# Configure APIM tools in Azure DevOps

## 建立新專案 **APIOps**
![image.png](/.attachments/image-0a538d04-0db7-4d50-83ec-995cdaf2a916.png)


# Extract APIM artifacts in Azure DevOps from extractor tool
----------------------------------------------------------
## 建立 Service Connection
選 Azure Resource Manager
選 目的端 Azure 訂閱資訊
服務名稱: conn-apim-walsin-rpa-dev

![image.png](/.attachments/image-7d1f56b8-8246-4fd7-879d-4e9356279c5c.png)
![image.png](/.attachments/image-84143362-211a-4f49-a5a5-a3179d2fb515.png)


## 建立環境
名稱: ENV apim-walsin-rpa-dev
![image.png](/.attachments/image-01fa8ae3-19d6-4b6d-af13-5ddc15354950.png)

## 設定 Approvals and checks
![image.png](/.attachments/image-25278c3b-470b-4131-8bdc-a72eb8839629.png)



## 下載 APIOps Toolkit for Azure APIM 
檔案名稱: Azure_DevOps.zip
版本(當時): **v6.0.1.7**
https://github.com/Azure/apiops/releases
![image.png](/.attachments/image-3ba56319-7b2a-4773-81a2-2b141d63d5bd.png)


## 建立變數群組
名稱: apim-automation
變數: 
- APIM_NAME: apim-walsin-rpa-dev
- apiops_release_version: **v6.0.1.7**
- RESOURCE_GROUP_NAME: rg-rpa-dev
- SERVICE_CONNECTION_NAME: conn-apim-walsin-rpa-dev

![image.png](/.attachments/image-f863d58a-3593-4305-b853-bd7bc840f8fd.png)
![image.png](/.attachments/image-e6980ae4-d0fd-4d2c-8fcd-e9394392abb5.png)


## 建立 REPO
名稱: APIM-artifacts
目錄階層要如下圖
![image.png](/.attachments/image-6f13f8a2-35b2-4dce-aafd-b864efc422aa.png)


## 授權 專案Build Service 可以對REPO操作
- Contribute
- Contribute to PR
- Create branch
![image.png](/.attachments/image-906fb35c-858d-45f9-b53c-cd5dd5c5ddcf.png)



## 建立 Pipelines

![image.png](/.attachments/image-9d427782-cd38-4661-8ea8-91b9b3a0b150.png)

![image.png](/.attachments/image-4d734d5c-19c2-4f60-9cac-589de4b0f927.png)

![image.png](/.attachments/image-c2263767-d5b8-407f-a180-1d4756a32ddf.png)

![image.png](/.attachments/image-cfab08d9-9f01-44c9-8c27-02c77c55a51d.png)

![image.png](/.attachments/image-98176c58-fc71-4279-8f92-8887288410be.png)




## Run Pipelines
有兩段
Stage A. 抓取來源端APIM
Stage B. 建立新分支，並將資料存回Repo
![image.png](/.attachments/image-b13a492d-246d-42e2-9449-ea316bf7b8ac.png)


## Run Success
服務會自動新增分支，並將來源APIM的資料料，存入新分支
![image.png](/.attachments/image-c9de3f9e-2c94-4f52-a8ba-9c5265e100ab.png)

---

# Create pipeline to automatically push changes using publisher tool

extractor 作業完畢後，會新增 PR 
到PR功能按同意
![image.png](/.attachments/image-6d0a0914-02ed-43c2-bd39-88638b4a5bdc.png)


