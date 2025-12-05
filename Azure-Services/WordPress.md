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


# 部署
![image.png](/.attachments/image-7a2d12e7-18be-45f6-b09e-a57af0192d63.png)

# 部署完成
![image.png](/.attachments/image-ac2199ec-0e4d-4d35-b4b7-51b11ce2abab.png)

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


# App Service

## 環境變數
- WP_EMAIL_CONNECTION_STRING
![image.png](/.attachments/image-73b9b251-fa18-44e3-9235-084c4fbce673.png)

# Key Vault
## 秘密
- email-connection-string
- storage-account-key
- wordpress-admin-password

![image.png](/.attachments/image-407e1e32-90a1-4802-9c1e-23934b9a78bd.png)


---
# 參考

- https://learn.microsoft.com/zh-tw/azure/app-service/overview-wordpress
- https://learn.microsoft.com/zh-tw/azure/active-directory-b2c/configure-authentication-in-azure-web-app#step-21-register-the-app
- https://learn.microsoft.com/zh-tw/azure/active-directory-b2c/openid-connect#validate-the-id-token
- https://learn.microsoft.com/zh-tw/azure/app-service/configure-authentication-provider-openid-connect


