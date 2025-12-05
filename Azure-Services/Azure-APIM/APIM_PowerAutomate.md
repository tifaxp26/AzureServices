[[_TOC_]]


目的
使用者進到單一入口 APGW
再到 API 管理中心 APIM
轉服務到 Power Automate HTTP Request URL
![image.png](/.attachments/image-8b74d89f-1300-4164-85e5-1d1505e02ff8.png)

# 設定 APIM
## 建立 powerautomate api
## 類型名稱: powerautomate
![image.png](/.attachments/image-5a4a27dd-82ed-4395-a845-e62318cea746.png)

## 建立方法名稱: YSTeamsNotification

## 建立方法類型: POST

## 金鑰驗證: 啟用
![image.png](/.attachments/image-d2c3b6b4-47de-4ec7-b70d-54d499a04169.png)

## 設定 Web service URL: 
https://prod-31.southeastasia.logic.azure.com:443/workflows/424cec4aa5a2458c8b286633733455ad/triggers/manual/paths

## 設定 Inbound processing Rewrite URL
![image.png](/.attachments/image-39498223-8e91-4592-890b-4eab8e5a9d80.png)
![image.png](/.attachments/image-b41d05fe-7db4-456c-b37c-170525f275a8.png)


# 設定 APIM 帳號、產品、訂閱
## 新增系統用帳戶 App YS_PowerAutomate
![image.png](/.attachments/image-e833eb17-3391-47e0-a0f7-aefbb12cf166.png)

## 新增產品
名稱: WalsinPowerAutomate
![image.png](/.attachments/image-c1467de4-26e9-4d2f-ac9f-cd0dc4da72f0.png)
![image.png](/.attachments/image-eec6a188-9e5c-4b72-9eaf-02d2117b1075.png)

## 加入訂閱
取得金鑰
![image.png](/.attachments/image-a81b72a1-0536-4eed-aea8-ab025aa72799.png)


# 測試 Postman
方法: POST
URL: https://apim-walsin-dataplatform.azure-api.net/powerautomate/YSTeamsNotification
![image.png](/.attachments/image-ffd04c8f-cb43-40d4-a62e-1048b66f64c2.png)

```
{
    "Authorization": "ZWM1OWMyMWQtMWMxMC00NDIwLWI5NDEtZjhjYzA3Y2Q4NzVl",
    "hostUrl": "http://local.walsin.com:85/comm/api/updatePub231Mst02TeamsFlag",
    "pushList": [
        {
            "serialNo": "00114145",
            "subject1": "精整_異常訊息",
            "subject2": "原物料庫存：B45_1、B46、B46_3、B47_1、B47_2、B49、B48、B60_1、B60_4",
            "content": "Tifa 來自APIM 測試 第 16則",
            "contentType": "text",
            "dateCreate": "2024-04-08 17:07:00.000",
            "mailList": [
                {
                    "EMP_ID": "ur07579",
                    "E_MAIL": "ahsin_lin@walsin.com"
                },
                {
                    "EMP_ID": "陳文政",
                    "E_MAIL": "tifa_chen@walsin.com"
                }
            ]
        }
    ]
}
```

![image.png](/.attachments/image-afb447de-bdb9-47b0-9ba3-9e2e71d48891.png)
![image.png](/.attachments/image-fc8619ea-29a6-42e2-93d7-fad1d5f3f6c9.png)