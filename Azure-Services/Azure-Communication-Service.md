[[_TOC_]]

# 說明

跨裝置和平台，接觸更多客戶
-------------
Azure 通訊服務提供多頻道通訊 API，可新增語音、視訊、聊天、文字簡訊/手機簡訊、電子郵件等內容至您所有的應用程式。

# Portal Communication Service

![image.png](/.attachments/image-2f35faaf-f0a4-4ee0-9ff7-396dfcb4a770.png)

# 建立自訂網域
從既有的服務 > 新增自訂網域
azmailbox-asia
--------------
![image.png](/.attachments/image-126a4e20-86eb-4cbf-892d-c7d2f4a2b7c0.png)

![image.png](/.attachments/image-5711f45b-f7fe-42a4-963b-50c5c1b05672.png)

# 註冊 Domin, SPF, DKIM, DKIM2 等資訊
![image.png](/.attachments/image-184cb571-5241-4310-81bc-24915638e35e.png)

# Azure DNS Zone 註冊
新增一組 域名 az-notify.walsin.com
![image.png](/.attachments/image-b35cb567-3070-4c3b-8618-67d071132d10.png)

註冊 SPF, DKIM, DKIM2 
![image.png](/.attachments/image-7927a629-b5e9-4a63-842c-49a50c744c9c.png)


# 加入域名
![image.png](/.attachments/image-bbee45f5-d225-461b-8b88-799f876c3bb1.png)
![image.png](/.attachments/image-56d18fe7-1729-46dd-bec4-80d07bb386eb.png)

# Mail From 新增
![image.png](/.attachments/image-226a286e-2ca0-469d-af55-9b15e6c692ac.png)

# Email 測試
![image.png](/.attachments/image-5767e02f-bd3c-4685-97e6-5db6d896a94f.png)


---

# 將角色指派給 Microsoft Entra 應用程式
---------------------------
https://learn.microsoft.com/zh-tw/azure/communication-services/quickstarts/email/send-email-smtp/smtp-authentication?tabs=built-in-role



---
前置作業:

