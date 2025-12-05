[[_TOC_]]


## Excessive Data Exposure
當透過 Checkmarx 集合為 OWASP TOP 10 - 2021 掃 ASP.NET MVC 時，
如果回傳的物件中，如果 類別/屬性名稱 中有一些 機敏性名稱時，
Checkmarx 就會出 Excessive_Data_Exposure 的中風險

### 機敏性名稱類例如以下字串，
"*Credit*",
"*credentials*",
"*secret*",
"*Account*",
"*SSN",
"DOB",
"SSN*",
"*SocialSecurity*",
"DeviceUniqueId",
"auth*",
"*passport*" 

### 建議
所以回傳前要先將那些機敏性屬性值清掉，
或是建立一個沒有那些機敏性屬性的類別回傳出去。
以下是透過 Method ，將機敏性屬性值清掉 (user.WithoutSensitiveData())，如下，
![image.png](/.attachments/image-cd4c6086-3d3b-4763-88b6-bc211c87f558.png)
