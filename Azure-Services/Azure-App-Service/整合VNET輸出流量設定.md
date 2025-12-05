[[_TOC_]]

# 使用私人端點時的考量
您可以使用私人端點來替代服務端點，以保護應用程式閘道與 App Service (多租用戶) 間的流量。 您必須確保應用程式閘道可以使用 DNS 來解析 App Service 應用程式的私人 IP 位址。 或者，您可以使用後端集區中的私人 IP 位址，並在 HTTP 設定中覆寫主機名稱。
<IMG  src="https://learn.microsoft.com/zh-tw/azure/app-service/media/overview-app-gateway-integration/private-endpoint-appgw.png"  alt="Diagram that shows traffic flowing to an application gateway in an Azure virtual network and then flowing through a private endpoint to instances of apps in App Service."/>


# 虛擬網路整合

您可以控制哪些流量通過虛擬網路整合。應用程式路由會定義哪些流量從您的應用程式路由傳送至虛擬網路。設定路由會影響在應用程式啟動前或啟動期間發生的作業。虛擬網路路由會處理應用程式和設定流量從虛擬網路路由傳送出去的方式。

## 建立 App 專用 subnet 區域，一般建議 /26 或 /27 
![image.png](/.attachments/image-b15673be-954a-4c85-9832-5d4270d079cc.png)

![image.png](/.attachments/image-dc28428f-0695-4f7c-ba81-91848225520b.png)
![image.png](/.attachments/image-c11ae895-12f8-4bb9-b4fd-b8b28a109df1.png)


## 區域 VNET 集成
系統會自動分派 實例私有IP分配
![image.png](/.attachments/image-c48d046d-d41a-46df-84d1-765a979b225e.png)
![image.png](/.attachments/image-be2a1d1a-d64c-4783-8630-0386f5823a16.png)

# 部署位置
如果有啟用 部署位置 prePord / Prod
則會以 閘道的方式輸出

VNET 整合資訊會看不到
![image.png](/.attachments/image-b66c4813-e145-4c0a-a284-972f086a3620.png)

點選 VNET整合
![image.png](/.attachments/image-63506ee1-6928-4527-9ed4-da7c820dbc78.png)

到 KUDU 察看環境變數
輸出位置 WEBSITE_PRIVATE_IP
![image.png](/.attachments/image-4cac745a-2984-4797-85d7-61add0f21579.png)


