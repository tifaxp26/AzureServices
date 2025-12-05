[[_TOC_]]


從您的所有資料中揭露見解，並以 Azure Databricks 建立人工智慧 (AI) 解決方案，迅速設定您的 Apache Spark 環境、自動調整規模，並對互動式工作區中的共用專案進行共同作業。
![image.png](/.attachments/image-8a7807f2-9254-4124-b4e9-f9f115b2a69d.png)

![image.png](/.attachments/image-6acdb921-6ab2-41f5-ad8d-51b0a2219175.png)


# 虛擬網路

AZ-VNETDPP03

位址空間: 10.101.126.0/26
位址範圍: 10.101.126.0 - 10.101.126.63
![image.png](/.attachments/image-ed1869a5-40da-4fc2-8317-fde5910b0267.png)


子網路
AZ-VNETDPP03
【專案Private Links】 AZ-SUBNP126-0 10.101.126.0/27
【公用子網路】 AZ-SUBNP126-32 10.101.126.32/28
【私人子網路】 AZ-SUBNP126-48 10.101.126.48/28



![image.png](/.attachments/image-5fa2d376-0ea5-49fe-85d8-5bd3e5911de7.png)

對等互連
AZ-VNETDPP03

![image.png](/.attachments/image-2d590011-a9e6-4572-9f12-0c4a24a878a0.png)

-----------------------------------------


# IP Group

ip-group-dataplatform-prod
IP Addresses: 10.101.124.0/22

![image.png](/.attachments/image-e3c4e604-af0f-48eb-b8b9-ae6743e11d84.png)

-----------------------------------------


# 路由表
rt-dataplatform-prod

名稱: AZ-SUBNP126-0
  位址範圍: 10.101.126.0/27
  虛擬網路: AZ-VNETDPP03
  安全性群組: dadatabricks-sandbox-nsg

【公用子網路】
名稱: AZ-SUBNP126-32
  位址範圍: 10.101.126.32/28
  虛擬網路: AZ-VNETDPP03
  安全性群組: databricksnsgi2qm43ehoz45s

【私人子網路】
名稱: AZ-SUBNP126-48
  位址範圍: 10.101.126.48/28
  虛擬網路: AZ-VNETDPP03
  安全性群組: databricksnsgi2qm43ehoz45s

![image.png](/.attachments/image-85c79cb9-e21c-4c67-a404-38bc17c03f39.png)

--------------------


# 服務建立步驟
基本
![image.png](/.attachments/image-035b67bb-25a7-4bbe-a0d7-2629763aae98.png)
網路功能
![image.png](/.attachments/image-240c7b89-b291-4d68-bb5b-72cd64822998.png)
私人端點
![image.png](/.attachments/image-99b2c688-b1d9-43d1-a2f3-a147ef100a67.png)

**注意:
這邊要建一條 Private Link 在 VPN VNET Subnet 中。**
![image.png](/.attachments/image-49939687-2521-4962-9c65-0ea77e6c3c4b.png)



# Azure 私人DNS 區域

可以看到 FQDN 會有多組IP

![image.png](/.attachments/image-9a0e3d77-aeb7-4963-b6b3-fcab1f597e2a.png)
![image.png](/.attachments/image-6e977f00-3b4e-4025-be2a-dab0668e2a15.png)
![image.png](/.attachments/image-1f1c2b6b-eaa7-4427-8d91-278df9c178dd.png)



# 完成 Portal
![image.png](/.attachments/image-1d29a71f-fb14-4d9e-9092-650ab50de0c0.png)


# RBAC
- 建議進入DataBricks 服務進行授權
- (不建議)進入 Data Bricks Portal IAM
![image.png](/.attachments/image-1e49972e-b7ed-45f8-9b78-e013e17bc985.png)


# Azure Firewall
Allow 清單
|  名稱   | 通訊協定  | 目的地類型 | 目的地 |
|  ----   | ----  |   ----  |   ----  |
| Allow Python Packages  | pypi.org,files.pythonhosted.org | FQDN | Https:443 | 
| Allow Linux Tools | database.clamav.net,esm.ubuntu.com | FQDN | Https:443 |
| Allow Docker | download.docker.com | FQDN | Https:443 |
| Allow Nvidia | nvidia.github.io,developer.download.nvidia.com | FQDN | Https:443 |
| Allow Nginx | nginx.org | FQDN | Http:80,Https:443 |



# 參考
- https://learn.microsoft.com/zh-tw/azure/databricks/security/network/classic/udr#azure-service-tags

