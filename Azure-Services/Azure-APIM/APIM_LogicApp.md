[[_TOC_]]


# 目的
1. 使用者進到單一入口 APGW 
1. 再到 API 管理中心 APIM 
1. 轉服務到 Loggic App URL

![image.png](/.attachments/image-3847e377-f30f-4770-9e0f-18d0ec51cc0a.png)


# 設定 APGW
由於 APIM 預設站台是沒有畫面的，會出現404錯誤，
因此 APGW 要排除404錯誤
## 建立健康狀態探針
命名: probes-{專案名稱}
![image.png](/.attachments/image-fd17962f-dceb-48e0-a258-74d8e8b8720f.png)

## 套用到後端設定
命名: AZ-APGWP02-Backend-{專案名稱}
![image.png](/.attachments/image-15f19811-13bc-490b-8f99-b78d8c64a80d.png)



建議使用 Proxy Status (Health Probe)
- Proxy Status (Health Probe): 
```
https://apim-walsin-dataplatform.azure-api.net/status-0123456789abcdef
```

- Developer Portal Status (Health Probe): 
```
https://apim-walsin-dataplatform.developer.azure-api.net/internal-status-0123456789abcdef
```

- Management Portal (Health Probe) : 
```
https://apim-walsin-dataplatform.management.azure-api.net/servicestatus
```

- https://apim-walsin-dataplatform.management.azure-api.net/servicestatus
```
{
    "version": "0.42.17434.0",
    "roleInstance": "gwhost_1",
    "skuType": "Standard",
    "region": "Southeast Asia",
    "sdpStage": "Stage_6",
    "zone": "   "
}
```




# 設定 APIM
## 建立 logic app api
- 類型名稱: logicapps
![image.png](/.attachments/image-ca40329d-5725-479e-a485-bdf27cff54b1.png)

- 建立方法名稱: TeamsNotification
- 建立方法類型: POST
- 金鑰驗證: 啟用
![image.png](/.attachments/image-7003eafe-0b35-4b6c-a87c-a694e641b714.png)
![image.png](/.attachments/image-3b1cf4e9-c739-4f69-bd0d-cd53e280de9e.png)

- 設定功能中，指定 logic app
![image.png](/.attachments/image-7d2f8c25-2125-4553-852b-24e0a5c202b4.png)

- 調整Policy
移除設定 Ocp-Apim-Subscription-Key 
![image.png](/.attachments/image-9ec289b7-e686-47fb-abbe-a7956a14ade8.png)

# 測試
- 方法: POST
- URL: https://apim-walsin-dataplatform.azure-api.net/logicapps/TeamsNotification


```
{   
    "chatType": "oneOnOne",
    "topic": "1:1聊天室",
    "poster": "User",
    "location": "Group chat",
    "members": "Tifa_Chen@walsin.com",
    "body": {
        "contentType": "html",
        "content": "【系統共用帳號】<br/><p><b>1:1 訊息發送測試...</b> 第27則</p>"
    }
}
```
![image.png](/.attachments/image-d5b38a56-ecf4-4f94-9528-74984cc6e809.png)
![image.png](/.attachments/image-d25b5632-5cbc-4591-bc00-758f9372b1b7.png)

