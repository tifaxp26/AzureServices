[[_TOC_]]

# 說明
專案名稱: Marketplace
系統採用 Azure 服務
正式環境
先申請系統 domain 網域名稱


系統建置步驟

# Azure Container Registry
## 概觀
![image.png](/.attachments/image-73886057-68aa-4cd0-b0d7-6a7e3c6a812d.png)
## 確認存放庫
![image.png](/.attachments/image-0ccf2970-02b8-4854-9984-635b692e0bf4.png)
## 確認影像檔
![image.png](/.attachments/image-2a6ddacb-834c-42b1-8f7e-2f584a31d9de.png)


# Azure App Service
## 概觀
OS: Linux
容器化佈署
![image.png](/.attachments/image-b7a8337f-1714-428f-8ad1-f0e4b1a8d173.png)
## 確認部署中心
![image.png](/.attachments/image-e9d6dc38-f53f-4568-b906-0dfd4f6987e7.png)
## 設定環境變數
時區
![image.png](/.attachments/image-939b216d-d77c-4869-8bb8-ad8a97c8ad7b.png)
## 設定組態
啟用 Always On
![image.png](/.attachments/image-f92018d2-899f-4e34-98c4-220845f77ed4.png)
## 開啟Application Insights
![image.png](/.attachments/image-a8cc2250-b43a-4f89-bebc-c11a8acdbfd2.png)
## 開啟身分識別(受控識別)
![image.png](/.attachments/image-8e284500-38dd-43da-a153-616cc052c932.png)
## 如果有前後端呼叫，記得這裡要開啟CORS
![image.png](/.attachments/image-61bd8dab-7f83-419f-a986-3ff580d15156.png)
## 開啟App Service 記錄(Log檔)
![image.png](/.attachments/image-9e0d772e-f6b8-4811-8c44-4cb37ba01095.png)

## 查找Logs
開啟進階工具
![image.png](/.attachments/image-84327e6d-0057-4aa1-b643-c90294aae123.png)
新版介面
{appUri}/newui/
![image.png](/.attachments/image-91474abc-52d0-4339-8ceb-8f783d6e3289.png)

## IAM

想要訪問的Log檔案存儲在Kudu，
需要開啓kudu讀取權限，
詳情可參考：
https://learn.microsoft.com/en-us/azure/app-service/resources-kudu#rbac-permissions-required-to-access-kudu

默認角色Web contributor和contributor已包含kudu訪問權限。
考慮到web contributor中包含了太多其他權限，因此建議您將Reader角色進行clone，然後單獨添加**Microsoft.Web/sites/publish/Action權限**
經過測試，kudu可以被訪問。

![image.png](/.attachments/image-29289555-8e12-44ab-ae8b-523fa9886f87.png)
![image.png](/.attachments/image-7bdfbcba-1c2d-4e03-a9eb-e3c4f1c0311e.png)





# Azure SQL Server
## 建立SQL Server
![image.png](/.attachments/image-136865ce-2c04-4fb1-b5aa-08a3367760c8.png)
## 建立彈性集區
sqlserver-walsin-elasticpool-mktpl-P01
## 建立資料庫
AZMKTPLDBP01
AZPORTALDBP01
AZWALSIN_FLOWABLEDBP01


# Azure Keyvault
## 概觀
![image.png](/.attachments/image-38a6b0c7-2553-4810-8973-dcd78ffc5322.png)
## 建立IAM
![image.png](/.attachments/image-a59ec2b4-2b97-495b-a0cc-99e4c08e34c4.png)
![image.png](/.attachments/image-95a9ca23-f96c-47d7-a2e0-2ea6afdd7778.png)

# Azure Application Gateway
## 概觀
華新內部服務 是以 AZ-APGWP02 為主。
![image.png](/.attachments/image-a132cbd7-30eb-4469-a0e5-02eb307b4ec4.png)

## 建立後端集區
![image.png](/.attachments/image-8afd27b9-1f0d-406a-8c9d-c7c122706376.png)
![image.png](/.attachments/image-a7a0b00a-5894-48c8-b690-63287f1ddec3.png)

## 建立後端設定
![image.png](/.attachments/image-d7a02cd8-59a7-4b24-88f5-9c89e7fccb58.png)
![image.png](/.attachments/image-c341f213-d974-4139-a123-f48382aabe44.png)
![image.png](/.attachments/image-a963598a-c355-48d9-a46a-331fee00c456.png)

## 設定監聽程式
![image.png](/.attachments/image-2063181f-9ddf-445b-95d6-669550fb3e81.png)
私人連線
![image.png](/.attachments/image-de82dbc5-63df-4e27-b650-c7ceb5a20dc7.png)
![image.png](/.attachments/image-27765d16-21fc-4d2e-862f-7fabb588bd69.png)
公用連線
![image.png](/.attachments/image-8c9f4fd8-9d9e-48e1-a7e4-e58af5e1f7a3.png)
![image.png](/.attachments/image-e15f10f5-a11d-4d98-8839-140991149d9a.png)

## 設定規則
![image.png](/.attachments/image-8a3af0e6-983c-4773-a446-8c0011888918.png)
公用連線
![image.png](/.attachments/image-aaba8b68-79c9-423f-b5ce-a52458ef3752.png)
![image.png](/.attachments/image-6f07a7b2-bc7c-4c20-86be-10d3246de3bb.png)
私人連線
![image.png](/.attachments/image-5996d4d1-54eb-457f-8980-09522434ffae.png)
![image.png](/.attachments/image-a00402c3-d5c8-4c36-bc17-6fba27fdab75.png)

## 重寫
由於使用到 CAS 系統作為登入 並轉址服務，因此這邊要設定 Location ReWrite
![image.png](/.attachments/image-153f790c-5a19-4183-9b9a-98cc591434c1.png)

建立名稱
![image.png](/.attachments/image-cc87c9d9-b277-460c-943b-d7c0e60c9b73.png)

要比對的模式 (https?):\/\/.*azurewebsites.net(.*)$
![image.png](/.attachments/image-614c5a0c-0c97-44bf-bc51-b1dcce0536f3.png)
標頭值  {http_resp_Location_1}://marketplace.walsin.com{http_resp_Location_2}
![image.png](/.attachments/image-468e83c6-3be2-4d15-935e-73e190790125.png)


# Azure VNET

# Azure Storage Account
## 概觀
建立 儲存體帳戶
帳戶種類:StorageV2 (一般用途 v2)
階層式命名空間: 已啟用

![image.png](/.attachments/image-17acdb46-19ab-4c08-856b-a651b95ed20c.png)

