[[_TOC_]]

# 說明
來源端: 開發環境 DI 服務 且 訓練完成的模型
目的端: 正式環境 DI 服務

# 必要條件
授權給受控服務:
- RBAC 儲存體 Blob 資料參與者 授權給 DI服務
![image.png](/.attachments/image-051ba442-de1f-4012-9a0a-f6e9170b71e2.png)

授權給開發同仁帳號:
- 認知服務參與者
- 儲存體 Blob 資料參與者
- Azure AI Developer


# 步驟一 來源專案分享與授權
![image.png](/.attachments/image-6b213e70-0cbc-42df-a82a-989f35b9cf6b.png)

# 步驟二 建立目的端專案
![image.png](/.attachments/image-4cc9e9a7-bfc5-40f8-a5ad-8fd5c133af4e.png)

選擇 目的端 訂閱帳戶/資源群組/DI服務/API反本
![image.png](/.attachments/image-5430a211-a6e2-4635-aa02-279fb94fc375.png)

選擇 Storage Account 存放模型訓練的定義檔、結果檔
![image.png](/.attachments/image-dd4b7371-c4d8-4484-8b59-fd680a404290.png)

建立
![image.png](/.attachments/image-add69746-0415-4798-ad92-8cb97a81b0d4.png)


# 步驟三 複製模型
到來源端專案，
挑選模型 > 複製
選目的端專案
![image.png](/.attachments/image-ad0eada1-128a-4f8f-914d-8eaadb1538ab.png)

![image.png](/.attachments/image-7ed5317a-2eca-4793-988b-d9b08d20c512.png)

# 步驟四 檢查Storage 檔案
![image.png](/.attachments/image-fd05a12f-1ded-42be-aa15-5b655378258c.png)

# 完成