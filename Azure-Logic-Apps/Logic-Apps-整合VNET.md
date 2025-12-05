[[_TOC_]]

# 虛擬網路
## 規劃IP網段 及 子網路
基本拆分/26 共4組子網路
定義: 10.101.150.0/26 為Private IP 來源區域
定義: 10.101.150.64/26 為虛擬網路輸出 for Microsoft.Web/serverfarms
![image.png](/.attachments/image-9e3b755b-457b-4340-9c1b-035140d52574.png)

# Logic Apps
名稱: logic-poc01
網路 > 建立私人端點
網路 > 套用輸出流量設定
![image.png](/.attachments/image-e83fd105-5a7b-4f5f-820c-fe51d057ffc0.png)
公用存取限制: 虛擬網路 + 限制IP
**AzureConnector是包括了Office 365即outlook的IPs 
我测试了这个设定，Forms和email都可以trigger**
![image.png](/.attachments/image-071fdddd-ecd3-45b4-92bf-5c535d0dc233.png)

網路 > 套用輸出流量設定 > 虛擬網路整合
![image.png](/.attachments/image-5676644e-984b-4b57-b339-b3825b1e5771.png)
網路 > 建立私人端點
![image.png](/.attachments/image-b3b46d7a-72fd-4cb1-ab41-12acef2b2652.png)


## 工作流程
標準版 可集中化 Flows
![image.png](/.attachments/image-971f8355-f294-4b8e-9598-25beca7f8e3b.png)
![image.png](/.attachments/image-7f235ed8-8045-48bc-a41c-93ba80089b4f.png)

## 連線
範例使用
- Mircosoft Forms
- Outlook
- Azure Blob
- Azure Function
- Azure Keyvault
由於服務在同VNET中，防火牆大致上不用額外開通。
![image.png](/.attachments/image-fb7fdb0f-3240-44d3-99da-bb0d9aae03ae.png)
![image.png](/.attachments/image-83dab093-2dee-4146-b3b3-44df5100732b.png)
![image.png](/.attachments/image-72571d66-8de2-4e30-a7b6-dd413c701597.png)


# 函數應用程式
func-walsin-poc01
![image.png](/.attachments/image-e8794012-4aa2-43b3-887f-bfcaf1c09194.png)

## 網路設定
私人端點 + 公用存取限制 + 輸出流量整合
![image.png](/.attachments/image-86095b18-1e9c-4bfb-803b-734e196f1c3e.png)

- 存取限制: 
加上公司外網IP
加上AzureDevOps 服務來源
![image.png](/.attachments/image-aef44aba-acca-49e4-a714-5d4d75605d27.png)

- 私人端點 
![image.png](/.attachments/image-00404468-f0c6-4062-98c6-372f132882c3.png)

- 輸出流量整合
![image.png](/.attachments/image-4668b74a-da18-4ea6-87ec-707a09439041.png)


# 金鑰保存庫
名稱: akv-walsin-poc01

## 設定秘密: MoveiName
![image.png](/.attachments/image-da00b54a-cb11-4cf9-8e0d-b63ae9a016df.png)

## 啟用身分識別 系統指派
![image.png](/.attachments/image-b2dd4528-bd70-4de9-a2c9-881482cb15b8.png)

## 網路設定
- 允許來自特定虛擬網路和 IP 位址的公用存取
- 允許選取的虛擬網路安全且直接使用服務端點連線至您的資源
- 新增 IP 範圍，以允許來自網際網路或您內部部署網路的存取。
![image.png](/.attachments/image-ed3dc521-75e3-4565-868c-07608c545379.png)
![image.png](/.attachments/image-0db69ebb-9e1e-497c-bf2b-ef46be228136.png)
![image.png](/.attachments/image-396a5293-4421-4921-b235-426211f75596.png)

- 私人端點連線
![image.png](/.attachments/image-44e61beb-45e2-4a00-873a-489837386819.png)


# 儲存體帳戶
名稱: stblobpoc01
容器: databox
![image.png](/.attachments/image-de7b3e19-4013-4ea5-b85f-1297f9c117e7.png)

## 網路設定
- 已從選取的虛擬網路和 IP 位址啟用
- 允許選取的虛擬網路安全且直接使用服務端點連線至您的資源
- 新增 IP 範圍以允許來自網際網路或您內部部署網路的存取。
![image.png](/.attachments/image-1567ea9a-1cd3-47ba-b710-e1878e645f34.png)
![image.png](/.attachments/image-5ac4b675-92d5-425c-ba75-1a184bfa16a5.png)

# Private Link
檢查 Private Link 是否正確設定 & 啟用
![image.png](/.attachments/image-2ca2d0ba-07e9-4d0a-a794-9a9b64d68d37.png)

# 私人 DNS 區域
檢查 DNS 是否都有設定 記錄集 及 
![image.png](/.attachments/image-5514578e-3782-4b7d-a607-ec43a4412583.png)
![image.png](/.attachments/image-72dc4782-bbb7-45bf-a7e7-98949b7bf39b.png)
![image.png](/.attachments/image-df077558-8aab-4263-aee8-80463175ade8.png)

# 整合測試
1. Forms 提交表單
1. 發送 outlook
1. 建立日期變數
1. 呼叫 Azure Function 整合 Keyvault 取秘密
1. 呼叫 Azure Blob 寫紀錄
![image.png](/.attachments/image-31b9b186-f91b-4d82-850b-f194d37c5090.png)


# Private Link 中心 | Azreu 監視器 Privae Link 範圍
由於採用 VNET 輸出整合，因此監視服務要另外設定 Private Link Scooe (pls) 才能蒐集到 logs
## 建立Azure 監視器私人連結範圍
![image.png](/.attachments/image-a3c0c6c8-2042-4c3b-89a8-9e25c3bb78e9.png)

## 將 monitor 服務納進範圍中
![image.png](/.attachments/image-c24a269c-ca64-4854-b41c-be8327b8e767.png)

## 建立私人端點連線
![image.png](/.attachments/image-d7f8fe21-a0b8-4d6c-8e62-bd0151495ca0.png)

1. 輸入私人端點 基本資訊
名稱: pls-{服務名稱}-pep{流水號}
![image.png](/.attachments/image-017d89fc-a7c6-4d8b-8d48-48936d17d637.png)

1. 選擇資源
連線類型 microsoft.insights/privateLinkScopes
資源群組 rg-logicapps-poc01
資源 pls-logic-poc01
目標子資源 azuremonitor
![image.png](/.attachments/image-428d9005-b43c-48c1-b192-cf536b960472.png)

1. 設定動態IP
![image.png](/.attachments/image-c2e71df8-1188-44d8-b9de-4e72a69a4fa0.png)

1. DNS
 Private DNS 要在VNET資源群組中
![image.png](/.attachments/image-f54395b3-2078-4628-821d-588e05eaf96d.png)

## Private Link 中心 | 私人端點
檢查 Private Link 中心 | 私人端點
![image.png](/.attachments/image-57126928-3bb1-4335-9f37-bb11bb9d496b.png)

檢查 私人 DNS 區域
![image.png](/.attachments/image-5e357a68-7c4e-4cd9-ae68-f1f0a6a64e26.png)
