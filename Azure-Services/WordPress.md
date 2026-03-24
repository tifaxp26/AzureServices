[[_TOC_]]

說明
【在 App Service 上建立 WordPress】
主控方案: Standard PremiumV3 App Service, Burstable MySQL database

# 環境建置
網站語言: 繁體中文
系統管理員電子郵件: sysadmin@walsin.com
系統管理員使用者名稱: sysadmin
系統管理員密碼: 1qaz@WSX3edc

遠端虛擬網路摘要
  AZ-VNETCONNP01-to-VNETTESTD02
本地遠端虛擬網路摘要
  AZ-VNETTESTD02-to-VNETCONNP01

DNS forwarding ruleset
  vlink-AZ-VNETTESTD02

內部DNS forwarding
   .southeastasia-01.azurewebsites.net

虛擬網路
  虛擬網路在 Azure 中會以邏輯方式與彼此隔離。
  您可以設定其 IP 位址範圍、子網路、路由表、閘道器以及安全性設定，
  非常類似您資料中心內的傳統網路。
  根據預設，位於相同虛擬網路中的虛擬機器可互相存取。 
虛擬網路: AZ-VNETTESTD02 (rg-wordpress)
Web 應用程式子網路: AZ-SUBNET151-0
資料庫子網路: AZ-SUBNET151-32
KeyVault Subnet: AZ-SUBNET151-64

# 新增 WordPress on App Service 
![image.png](/.attachments/image-2d1dcd10-84c8-4fe9-9b00-0a295d8be7d2.png)

## 基本
![image.png](/.attachments/image-19ea0763-e367-4917-b70e-187bd43cb7e4.png)
![image.png](/.attachments/image-d7c45271-985e-4b40-bf3a-97336203a5ba.png)

### 方案
![image.png](/.attachments/image-017d921d-612e-4052-a426-63285cb0be43.png)
![image.png](/.attachments/image-04ec1f23-025f-4c32-b025-4dbc10c7c317.png)

## 增益集
- Front Door 不勾選，後面再設定
- 建議使用預設Storage Account

![image.png](/.attachments/image-381f9d56-4b7a-487d-8c38-bfabe2c7be6b.png)

## 網路
![image.png](/.attachments/image-b6853568-d606-4804-bbc4-bc9b23d66dff.png)

## 部署
![image.png](/.attachments/image-250d26f5-f984-474e-9fc6-1f64061cbf35.png)

---

# 部署完成
![image.png](/.attachments/image-fb1075c8-2ad8-4442-8074-e28e73fe12da.png)
![image.png](/.attachments/image-f522d6a1-8229-4af9-be95-fe4b0a9560f7.png)
![image.png](/.attachments/image-6283ff30-d6db-44bc-b440-518f32317859.png)

---

# 首頁，初始畫建置中
![image.png](/.attachments/image-d477116a-d9b5-4ed2-ad25-aed1fa3e7d27.png)

# 後台管理
若要存取 WordPress 管理頁面，
請前往 **/<app-service-name>/wp-admin** 
並使用您在步驟三中建立的認證登入。 

![image.png](/.attachments/image-d51aa226-ae7e-4753-a7ed-89c3a04690bc.png)


# 整合AAD
使用者整合 AAD，進入 Portal
![image.png](/.attachments/image-a781c9ca-793d-4d3d-9ff1-70a8ee02a337.png)

## 整合 AAD B2C
- 識別提供者: 選 OpenID Connect
- OpenID 提供者名稱: walsinb2ct01
- 中繼資料 URL
https://walsinb2ct01.b2clogin.com/walsinb2ct01.onmicrosoft.com/B2C_1_signin/v2.0/.well-known/openid-configuration
- 用戶端識別碼: 79d62895-9344-4db2-a40f-096e914a4ac7
- 用戶端密碼

![image.png](/.attachments/image-928e71ba-a940-4f67-b4fa-49caf88ca127.png)
![image.png](/.attachments/image-ba4e38ac-4f44-4d3f-aefe-883f4754a630.png)

套用
![image.png](/.attachments/image-ebcaaf01-768e-403c-abce-cc75af6dbd8f.png)

注意
B2C AAD 目錄的 App Registration 要調整驗證URL
- Web 導向: 
**https://walsinwp-auhmcecxdgfmchgv.southeastasia-01.azurewebsites.net/.auth/login/aadb2c/callback**

![image.png](/.attachments/image-0417cef3-79c6-4070-896c-ea92f3ac04e2.png)



# 系統後台
## 設定管理員
![image.png](/.attachments/image-8305dffd-bdfb-4372-b112-d1172e1b72ab.png)

