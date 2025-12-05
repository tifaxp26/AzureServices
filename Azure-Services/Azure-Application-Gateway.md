[[_TOC_]]


說明: 如何設定APGW

# 設定 接聽程式
**listener-{專案}-{環境}{-Private}**

接聽程式名稱: listener-l5-prod、listener-l5-prod-Private
- 前端 IP: 公用/私人
- 連接埠: 443
- 選擇憑證: 選取現有 / WalsinCert_2024
- 接聽程式類型: 多站台
- 主機類型: 單一
- 主機名稱: l5dip.walsin.com

![image.png](/.attachments/image-6c439b23-df03-435e-8768-b874aaa2825c.png)
![image.png](/.attachments/image-53022b86-4572-46b2-9b5c-2abb930f4fab.png)

# 設定 後端集區
**backendPool-{專案}-{環境}**

名稱: backendPool-l5-prod
新增不含目標的後端集區: 否
後端目標
 - 目標類型: IP 位址或 FQDN
 - 目標: 10.101.XXX.XXX
![image.png](/.attachments/image-269ec0e9-22af-42da-b2eb-9cb07e307342.png)


# 設定 後端設定
**backendSetting-{專案}-{環境}**

後端設定名稱: backendSetting-l5-prod
後端通訊協定: HTTP
後端連接埠: 80

其他設定
- 以 Cookie 為基礎的親和性: 停用
- 清空連線: 停用
- 要求逾時 (秒): 180
- 覆寫後端路徑: 無

主機名稱
- 以新的主機名稱覆寫: 是
- 主機名稱覆寫: 以特定網域名稱覆寫
- 主機名稱: l5dip.walsin.com
- 使用自訂探查: 是
- 自訂探查: HTTP_Prod

![image.png](/.attachments/image-188b767c-4e45-485d-a24a-9430d14cd334.png)
![image.png](/.attachments/image-c56fa3cd-eb30-4ba8-b350-055a3f31e41a.png)


# 設定 規則
**routingRule-{專案}-{環境}**

- 規則名稱: routingRule-l5-prod
- 優先順序: 
- 接聽程式: istener-l5-prod
- 後端目標: backendPool-l5-prod	
- 後端設定: backendSetting-l5-prod
	
# 設定 健康狀態探查
**probe-{專案}-{環境}**

名稱: probe-l5-prod
- 通訊協定: HTTP
- 從後端設定挑選主機名稱: 是
- 從後端設定挑選連接埠: 是
- 路徑: /
- 間隔 (秒): 30
- 逾時 (秒): 30
- 狀況不良閾值: 3
- 請使用符合條件的探查: 是
- HTTP 回應狀態碼相符: 200-399

![image.png](/.attachments/image-b2512530-3878-4f8c-9fd9-a091ddd1ea4f.png)
![image.png](/.attachments/image-f209b445-12bb-4803-9cf7-8e52888c354a.png)

---

#　應用程式閘道 - 【重寫功能】
說明:
服務入口: marketplace-dev.walsin.com
後端服務: app-walsin-dataplatform-marketplace-dev.azurewebsites.net
華新內部CAS: casdev.walsin.com/cas/login


登入成功後，
會由 CAS 轉回 marketplace-dev.walsin.com，
需要設定 重寫功能

![image.png](/.attachments/image-f5b83f57-5aed-451b-badf-0de1dfcc2d9e.png)

建立集合名稱
1. rewrite-set-marketplace-dev
1. 選擇規則項目
![image.png](/.attachments/image-fe7d8cca-7f38-4d30-b3c5-38233e94e268.png)

建立規則設定
![image.png](/.attachments/image-b8de6c9f-4573-4335-a56f-9c72fd0924e4.png)

1. 重寫規則名稱: LocationHeader
1. 要比對的模式
`(https?):\/\/.*azurewebsites.net(.*)$`
![image.png](/.attachments/image-76177772-d13a-4ed0-a797-37cb05c239b5.png)

1. 標頭值
`{http_resp_Location_1}://marketplace-dev.walsin.com{http_resp_Location_2}`
![image.png](/.attachments/image-d81901d2-ea02-4016-995a-7f7a0bd0be3e.png)


