[[_TOC_]]

如果要連到其他訂閱裡的資料庫，
則需先開通 Azure FW。

由於都是內部網路服務，
所以資料庫端要另外設定Allow 方式。

# 作法一: 使用Private Link
在 SQLDB 中，建立私人端點，並串接到 DataBricks VNET
- 基本
![image.png](/.attachments/image-28a7e34d-d8d8-4aa7-bdfa-fa04625a51eb.png)

- 資源
![image.png](/.attachments/image-f307e13a-f22f-4797-a8eb-07e6a2751e4f.png)

- 虛擬網路
![image.png](/.attachments/image-22a146c5-0a6f-46da-862f-103056696d0a.png)

- DNS
![image.png](/.attachments/image-1b5d163f-c655-43f5-8a16-ddddf6a4bacd.png)


# 作法二: 開啟受信任SubNET
在 SQLDB 中，受信網路，增加 DataBricks Subnet，
是 服務專用的 Subnet，不是 private IP 那個 subnet
![image.png](/.attachments/image-bf491cc8-6de0-42b5-85e0-0f7d9a15fdf1.png)


# 作法三: 開啟受信任SubNET (推薦)
在 SQLDB 中，開啟受信網路，勾選 允許 Azure 服務和資源存取此伺服器
![image.png](/.attachments/image-40685856-7084-4e9c-a6a4-ffba6cff3f6c.png)



