[[_TOC_]]

# 說明
在 Azure Hub-Spoke 架構下，
- Spoke 串接 Ruleset-pdns-azure，可轉送到外服務。
- Hub 串接 Ruleset-pdns-walsin，當各Spoke VNET 請求到 FW時，才會解析到內網DNS服務。

![](https://learn.microsoft.com/zh-tw/azure/dns/media/dns-resolver-overview/resolver-architecture.png#lightbox)

---
問題：
我們的Azure firewall無法正常解析*.walsin.com相關的域名。我們希望用內網DNS server解析相關的域名。

解決方案：
1. 我們創建了一個新DNS forwarding ruleset 並在其中創建了一個rule， 將去往*.walsin.com的DNS query送到您的onprem DNS server做解析。
1. 我們將這個DNS forwarding ruleset與您的Azure firewall所在的Vnet做了Virtual link，保證Azure Firewall的針對*.walsin.com的流量會被送到您onprem的DNS server。
1. 我們在您的測試機器上進行了驗證，確定現在的域名解析已經正常。

補充信息：這里是對會議上討論的問題的總結。


- 針對您在會議上提出的關於DNS resolver inbound/outbound endpoint 的區別:
Private DNS resolver的inbound endpoint 可以被看做是一組部署在Azure上的custom DNS server，您可以指定onprem/Azure上的DNS query送到指定的DNS inbound endpoint IP，inbound endpoint會做DNS解析。而outbound endpoint更像是我們在DNS server里部署的Forwarding rules，outbound endpoint只處理我們配置的Forwarding ruleset里match的rule的DNS query的轉發請求。
更多細節可參考 https://learn.microsoft.com/en-us/azure/dns/dns-private-resolver-overview#inbound-endpoints

- 為什麽不能使用現有的Forwarding Ruleset增加Virtual network link：
在您現有的Forwarding ruleset中，您有一些rule 設置了destination為您的Private DNS resolver的inbound endpoint，在這種rule存在的情況下，我們不能再把這個Forwarding ruleset跟您Inbound endpoint所在的Vnet進行綁定，不然會造成DNS query loop。因為Azure firewall和DNS resolver是在同一個Vnet，所以不能直接增加現有ruleset的Virtual link。
更多解釋清參考 https://learn.microsoft.com/en-us/azure/dns/private-resolver-endpoints-rulesets#rules.


---

# 建立 DNS Forwarding Ruleset
![image.png](/.attachments/image-ec610917-2cdf-42ac-963e-aab5f477cbea.png)
 
![image.png](/.attachments/image-92e50530-9cc2-45d1-9a90-5e133efae82f.png)

# 新增 往華新內部DNS 
- rule-walsin-com
- 指定名稱
- 指定DNS IP

![image.png](/.attachments/image-5cb80539-5554-45bc-a8dc-b792a9688ffa.png)

# V-Link 串接
串接 Azure Hub VNET (AZ-VNETCONNP01)
一個VNET 只能串接一組 Ruleset
![image.png](/.attachments/image-499feab0-236a-4515-b7de-c5f6dfc1eb91.png)

# dns-outbound endpoint
指定解析器 pdns-resolver
也就是 forwarding
![image.png](/.attachments/image-27c1433a-fda7-40e0-9dae-9783117e7149.png)


# DNS private resolver
會看到建立後的結果
![image.png](/.attachments/image-e98a83c3-88bf-4aff-b8e6-87b4857e6b4f.png)
![image.png](/.attachments/image-23b042a6-fe2b-40ef-a0f2-086a68c53c71.png)
![image.png](/.attachments/image-c6348469-a300-4b49-bebb-dd77f9a0f499.png)

