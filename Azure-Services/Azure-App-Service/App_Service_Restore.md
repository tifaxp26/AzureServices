[[_TOC_]]

如何還原Azure App Serivce

確認以下:
- 訂閱帳戶
- 資源群組
- 地區
- 服務 SKU

範例:

```
訂用帳戶: WALSIN-Azure-Dataplatform-Dev
訂用帳戶 ID: 92bf3733-81dc-4e62-a709-978b569d16db
資源群組: rg-dataplatform-dev
App Service 方案: ASP-dataplatform-marketplace-dev (B1: 1)
App Service: app-walsin-dataplatform-marketplace-api-dev
```


使用 Azure CLI Powershell

```
az login --tenant 97876bed-bb9a-4617-802b-94c62e7837b5
Set-AzContext -SubscriptionId 92bf3733-81dc-4e62-a709-978b569d16db
```
確認被刪除的服務
`Get-AzDeletedWebApp -Name app-walsin-dataplatform-marketplace-api-dev -Location Southeastasia`

還原服務
`Restore-AzDeletedWebApp -Location Southeastasia -ResourceGroupName rg-dataplatform-dev -TargetResourceGroupName rg-dataplatform-dev -Name app-walsin-dataplatform-marketplace-api-dev -TargetAppServicePlanName ASP-dataplatform-marketplace-dev`


完成。