## 安裝外掛
- Health Endpoint
![image.png](/.attachments/image-00e88722-305b-4480-9b31-91fba143b80a.png)


# 整合 Communication Service
- 名稱: acs-notification-asia

## 授權IAM
- 賦予角色: Communication and Email Service Owner
- 對象: 使用者受控識別
![image.png](/.attachments/image-5dbfc8e7-a22a-4799-97b5-b4efdfaca5a8.png)

## App Setting
修改環境變數
名稱: WP_EMAIL_CONNECTION_STRING
內容: endpoint=`<endpoint>`;senderaddress=`<sender-address>`;accesskey=`<access-key>`

- WP_EMAIL_CONNECTION_STRING
![image.png](/.attachments/image-b4a74d18-aeff-44fb-ba4c-b2eab88ffda1.png)


使用變更密碼進行發信測試
![image.png](/.attachments/image-20209cf3-c034-4680-9466-83b94913b0aa.png)



# 使用者受控識別
授權
- Key Vault 祕密使用者
- Custom Email Contributor Role
- 儲存體 Blob 資料參與者

資源
- App Service
- MySQL

![image.png](/.attachments/image-b666431c-714b-4358-bade-0b3c441829d6.png)
![image.png](/.attachments/image-f5e3d6a7-d3a9-4428-9a8d-7e34e521a875.png)


# 整合 Key Vault
環境變數 > 提取參考值
![image.png](/.attachments/image-9c97f713-7845-4873-b1c0-b205244af3c9.png)



## 秘密
**內容格式: @Microsoft.KeyVault(SecretUri=secret_uri_with_version)**

- email-connection-string
- storage-account-key
- wordpress-admin-password

![image.png](/.attachments/image-407e1e32-90a1-4802-9c1e-23934b9a78bd.png)
![image.png](/.attachments/image-73b9b251-fa18-44e3-9235-084c4fbce673.png)

# Azure Application Gateway 相關設定
## 重寫
if 
- 要檢查的變數類型: HTTP 標頭
- 標頭類型: 回應標頭
- 標頭名稱: 一般標頭
- 一般標頭: Location
- 要比對的標頭值模式: (https?):\/\/.*azurewebsites.net(.*)$
- Operator 等於 (=)

then
- 重寫類型: 回應標頭
- 動作類型: 設定
- 標頭名稱: 一般標頭
- 一般標頭: Location
將標頭值設定為: {http_resp_Location_1}://steeval.walsin.com{http_resp_Location_2}

![image.png](/.attachments/image-cd3589a4-1c6f-4f88-924e-306b68c9f2dc.png)
![image.png](/.attachments/image-0025bd06-ebcd-4f1f-bbf0-4bc9b18925f3.png)


# 自訂域名
## 說明
根據 WordPress 環境設定檔，系統會抓 HTTP_HOST 這個 Headers 參數
![image.png](/.attachments/image-8b4b8f32-1c1e-462e-a56f-1b9a0d7d8c92.png)

## 先註冊DNS
![image.png](/.attachments/image-446f6fa5-28de-4b12-92f5-588c8b30c70e.png)

## App Service 自訂域名
![image.png](/.attachments/image-2e8dd23b-d5e5-461f-ab49-4614447ca1f4.png)

## 調整作法一 
Application Gateway
後端設定 > 主機名稱覆寫 > 以特定網域名稱覆寫
![image.png](/.attachments/image-1903190a-fad8-4ea1-b6a5-5931cb83ec77.png)

## 調整作法二 (推薦)
下載 App Service WordPress 環境設定檔
![image.png](/.attachments/image-40f89dfa-f793-444c-8ad3-f49434fa4bc9.png)

修改以下資訊

```
define('WP_HOME', 'https://steeval.walsin.com');
define('WP_SITEURL', 'https://steeval.walsin.com');
define('DOMAIN_CURRENT_SITE', 'https://steeval.walsin.com');
```

![image.png](/.attachments/image-56fe241f-f399-440d-994e-3e89d0b56914.png)

驗證 前後端
![image.png](/.attachments/image-d77adb38-e8fb-4c7e-a4f2-e6c4a993f584.png)
![image.png](/.attachments/image-f89ef598-7007-436c-8c0c-51b0e1c9b1b1.png)


---
# 參考

- https://learn.microsoft.com/zh-tw/azure/app-service/overview-wordpress
- https://learn.microsoft.com/zh-tw/azure/active-directory-b2c/configure-authentication-in-azure-web-app#step-21-register-the-app
- https://learn.microsoft.com/zh-tw/azure/active-directory-b2c/openid-connect#validate-the-id-token
- https://learn.microsoft.com/zh-tw/azure/app-service/configure-authentication-provider-openid-connect