您需要一個具有有效訂閱的 Azure 帳戶。若尚未建立，您可以免費建立帳戶：[建立 Azure 帳戶](https://apc01.safelinks.protection.outlook.com/?url=https%3A%2F%2Flearn.microsoft.com%2Fzh-tw%2Fazure%2Fcost-management-billing%2Fmanage%2Fcreate-enterprise-subscription&data=05%7C02%7CTifa_Chen%40walsin.com%7Cc227936ea6fc4246b87108ddc5d5f0bc%7C97876bedbb9a4617802b94c62e7837b5%7C1%7C0%7C638884244842320939%7CUnknown%7CTWFpbGZsb3d8eyJFbXB0eU1hcGkiOnRydWUsIlYiOiIwLjAuMDAwMCIsIlAiOiJXaW4zMiIsIkFOIjoiTWFpbCIsIldUIjoyfQ%3D%3D%7C0%7C%7C%7C&sdata=L2UG1Q%2B2dl7ekFPgu66Wvf7nQZzubqDfGzFQ38ZUhAs%3D&reserved=0 "原始的 URL: https://learn.microsoft.com/zh-tw/azure/cost-management-billing/manage/create-enterprise-subscription。按一下或點選 [如果您信任此連結。")

* * *

步驟一：建立 Communication Services 資源

*   請參考快速入門教學：
    
    [快速入門 - 建立並管理 Azure Communication Services 資源](https://apc01.safelinks.protection.outlook.com/?url=https%3A%2F%2Flearn.microsoft.com%2Fzh-tw%2Fazure%2Fcommunication-services%2Fquickstarts%2Fcreate-communication-resource&data=05%7C02%7CTifa_Chen%40walsin.com%7Cc227936ea6fc4246b87108ddc5d5f0bc%7C97876bedbb9a4617802b94c62e7837b5%7C1%7C0%7C638884244842370193%7CUnknown%7CTWFpbGZsb3d8eyJFbXB0eU1hcGkiOnRydWUsIlYiOiIwLjAuMDAwMCIsIlAiOiJXaW4zMiIsIkFOIjoiTWFpbCIsIldUIjoyfQ%3D%3D%7C0%7C%7C%7C&sdata=edt%2FctmYqcT8WyU8PJ2eX0Wpo6DqGFUlBYzkd%2F%2BYemU%3D&reserved=0 "原始的 URL: https://learn.microsoft.com/zh-tw/azure/communication-services/quickstarts/create-communication-resource。按一下或點選 [如果您信任此連結。")
    

* * *

步驟二：建立 Email Communication Service

*   請參考快速入門教學：
    
    [快速入門 - 建立並管理 Email Communication Service 資源](https://apc01.safelinks.protection.outlook.com/?url=https%3A%2F%2Flearn.microsoft.com%2Fzh-tw%2Fazure%2Fcommunication-services%2Fquickstarts%2Femail%2Fcreate-email-communication-resource&data=05%7C02%7CTifa_Chen%40walsin.com%7Cc227936ea6fc4246b87108ddc5d5f0bc%7C97876bedbb9a4617802b94c62e7837b5%7C1%7C0%7C638884244842389166%7CUnknown%7CTWFpbGZsb3d8eyJFbXB0eU1hcGkiOnRydWUsIlYiOiIwLjAuMDAwMCIsIlAiOiJXaW4zMiIsIkFOIjoiTWFpbCIsIldUIjoyfQ%3D%3D%7C0%7C%7C%7C&sdata=NytlBQjxkFjljNVkMblHwINkqhKXNSAT2cPk%2FPaXJ%2B0%3D&reserved=0 "原始的 URL: https://learn.microsoft.com/zh-tw/azure/communication-services/quickstarts/email/create-email-communication-resource。按一下或點選 [如果您信任此連結。")
    

* * *

步驟三：設定網域

*   測試用途：
    
    您可以建立 Azure 管理的網域（Managed Domain）供測試用途。  
    ⚠️ _注意：Azure 管理網域不建議用於正式環境_。
    
    [將 Azure 管理網域新增至 Email Communication Service](https://apc01.safelinks.protection.outlook.com/?url=https%3A%2F%2Flearn.microsoft.com%2Fzh-tw%2Fazure%2Fcommunication-services%2Fquickstarts%2Femail%2Fadd-managed-domain&data=05%7C02%7CTifa_Chen%40walsin.com%7Cc227936ea6fc4246b87108ddc5d5f0bc%7C97876bedbb9a4617802b94c62e7837b5%7C1%7C0%7C638884244842403716%7CUnknown%7CTWFpbGZsb3d8eyJFbXB0eU1hcGkiOnRydWUsIlYiOiIwLjAuMDAwMCIsIlAiOiJXaW4zMiIsIkFOIjoiTWFpbCIsIldUIjoyfQ%3D%3D%7C0%7C%7C%7C&sdata=700SUCpL8jn6JvyAVv91sO5nWxU57td03U7JWQFGwYo%3D&reserved=0 "原始的 URL: https://learn.microsoft.com/zh-tw/azure/communication-services/quickstarts/email/add-managed-domain。按一下或點選 [如果您信任此連結。")
    
      
     
    
*   正式用途：
    
    若為正式用途，請使用自訂網域（例如公司擁有的子網域）  
    您可參考下列文件購買網域：
    
    [購買自訂網域](https://apc01.safelinks.protection.outlook.com/?url=https%3A%2F%2Flearn.microsoft.com%2Fzh-tw%2Fazure%2Fcommunication-services%2Fquickstarts%2Femail%2Fadd-azure-managed-domains%3Fpivots%3Dplatform-azp&data=05%7C02%7CTifa_Chen%40walsin.com%7Cc227936ea6fc4246b87108ddc5d5f0bc%7C97876bedbb9a4617802b94c62e7837b5%7C1%7C0%7C638884244842418181%7CUnknown%7CTWFpbGZsb3d8eyJFbXB0eU1hcGkiOnRydWUsIlYiOiIwLjAuMDAwMCIsIlAiOiJXaW4zMiIsIkFOIjoiTWFpbCIsIldUIjoyfQ%3D%3D%7C0%7C%7C%7C&sdata=TjvYU7KkdGFK4LwojC4lKAen4gbdTuuWpoTTfTxm%2F%2Bw%3D&reserved=0 "原始的 URL: https://learn.microsoft.com/zh-tw/azure/communication-services/quickstarts/email/add-azure-managed-domains?pivots=platform-azp。按一下或點選 [如果您信任此連結。")
    

*   新增與驗證網域或子網域：
    
    根據您使用的是 根網域 或 子網域（如您目前使用子網域），請依據說明將 DNS 記錄新增至對應的 DNS 區域，或更新自動產生的記錄。
    
    [新增自訂已驗證的電子郵件網域](https://apc01.safelinks.protection.outlook.com/?url=https%3A%2F%2Flearn.microsoft.com%2Fzh-tw%2Fazure%2Fcommunication-services%2Fquickstarts%2Femail%2Fadd-custom-verified-domains%3Fpivots%3Dplatform-azp&data=05%7C02%7CTifa_Chen%40walsin.com%7Cc227936ea6fc4246b87108ddc5d5f0bc%7C97876bedbb9a4617802b94c62e7837b5%7C1%7C0%7C638884244842438557%7CUnknown%7CTWFpbGZsb3d8eyJFbXB0eU1hcGkiOnRydWUsIlYiOiIwLjAuMDAwMCIsIlAiOiJXaW4zMiIsIkFOIjoiTWFpbCIsIldUIjoyfQ%3D%3D%7C0%7C%7C%7C&sdata=jZ1HDV4mtqF9eY1xFdzDeM568rERKdDDHWOdr9gGbIw%3D&reserved=0 "原始的 URL: https://learn.microsoft.com/zh-tw/azure/communication-services/quickstarts/email/add-custom-verified-domains?pivots=platform-azp。按一下或點選 [如果您信任此連結。")
    

*   驗證網域與子網域：
    
    請依據說明進行 DNS 驗證，以確保網域可用於發送郵件。
    
    [驗證自訂電子郵件網域](https://apc01.safelinks.protection.outlook.com/?url=https%3A%2F%2Flearn.microsoft.com%2Fzh-tw%2Fazure%2Fcommunication-services%2Fquickstarts%2Femail%2Fadd-custom-verified-domains%3Fpivots%3Dplatform-azp%23configure-sender-authentication-for-custom-domain&data=05%7C02%7CTifa_Chen%40walsin.com%7Cc227936ea6fc4246b87108ddc5d5f0bc%7C97876bedbb9a4617802b94c62e7837b5%7C1%7C0%7C638884244842459119%7CUnknown%7CTWFpbGZsb3d8eyJFbXB0eU1hcGkiOnRydWUsIlYiOiIwLjAuMDAwMCIsIlAiOiJXaW4zMiIsIkFOIjoiTWFpbCIsIldUIjoyfQ%3D%3D%7C0%7C%7C%7C&sdata=ZPhPeXvn%2BmUukYuZ6na4gBZW46y73BwoubGnSGsAYH0%3D&reserved=0 "原始的 URL: https://learn.microsoft.com/zh-tw/azure/communication-services/quickstarts/email/add-custom-verified-domains?pivots=platform-azp#configure-sender-authentication-for-custom-domain。按一下或點選 [如果您信任此連結。")
    

  
 

* * *

步驟四：連接已驗證的網域

*   [如何連接已驗證的電子郵件網域](https://apc01.safelinks.protection.outlook.com/?url=https%3A%2F%2Flearn.microsoft.com%2Fzh-tw%2Fazure%2Fcommunication-services%2Fquickstarts%2Femail%2Fconnect-email-communication-resource%3Fpivots%3Dazure-portal&data=05%7C02%7CTifa_Chen%40walsin.com%7Cc227936ea6fc4246b87108ddc5d5f0bc%7C97876bedbb9a4617802b94c62e7837b5%7C1%7C0%7C638884244842478636%7CUnknown%7CTWFpbGZsb3d8eyJFbXB0eU1hcGkiOnRydWUsIlYiOiIwLjAuMDAwMCIsIlAiOiJXaW4zMiIsIkFOIjoiTWFpbCIsIldUIjoyfQ%3D%3D%7C0%7C%7C%7C&sdata=9rFV2lQaXfWX4hSXuTAS9y8JDfnAxMRR0BAXkTTM4Kk%3D&reserved=0 "原始的 URL: https://learn.microsoft.com/zh-tw/azure/communication-services/quickstarts/email/connect-email-communication-resource?pivots=azure-portal。按一下或點選 [如果您信任此連結。")
    

* * *

步驟五：發送電子郵件

*   [開始發送電子郵件](https://apc01.safelinks.protection.outlook.com/?url=https%3A%2F%2Flearn.microsoft.com%2Fzh-tw%2Fazure%2Fcommunication-services%2Fquickstarts%2Femail%2Fsend-email%3Ftabs%3Dwindows%252Cconnection-string%252Csend-email-and-get-status-async%252Csync-client%26pivots%3Dplatform-azportal&data=05%7C02%7CTifa_Chen%40walsin.com%7Cc227936ea6fc4246b87108ddc5d5f0bc%7C97876bedbb9a4617802b94c62e7837b5%7C1%7C0%7C638884244842496481%7CUnknown%7CTWFpbGZsb3d8eyJFbXB0eU1hcGkiOnRydWUsIlYiOiIwLjAuMDAwMCIsIlAiOiJXaW4zMiIsIkFOIjoiTWFpbCIsIldUIjoyfQ%3D%3D%7C0%7C%7C%7C&sdata=zhD3%2BSj2j0aJOYxMcIFvQe2Z%2Fq4tV0dO0psEsyArvks%3D&reserved=0 "原始的 URL: https://learn.microsoft.com/zh-tw/azure/communication-services/quickstarts/email/send-email?tabs=windows%2Cconnection-string%2Csend-email-and-get-status-async%2Csync-client&pivots=platform-azportal。按一下或點選 [如果您信任此連結。")
