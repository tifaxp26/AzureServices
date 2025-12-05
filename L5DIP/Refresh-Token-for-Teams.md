[[_TOC_]]

使用 .NET Core C# 實作 OpenID Connection 微軟提供的範本，
進行帳號登入並取得 應用程式註冊 授權，
再使用 postman 取得 Refresh Token 資訊。


# 設定 應用程式註冊
名稱: Portals-walsinAppsPortal
ClientID: f7ccc651-d8b2-4a76-8cc1-575dd3abf230 
![image.png](/.attachments/image-8e52816f-035a-4893-91bf-564273ef02b7.png)

# 設定 Scope 
要有 
- OpenIdConnectScope.OpenIdProfile
- OpenIdConnectScope.OfflineAccess
- Chat.Create Chat.Read Chat.ReadBasic Chat.ReadWrite
![image.png](/.attachments/image-fd7965bf-a37a-4a8e-b5f5-77e3b9eae1ac.png)

# 登入
使用帳號: IT_DIP_Agent@walsin.com 
![image.png](/.attachments/image-859e08f2-571e-4a8a-bdb2-6084561b4a81.png)

# 取得授權碼 authorizationCode
![image.png](/.attachments/image-0cf07b33-9a86-4a07-87bf-52a775b1a467.png)

# 使用Postman 取得 Refresh Token
![image.png](/.attachments/image-a111b30b-1474-402e-912f-ad31fd7b8383.png)

![image.png](/.attachments/image-1ad6555a-684d-4175-a176-09f0ab4f119b.png)

# 更新Keyvault 秘密
開發環境金鑰庫: az-keyvault-walsin-l5d01
正式環境金鑰庫: az-keyvault-walsin-l5p01
秘密名稱: BotRefreshToken
![image.png](/.attachments/image-6c71929c-8865-4119-a841-c51309420650.png)
![image.png](/.attachments/image-438f002a-0199-40c1-9f95-9e631e9287d4.png)