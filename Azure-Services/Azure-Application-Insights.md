[[_TOC_]]

# 說明
符合CAF 規範，
服務設定Private Endpoint後，
其 Application Insights 都要另外設定 private endpoint 及 private DNS !!
![image.png](/.attachments/image-9f499363-3b83-4798-9e7f-a372ec918915.png)


# 開啟 Private Link 中心 | Azreu 監視器 Privae Link 範圍
![image.png](/.attachments/image-025e29f1-3283-4557-a4e0-851811c5b88b.png)

# 新增 Private Link 範圍
![image.png](/.attachments/image-c4fc5ac9-f284-4652-952b-527cf752bc5e.png)

# 選取 服務
![image.png](/.attachments/image-219016d7-4d85-4d9f-b74e-018435a21d9a.png)
![image.png](/.attachments/image-27bf6e83-0687-43fc-90d4-58e0911b72c0.png)

# 建立私人端點
![image.png](/.attachments/image-88350644-8816-40c6-94e3-3393b722ce6b.png)

- 基本
名稱: **appi-{服務名稱}-pep{流水號}**
![image.png](/.attachments/image-371d449f-2c5a-4990-8828-510c2d9d350f.png)

- 資源
資源類型: Microsoft.Insights/privateLinkScope
![image.png](/.attachments/image-7b74ab58-f68a-45b2-a39a-371329054f50.png)

- 虛擬網路
![image.png](/.attachments/image-ea1b6d5c-9271-4a64-809e-72e8f3cb62eb.png)

- DNS
記得選 "否"
![image.png](/.attachments/image-a5ff329a-6f7b-47b0-876a-10eb49b86e42.png)
(預設如下)
![image.png](/.attachments/image-a679d234-7cc2-4e10-b8a6-4ff74d7f5c7e.png)

完成，將以下資訊 註冊到 Private DNS
![image.png](/.attachments/image-d033da95-1301-4143-83b2-5c8552d09c74.png)
![image.png](/.attachments/image-06cfa20b-840f-4638-b8f7-f8220b3cfa9e.png)


# 為 Application Insights Profiler 和快照偵錯工具設定 BYOS
當您使用 Application Insights Profiler 或快照偵錯工具時，應用程式所產生的成品會透過公用網際網路，預設上傳至 Azure 儲存體帳戶。 針對這些成品和儲存體帳戶，Microsoft 會控制及承擔下列費用：

- 處理和分析。
- 待用加密和存留期管理原則。

同時，當您設定自備儲存體帳戶 (BYOS) 時，成品會上傳至僅由您控制及支付以下費用的儲存體帳戶：

- 待用加密原則和存留期管理原則。
- 網路存取：

## 注意: 如果您要啟用 Azure Private Link 或客戶自控金鑰，則需要 BYOS。

- 深入了解適用於 Application Insights 的 Private Link。
- 深入了解適用於 Application Insights 的客戶自控金鑰。

在本指南中，您將了解如何：

將診斷服務的存取權授與儲存體帳戶。
連結儲存體帳戶和 Application Insights 資源。
了解儲存體帳戶的存取方式。

必要條件
確認您已在與 Application Insights 資源相同的位置建立儲存體帳戶。
如果您已啟用 Private Link，請允許虛擬網路連線到信任的 Microsoft 服務。
將診斷服務的存取權授與儲存體帳戶
BYOS 儲存體帳戶會連結到 Application Insights 資源。 
首先，透過儲存體帳戶中存取控制 (IAM) 頁面，
將 Storage Blob Data Contributor 角色授與名為 Diagnostic Services Trusted Storage Access 的 Microsoft Entra 應用程式。


指派下列角色:
設定	         值
角色	         儲存體 Blob 資料參與者
存取權指派對象	 使用者、群組或服務主體
成員	         診斷服務信任的儲存體存取

![image.png](/.attachments/image-481bf7a1-8aa4-4350-9a33-6dc793d84d10.png)
![image.png](/.attachments/image-9c0f5837-51a7-479f-b8a4-9861579bfb0c.png)


## 注意
如果您也使用 Private Link，則需要一個額外的設定，以允許從虛擬網路連線到信任的 Microsoft 服務。 如需詳細資訊，請參閱儲存體網路安全性文件。

# 連結儲存體帳戶和 Application Insights 資源
您有三個選項可用來設定 BYOS，以進行程式碼層級診斷，例如 Profiler 和快照偵錯工具：

Azure CLI


```
az login --tenant 97876bed-bb9a-4617-802b-94c62e7837b5
az account set --subscription 2ba82b79-5aa9-4145-bfde-7323c8c2fa72
```


1. 安裝 Application Insights CLI 延伸模組。
`az extension add -n application-insights`

1. 使用 Application Insights 資源連線至儲存體帳戶。
`az monitor app-insights component linked-storage link --resource-group "rg-dataplatform-prod-01" --app "app-walsin-DataPlatform" --storage-account "stbyosdpprod01"`

預期輸出：

```
{
  "id": "/subscriptions/{subscription}/resourcegroups/byos-test/providers/microsoft.insights/components/byos-test-westus2-ai/linkedstorageaccounts/serviceprofiler",
  "linkedStorageAccount": "/subscriptions/{subscription}/resourceGroups/byos-test/providers/Microsoft.Storage/storageAccounts/byosteststoragewestus2",
  "name": "serviceprofiler",
  "resourceGroup": "byos-test",
  "type": "microsoft.insights/components/linkedstorageaccounts"
}
```

![image.png](/.attachments/image-5a87face-5f7c-4c43-b079-58b07b569ba9.png)





