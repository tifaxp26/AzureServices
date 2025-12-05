[[_TOC_]]

# 分頁機制

## API Request 公版範例

```
{
    "header": {
        "email": "user@walsin.com",
        "dept": "HA110000TP",
        "userNo": "urxxxxx"
    },
    "pageInfo": {
        "page": 1,
        "pageSize": 5000
    },
    "payload": {
         ....
    }
}
```

## API 後端處理規範
1. api/Controller 收到請求後，依據商業用途建立服務入口
(以Item1為例)
![image.png](/.attachments/image-917fb30d-521e-48f3-9c38-dc79a97d732f.png)

1. 依據需求，做分頁處理
![image.png](/.attachments/image-4861d34a-62bb-4c70-8dcd-fe88fc44765f.png)

1. 回傳結果 會有
   - 此請求條件下的總筆數
   - 此請求條件下的資料結果
![image.png](/.attachments/image-fdc3e4b5-fa57-479d-a816-49f946e461fb.png)

## Client端 
收到response後，依據 總筆數total 並計算頁數，再次呼叫API取下頁資料。
