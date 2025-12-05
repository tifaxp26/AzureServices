[[_TOC_]]


# 帳戶概觀

## 帳戶類型
綁定 Azure 訂用帳戶，功能服務隨用隨付
訂用帳戶: WALSIN-Azure-Management
資源群組: rg-ms-purview
資源名稱: mspurview-prod

![image.png](/.attachments/image-81af8d72-22ff-4349-96cc-2ff44744ad7a.png)
![image.png](/.attachments/image-d0d2a177-dbf7-4798-9330-aea8b681f7a5.png)

## 整合的目錄
啟用 Virtaul Network
![image.png](/.attachments/image-25bd8eb6-2617-4214-b86f-dca3b2d607a7.png)
![image.png](/.attachments/image-c47b443d-8414-4407-8341-642f0dac0555.png)


# Azure 
訂用帳戶中，會看到服務
![image.png](/.attachments/image-98babf3e-9b2e-46bd-aa59-18ad4e79e8ec.png)

## Settings

### Networking
- Firewall


- Private Endpoint connections

建立私人端點，適用 account/portal/platform
注意: 要註冊新域名
- 地端DNS: Forwarding，
- 雲端DNS: Forwarding 及 Private DNS Zones

![image.png](/.attachments/image-3b54b72d-ee67-4798-a28a-89a9f2a60cb4.png)

基本
![image.png](/.attachments/image-bae07557-a290-47ee-8627-6e6eea9654d9.png)

資源
![image.png](/.attachments/image-8fb8336a-ff67-4a3c-a420-49b88f052ccd.png)

虛擬網路
![image.png](/.attachments/image-428df12f-48a8-4ca9-8c20-f3e794dda707.png)

DNS
訂閱帳戶: WALSIN-Azure-Management
資源群組: rg-management
![image.png](/.attachments/image-4b621bb5-e367-4d3c-8beb-1aee9f8c24eb.png)


- Ingestion private Endpoint connections



## 私人DNS區域

- privatelink.purview.azure.com 
------------------------------
![image.png](/.attachments/image-bffecd7a-2186-40f7-a088-2a8cbb2406f6.png)

- privatelink.purviewstudio.azure.com
-----------------------------------
![image.png](/.attachments/image-66eb13d7-a3d7-4dc4-b1a1-70948098e18a.png)

- privatelink.purview-service.microsoft.com
-----------------------------------------
![image.png](/.attachments/image-62987e60-8873-473c-bb66-fa99f02a289e.png)


## DNS forwarding ruleset
確認以下
- Rule Name: rule-purview-azure-com
- Rule Name: rule-purviewstudio-azure-com
- Rule Name: rule-purview-service-microsoft-com

![image.png](/.attachments/image-eec23dfa-16b4-4f70-b34e-bcef23ab4d1d.png)


---

# 整合執行階段
地區: 東南亞
![image.png](/.attachments/image-3ba25a80-65dd-4e07-a0b3-984427fc9d70.png)
![image.png](/.attachments/image-03d9232a-7a26-4900-a6e7-4d7d7b70955b.png)

---

# 受控私人端點

建立 受控虛擬網路 VNET
名稱: ManagedVENT-SEA
![image.png](/.attachments/image-6e64f34d-10cc-413d-b35e-096e158ab80b.png)

建立 受控私人端點 MPE
名稱: mpe-sql-dataplatform-prod-01
連接到 sql-dataplatform-prod-01
受控虛擬網路: ManagedVENT-SEA
![image.png](/.attachments/image-724ef9f2-c52c-4fc8-a217-ed0985378db6.png)
![image.png](/.attachments/image-5c1edee9-4db4-4118-98b7-84b185f3aec8.png)

到 sql-dataplatform-prod-01 網路 > 私人存取，進行核准
![image.png](/.attachments/image-ba0b7f21-969b-42b1-9ba8-0b048bde60ba.png)

完成私人端點串接
![image.png](/.attachments/image-71c30fbf-36f7-4385-ac0d-4a780e94bcae.png)

把 kv 也接上 (資料庫連線會用到)
![image.png](/.attachments/image-174a5e0e-3053-48f8-b80e-876034e20830.png)
![image.png](/.attachments/image-6d75f173-07c1-48e1-aacb-c5cbb8af26e4.png)

RBAC
- 新增 SQL DB 參與者: mspurview-prod
- 新增 SQL Server 參與者: mspurview-prod
- 新增 Key Vault 祕密員: mspurview-prod

---

# 資料來源
註冊資料來源
![image.png](/.attachments/image-b058da7f-6790-47b6-a7be-c4c9e4a545b6.png)

進行掃描
名稱: Scan-dp-prod-l5dip
透過整合執行階段連線: IntegrationRuntime-SEA (受控虛擬網路) (正在執行)
伺服器端點
選取資料庫: 來自Azure訂閱
資料庫名稱
認證: db-secret-l5dip
![image.png](/.attachments/image-a1b4b085-8aee-47b8-bda5-9d9f9a04ac50.png)

掃描完成
![image.png](/.attachments/image-8eabc03e-a44f-412c-b688-bcc3474636ab.png)
![image.png](/.attachments/image-1ce9b134-af60-4076-830b-9e0a96cb76ee.png)

--- 

# 認證
新增 KV 服務認證
名稱: db-secret-l5dip
![image.png](/.attachments/image-65c1d848-ddfb-4eb2-a656-8156019aba11.png)
![image.png](/.attachments/image-c6288d1a-1b26-473d-87a2-06bb28384077.png)

---

# 譜系連線
新增 Data Factory
![image.png](/.attachments/image-0cd4bf73-a0db-412d-bcb2-62a327e1cc90.png)
![image.png](/.attachments/image-6ace734a-7daa-4f70-893a-c998fb6bbf7c.png)
