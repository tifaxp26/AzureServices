[[_TOC_]]

# 建置服務
定價層: 進階 (包含 HSM 備份金鑰的支援)
![image.png](/.attachments/image-7f347e7f-5bcc-4510-bbee-e9543a537636.png)
存取類型
採用RBAC
![image.png](/.attachments/image-a4bd8236-0c71-4295-bd2a-8d6cd2457a08.png)
網路功能
整合VNET、NSG、Router
![image.png](/.attachments/image-fe4af014-d30a-42cd-9ad3-b205b0caf0d2.png)
啟用私人端點
![image.png](/.attachments/image-19149714-a075-4bee-b1f1-49391262ec29.png)


# 作業順序
取得中信提供的憑證
我們再產生金鑰
進行簽章作業


# 建立 Azure Keyvault 服務
名稱: akv-walsin-HSMCTBC-dev
SKU (定價層): 進階 premium
![image.png](/.attachments/image-76129ea0-a14d-412a-b14a-8f651ce4eb56.png)


# 建立 憑證請求檔
憑證名稱: WalsinCSRCertificate
憑證授權單位 (CA) 的類型：非整合式 CA 所發行的憑證
主旨："CN=35412204--35412204BC"
![image.png](/.attachments/image-b8a593df-2423-4388-8801-98e87c9ab19f.png)

# 完成 CSR  & 下載
提供 憑證請求檔 CSR，
![image.png](/.attachments/image-b8fb8e8d-bfac-4fdf-aab9-4bd06f3e626b.png)


# 提供CSR 給對方完成請求後，
對方會提供憑證(.cer)檔，
我們再次進行合併作業。
1. 選擇 合併作業
1. 選擇 合併已簽署的要求
![image.png](/.attachments/image-079d55ef-139b-4406-b1ea-1cbf6a879844.png)
![image.png](/.attachments/image-5897e316-3c28-4a03-bd99-33675934df15.png)


# 下載 PFX 檔
![image.png](/.attachments/image-185d05f4-ba17-4096-a626-25a3bc3de78f.png)
![image.png](/.attachments/image-377b5262-1abb-4600-9485-dbdcd380dbb5.png)


# 產生 HSM 金鑰
![image.png](/.attachments/image-4b2a6fda-15ef-4b73-b769-751b57b63f00.png)
![image.png](/.attachments/image-da55c7ae-8cf9-4d7f-82b7-110472b4c3dc.png)

# 確認作業項目
![image.png](/.attachments/image-ca7946a0-1c8a-4731-a951-d677ca75d471.png)


提供 HSMCTBC 金鑰識別碼
金鑰名稱: HSMCTBCKey
金鑰類型: RSA-HSM
RSA 金鑰大小: 2048







