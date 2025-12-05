[[_TOC_]]


# 步驟
1. 選擇訂閱帳戶及資源群組
1. 建立 Azure AI Services 服務
1. 建立 AI Search 服務


# Azure AI Services
參考資訊

```
Basics
  名稱: aai-walsin-sandbox-dev
  位置: eastus2
  Pricing tier: Standard S0
Network
  All networks, including the internet, can access this resource
```

![image.png](/.attachments/image-1a91b5c0-3732-4679-9a57-3f0703377813.png)
![image.png](/.attachments/image-636690c5-5628-4382-bc0f-7b6690e7121e.png)


# Azure AI Search
參考資訊
```
Basics
  名稱: srch-crm-poc  
  位置: Southeast Asia
  Pricing tier: Free F0
Network
  All networks, including the internet, can access this resource
```

![image.png](/.attachments/image-aaed1fda-caf9-4742-a2d1-95fa92eb803f.png)
![image.png](/.attachments/image-d931191c-5a08-4a26-a744-f8613b8b2d28.png)



## 建立中樞 / 建立專案


```
中樞和專案: 
  名稱: Hub-AAI-Studio-Sandbox-dev
  訂用帳戶: WALSIN-Azure-RPA-Dev
  資源群組: rg-aai-dev
  位置: eastus2
  中樞: Hub-AAI-Studio-Sandbox-dev
  資料儲存體: 
	 (新) 中樞、儲存體、Key Vault、AI 服務
  公用網路存取: 已啟用
```

### 到 Azure AI Foundry
![image.png](/.attachments/image-557df7b0-d061-4edd-a6dc-633cbb7e341a.png)

### 根據訂閱帳戶 需建專案中樞
![image.png](/.attachments/image-3bf4b7d7-89b6-4dea-95aa-c13de7c62056.png)
![image.png](/.attachments/image-acb93902-353a-43f2-a061-e60159dc07d1.png)
![image.png](/.attachments/image-b2550f8e-7bf3-4f4b-83d3-d8d9ce6841fa.png)

### 專案概觀
![image.png](/.attachments/image-9cd849d4-146a-4d2c-83c2-0dc08d619382.png)

![image.png](/.attachments/image-14b1d524-7574-4282-bf9f-ea4d374a794c.png)


## 中樞服務 建立 Log Analytics 工作區 及 Application Insights
Log Analytics 工作區
  DefaultWorkspace-hub-aai-studio-sandbox-dev
Application Insights:
  appi-hub-aai-studio-sandbox-dev

![image.png](/.attachments/image-68660bc2-7e88-4b30-8e3f-43f94b83df66.png)


 