驗證
![image.png](/.attachments/image-5bf03147-1268-4c1d-8b7f-1a0f344d85db.png)

![image.png](/.attachments/image-ec296b46-552f-4b6c-9a62-87ba4c35bf8a.png)



# 設定 SSL 憑證
更新SSL憑證
![image.png](/.attachments/image-e1d4d808-297d-44ce-9779-756f0ba6d494.png)

新增設定
![image.png](/.attachments/image-f6a49a91-791b-4f29-b3ca-217b424e532e.png)

逐一更新
![image.png](/.attachments/image-e0596385-a642-4e4a-8a65-f02bdae3b723.png)



# 設定 SSL 憑證 - 整合 Azure Keyvault

## 必要條件
- Key Vault 祕密員: 指派給執行的人員帳號
- Key Vault 系統管理員: 指派給 使用者受控識別


#### Key Vault Azure 角色型存取控制權限模型
因為目前 尚未支援在 Azure Portal 操作，
需利用 script 進行憑證 綁定作業。

應用程式閘道支援透過角色型存取控制權限模型在 Key Vault 中參考憑證。 
參考 Key Vault 的最初幾個步驟必須透過 ARM 範本、Bicep、CLI 或 PowerShell 來完成。
 
```
注意
不支援透過入口網站來指定遵從角色型存取控制權限模型的 Azure Key Vault 憑證。
```


```
# Get the Application Gateway we want to modify
$appgw = Get-AzApplicationGateway -Name MyApplicationGateway -ResourceGroupName MyResourceGroup
# Specify the resource id to the user assigned managed identity - This can be found by going to the properties of the managed identity
Set-AzApplicationGatewayIdentity -ApplicationGateway $appgw -UserAssignedIdentityId "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/MyResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/MyManagedIdentity"
# Get the secret ID from Key Vault
$secret = Get-AzKeyVaultSecret -VaultName "MyKeyVault" -Name "CertificateName"
$secretId = $secret.Id.Replace($secret.Version, "") # Remove the secret version so Application Gateway uses the latest version in future syncs
# Specify the secret ID from Key Vault 
Add-AzApplicationGatewaySslCertificate -KeyVaultSecretId $secretId -ApplicationGateway $appgw -Name $secret.Name
# Commit the changes to the Application Gateway
Set-AzApplicationGateway -ApplicationGateway $appgw
```

執行命令之後，即可在 Azure 入口網站中瀏覽至應用程式閘道，然後選取 [接聽程式] 索引標籤。按一下 [新增接聽程式] (或選取現有的接聽程式)，並將 [通訊協定] 指定為 [HTTPS]。
在 [選擇憑證] 下，選取先前步驟中命名的憑證。 選取之後，選取 [新增] (如果是建立) 或 [儲存] (如果是編輯)，將參考的 Key Vault 憑證套用至接聽程式。


在此範例中，我們使用 PowerShell 來參考新的 Key Vault 祕密。


## Step1. 建置 受控識別
類型: 使用者受控識別
![image.png](/.attachments/image-5f1ca3cd-33d8-4d68-8b09-745871a42bab.png)

## Step2. 先把SSL 放在 Keyvault 憑證中
![image.png](/.attachments/image-8c098f5e-140a-4895-8bec-2bd8ccf27d10.png)

## Step3. Keyvault 綁定 受控識別
**IAM: Key Vault 系統管理員**
![image.png](/.attachments/image-66877f41-28b2-4c1a-b770-fd0194c072cc.png)

## Step4. 執行 script


## APGW 接聽程式 TLS 憑證
設定成功後，會出現綁定的憑證資訊
![image.png](/.attachments/image-8139e9e3-8f11-4370-9437-f2ac898420a3.png)


## 接聽程式 更換憑證
![image.png](/.attachments/image-41ae13e2-0213-4c04-b1e8-5b69058768ef.png)

## 完成

[Azure Key Vault 憑證的 TLS 終止 | Microsoft Learn](https://learn.microsoft.com/zh-tw/azure/application-gateway/key-vault-certs?WT.mc_id=Portal-Microsoft_Azure_HybridNetworking#key-vault-azure-role-based-access-control-permission-model)




