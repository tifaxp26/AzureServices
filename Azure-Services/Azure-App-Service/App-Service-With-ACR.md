[[_TOC_]]

# 說明:
- App Service 使用 Private Link
- App Service 使用 Private Network Integation
- Azure Container Registy 使用 Private Link

![image.png](/.attachments/image-4d4f08c4-f4a1-495a-940d-ea19f65cb84f.png)
![image.png](/.attachments/image-e4b518b4-afb2-433a-92e1-3e44ed3c8319.png)
![image.png](/.attachments/image-6178534e-615b-40c4-aca9-2258b62cb22a.png)
![image.png](/.attachments/image-cd4ada1e-d37b-4de1-ab8d-cd73e9e12658.png)

因此需要使用 DevOps Hosted Agent 協助 docker build 及 image Push
![image.png](/.attachments/image-0223c633-beca-4102-9e98-5079f50f9eaf.png)




# 如何同步ACR image 更新事件

## App Service 部屬中心 
1. 選擇目的端 ARC

1. 啟用 連續部屬
![image.png](/.attachments/image-b08f7095-7b1c-407a-a56a-9fb0061fcc89.png)

1. 組態
啟用 SCM基本驗證
![image.png](/.attachments/image-f4e15bed-32f3-4f22-ac0a-07f095140e7b.png)

## Container Registry
1. ACR 服務
系統會自動建立 Webhook 勾稽通知事件
![image.png](/.attachments/image-55f3373f-aba0-48b6-b9ee-adba151093ef.png)

1. 由於ACR 存取金鑰 關閉 管理使用者
因此要去建 存放庫權限
1. 建立 範圍對應，一般 arcPull = Content Read
1. 建立 權杖
1. 產生 Token
以此身分進行存取
![image.png](/.attachments/image-15c560e1-3661-4bdb-9a5e-5c77b9be15c9.png)
![image.png](/.attachments/image-3a78ef9c-dbdf-4624-be36-d0a5f26a1d0a.png)
![image.png](/.attachments/image-331bd1e0-f5bc-4551-abf5-2267fb49fb77.png)


# 虛擬網路整合 問題

錯誤訊息

```
2025-02-10T01:11:32.303Z ERROR - Image pull for acrwalsind01.azurecr.io/aaa-walsin:20250208.15 failed. UnexpectedFaliure
2025-02-10T01:11:32.304Z ERROR - Pulling docker image acrwalsind01.azurecr.io/aaa-walsin:20250208.15 over VNET failed.
2025-02-10T01:11:32.306Z WARN  - Image pull failed. Defaulting to local copy if present.
2025-02-10T01:11:32.309Z ERROR - Image pull failed: Verify docker image configuration and credentials (if using private repository)
```



當 App Service 使用 虛擬網路整合，
是有限制 Docker Image MediaType 格式，
mediaType 必須是 `application/vnd.docker.distribution.manifest.v2+json`
![image.png](/.attachments/image-3a6623eb-8fa4-4e4a-86f4-1bb61c5688d1.png)

而暫不支援新式格式 
`application/vnd.oci.image.manifest.v1+json`
![image.png](/.attachments/image-3566b356-6972-416b-93f2-f2e6a9f66844.png)

解法:
由於新版 Docker DeskTop 會自動啟用 Containerd 功能
![image.png](/.attachments/image-61a1541b-a86f-4399-90e5-7ef3a9a53745.png)
要關閉此功能
![image.png](/.attachments/image-cdda2c16-a999-413d-9aea-3ea282f3cf21.png)


什麼是 Containerd:
![image.png](/.attachments/image-761e1ae3-cf58-4f98-944f-66a1f5ed4352.png)
![image.png](/.attachments/image-d086842e-5d00-4604-b83e-4d17cf31e582.png)


## 參考討論
- [Force Image to Use Manifest Media Type Docker V2, schema 2 Instead of OCI](https://forums.docker.com/t/force-image-to-use-manifest-media-type-docker-v2-schema-2-instead-of-oci/144974)


- [無法將映像從 Azure Container Registry 提取至 Azure Web 應用程式](https://learn.microsoft.com/zh-tw/troubleshoot/azure/azure-container-registry/pull-image-to-web-app-fail)

- [Troubleshooting Guide: Resolving Azure App Services Image Pull Issues from Azure Container Registry](https://techcommunity.microsoft.com/blog/appsonazureblog/troubleshooting-guide-resolving-azure-app-services-image-pull-issues-from-azure-/4010201)



