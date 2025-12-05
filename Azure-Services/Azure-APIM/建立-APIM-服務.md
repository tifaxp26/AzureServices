[[_TOC_]]


# 基本
     
| 地區 | Southeast Asia |
| --- | --- |
| 資源名稱 | apim-walsin-prod |
| 組織名稱 | Walsin Corp. |
| 管理員電子郵件 | Tifa_Chen@walsinsmc.onmicrosoft.com |
| 定價層 | 標準 v2 (99.95% SLA) |
| 單位 | 1 |

![image.png](/.attachments/image-6fa04712-ee22-4bc7-9d4a-ad4a246e41c4.png)

# 監視+安全
略，後續設定
![image.png](/.attachments/image-16628dd0-bbda-4265-9489-4b4d1831b968.png)

# 虛擬網路
略，後續設定
![image.png](/.attachments/image-e427b149-873e-4dd0-978f-a6414f68acb2.png)

# 受控識別
![image.png](/.attachments/image-9351d64f-ad4a-41bc-a2f7-e16ef194cd31.png)


---

# 使用虛擬網路來保護 Azure APIM 的輸入和輸出流量

## 虛擬網路插入 （傳統層）
------------

在 API 管理 傳統開發人員和進階層中，將 API 管理 實例部署在您控制存取的非因特網路子網中。 在虛擬網路中，APIM 執行個體可以安全存取其他網路 Azure 資源，也可以使用各種 VPN 技術連線到內部部署網路。
您可以使用 Azure 入口網站、Azure CLI、Azure Resource Manager 範本或其他工具來進行設定。 您可以使用 [網路安全組](https://learn.microsoft.com/zh-tw/azure/virtual-network/network-security-groups-overview)來控制 API 管理部署所在的子網輸入和輸出流量。
如需詳細的部署步驟和網路設定，請參閱：
*   [將您的 API 管理實例部署到虛擬網路 - 外部模式](https://learn.microsoft.com/zh-tw/azure/api-management/api-management-using-with-vnet)。
*   [將您的 API 管理實例部署至虛擬網路 - 內部模式](https://learn.microsoft.com/zh-tw/azure/api-management/api-management-using-with-internal-vnet)。
*   [API 管理插入虛擬網路的網路資源需求](https://learn.microsoft.com/zh-tw/azure/api-management/virtual-network-injection-resources)。



### 存取選項

使用虛擬網路，您可以設定開發人員入口網站、API 閘道和其他 API 管理 端點，以便從因特網（外部模式）或只能在虛擬網路中存取（內部模式）。
*   **外部** - API 管理端點可透過外部負載平衡器從公用因特網存取。 閘道可以存取虛擬網路內的資源。
    ![顯示外部虛擬網路連線的圖表。](https://learn.microsoft.com/zh-tw/azure/api-management/media/virtual-network-concepts/api-management-vnet-external.png)
    在外部模式中使用 APIM 來存取在虛擬網路中部署的後端服務。
    
*   **內部** - API 管理端點只能透過內部負載均衡器從虛擬網路內部訪問。 閘道可以存取虛擬網路內的資源。
    [![顯示與內部虛擬網路連線的圖表。](https://learn.microsoft.com/zh-tw/azure/api-management/media/virtual-network-concepts/api-management-vnet-internal.png)](https://learn.microsoft.com/zh-tw/azure/api-management/media/virtual-network-concepts/api-management-vnet-internal.png#lightbox)
    以內部模式使用 APIM，以：
    *   使用 Azure VPN 連線或 Azure ExpressRoute，讓裝載於私人資料中心的 API 可供第三方安全地存取。
    *   透過通用閘道器公開雲端式 API 和內部部署 API，以實現混合式雲端情節。
    *   使用單一閘道器端點來管理多個地理位置中所裝載的 API。

[](https://learn.microsoft.com/zh-tw/azure/api-management/virtual-network-concepts#virtual-network-injection-v2-tiers)

## 虛擬網路插入 （v2 層）
-------------

在 API 管理 進階 v2 層中，將您的實例插入虛擬網路的委派子網，以保護網關的輸入和輸出流量。 目前，您可以在建立實例時設定虛擬網路插入的設定。
在這裡設定中：
*   API 管理 閘道端點可透過私人IP位址的虛擬網路來存取。
*   只要已正確設定網路連線，API 管理就可以向孤立於網路或任何對等網路中的 API 後端提出外部請求。
針對您想要隔離 API 管理 實例和後端 API 的案例，建議使用此組態。 進階 v2 層中的虛擬網路插入會自動管理對 Azure API 管理 大部分服務相依性的網路連線。
![在虛擬網路中插入 API 管理實例以隔離輸入和輸出流量的圖表。](https://learn.microsoft.com/zh-tw/azure/api-management/media/inject-vnet-v2/vnet-injection.png)
如需詳細資訊，請參閱 [將進階 v2 實例插入虛擬網路](https://learn.microsoft.com/zh-tw/azure/api-management/inject-vnet-v2)。


## 虛擬網路整合 （v2 層）
-------------

標準 v2 和進階 v2 層支援輸出虛擬網路整合，讓您的 API 管理實例能夠連線到在單一連線虛擬網路或任何對等互連虛擬網路中隔離的 API 後端，只要網路連線已正確設定。 還是可以從網際網路公開存取 APIM 閘道、管理平面和開發人員入口網站。
輸出整合可讓 API 管理 實例同時連線到公用和網路隔離的後端服務。
![整合 API 管理实例与指派子网的图表。](https://learn.microsoft.com/zh-tw/azure/api-management/media/integrate-vnet-outbound/vnet-integration.png)
如需詳細資訊，請參閱 [整合 Azure API 管理實例與輸出連線的私人虛擬網路](https://learn.microsoft.com/zh-tw/azure/api-management/integrate-vnet-outbound)。


## 輸入私人端點
------

API 管理支援 [私人端點](https://learn.microsoft.com/zh-tw/azure/private-link/private-endpoint-overview) ，以保護您的 API 管理實例的輸入用戶端連線。 每個安全連線都會使用來自虛擬網路和 Azure Private Link 的私人 IP 位址。
[![此圖顯示使用私人端點與 API 管理的安全連線。](https://learn.microsoft.com/zh-tw/azure/api-management/media/virtual-network-concepts/api-management-private-endpoint.png)](https://learn.microsoft.com/zh-tw/azure/api-management/media/virtual-network-concepts/api-management-private-endpoint.png#lightbox)
使用私人端點和 Private Link，您可以：
*   建立多個與 APIM 執行個體的 Private Link 連線。
    
*   使用私人端點，以在安全連線上傳送輸入流量。
    
*   使用原則，以區分來自私人端點的流量。
    
*   僅將傳入流量限制為私人端點，以防止資料外流。
    
*   將私有入站端點整合至標準 v2 實例，並和輸出虛擬網路整合，為 API 管理用戶端和後端服務提供完整的網路隔離。
    ![此圖顯示使用私人端點與 API Management Standard v2 的安全輸入連線。](https://learn.microsoft.com/zh-tw/azure/api-management/media/private-endpoint/private-endpoint.png)
    



**重要
 您只能為 API 管理實例的 **輸入** 流量設定私人端點連線。**



如需詳細資訊，請參閱 [使用輸入私人端點私下連線到 API 管理](https://learn.microsoft.com/zh-tw/azure/api-management/private-endpoint)。



## 進階網路設定
------

### 使用 Web 應用程式防火牆保護 APIM 端點

您可能會遇到需要安全外部和內部存取 APIM 執行個體，以及觸達私人和內部部署後端彈性的情節。 在這些情節中，您可以選擇使用 Web 應用程式防火牆 (WAF) 管理對 APIM 執行個體端點的外部存取。
其中一個範例是將 APIM 執行個體部署在內部虛擬網路中，並使用網際網路對向 Azure 應用程式閘道將公用存取路由至其中：
[![此圖顯示 API 管理實例前面的應用程式閘道。](https://learn.microsoft.com/zh-tw/azure/api-management/media/virtual-network-concepts/api-management-application-gateway.png)](https://learn.microsoft.com/zh-tw/azure/api-management/media/virtual-network-concepts/api-management-application-gateway.png#lightbox)



# 將 Azure API 管理 實例與輸出連線的私人虛擬網路整合
===============================
**適用於：標準 v2 |進階 v2**
本文引導您完成為 Azure API 管理實例（標準 v2 或進階 v2（預覽））設定 _虛擬網路整合_ 的流程。 透過虛擬網路整合，您的實例可以對單一聯機虛擬網路或任何對等互連虛擬網路中隔離的 API 提出輸出要求，只要網路連線已正確設定。
當 API 管理 實例與虛擬網路整合以進行輸出要求時，閘道和開發人員入口網站端點仍可公開存取。 API 管理 實例可以同時連線到公用和網路隔離的後端服務。
![API 管理實例與負責出站流量的虛擬網路整合的圖表。](https://learn.microsoft.com/zh-tw/azure/api-management/media/integrate-vnet-outbound/vnet-integration.png)
如果您想要將 Premium v2 （預覽） API 管理實例插入虛擬網路，以隔離輸入和輸出流量，請參閱 [將進階 v2 實例插入虛擬網路](https://learn.microsoft.com/zh-tw/azure/api-management/inject-vnet-v2)。

 重要
*   本文所述的輸出虛擬網路整合僅適用於標準 v2 和進階 v2 層中的 API 管理 實例。 如需不同層級的網路選項，請參閱 [搭配 Azure API 管理使用虛擬網路](https://learn.microsoft.com/zh-tw/azure/api-management/virtual-network-concepts)。
*   您可以在標準 v2 或進階 v2 層中建立 API 管理 實例，或在建立實例之後啟用虛擬網路整合。
*   目前，您無法在進階 v2 實例的虛擬網路插入與虛擬網路整合之間切換。

[](https://learn.microsoft.com/zh-tw/azure/api-management/integrate-vnet-outbound#prerequisites)

必要條件
----

*   [標準 v2 或進階 v2](https://learn.microsoft.com/zh-tw/azure/api-management/v2-service-tiers-overview) 定價層中的 Azure API 管理實例
*   (選用) 為了進行測試，裝載於虛擬網路中不同子網路內的範例後端 API。 例如，請參閱 [教學課程：建立 Azure Functions 私人網站存取](https://learn.microsoft.com/zh-tw/azure/azure-functions/functions-create-private-site-access)。
*   具有您 API 管理 後端 API 裝載所在子網的虛擬網路。 如需虛擬網路和子網的需求和建議，請參閱下列各節。

[](https://learn.microsoft.com/zh-tw/azure/api-management/integrate-vnet-outbound#network-location)

### 網路位置

*   虛擬網路必須位於與 APIM 執行個體相同的區域與 Azure 訂用帳戶中。

[](https://learn.microsoft.com/zh-tw/azure/api-management/integrate-vnet-outbound#dedicated-subnet)

### 專用子網

*   用於虛擬網路整合的子網只能由單一 API 管理 實例使用。 無法與另一個 Azure 資源分享。

[](https://learn.microsoft.com/zh-tw/azure/api-management/integrate-vnet-outbound#subnet-size)

### 子網路大小

*   最小值：/27 (32 個位址)
*   建議： /24 （256 個位址） - 以容納調整 API 管理 實例

[](https://learn.microsoft.com/zh-tw/azure/api-management/integrate-vnet-outbound#network-security-group)

### 網路安全性群組

網路安全組必須與子網相關聯。 設定閘道存取 API 後端所需的任何網路安全組規則。 若要設定網路安全組，請參閱 [建立網路安全組](https://learn.microsoft.com/zh-tw/azure/virtual-network/manage-network-security-group)。

[](https://learn.microsoft.com/zh-tw/azure/api-management/integrate-vnet-outbound#subnet-delegation)

### 子網路委派

子網必須委派給 **Microsoft.Web/serverFarms** 服務。
![顯示入口網站中子網委派至 Microsoft.Web/serverFarms 的螢幕快照。](https://learn.microsoft.com/zh-tw/azure/api-management/media/virtual-network-injection-workspaces-resources/delegate-external.png)

 注意
您可能需要在訂用帳戶中註冊 `Microsoft.Web/serverFarms` 資源提供者，以便將子網路委派給服務。

如需設定子網委派的詳細資訊，請參閱 [新增或移除子網委派](https://learn.microsoft.com/zh-tw/azure/virtual-network/manage-subnet-delegation)。

[](https://learn.microsoft.com/zh-tw/azure/api-management/integrate-vnet-outbound#permissions)

### 權限

您至少必須擁有子網路或較高層級的下列角色型存取控制權限，才能設定虛擬網路整合：

展開資料表

| 動作 | 描述 |
| --- | --- |
| Microsoft.Network/virtualNetworks/read | 讀取虛擬網路定義 |
| Microsoft.Network/virtualNetworks/subnets/read | 讀取虛擬網路子網路定義 |
| Microsoft.Network/virtualNetworks/subnets/join/action | 加入虛擬網路 |

[](https://learn.microsoft.com/zh-tw/azure/api-management/integrate-vnet-outbound#configure-virtual-network-integration)

設定虛擬網路整合
--------

本節會引導您完成為現有 Azure API 管理實例設定外部虛擬網路整合的程式。 您也可以在建立新的 API 管理實例時設定虛擬網路整合。
1.  在 [Azure 入口網站](https://portal.azure.com/)中，流覽至您的 API 管理實例。
2.  在左側功能表中的 **[部署 + 基礎結構**] 底下，選取 [ **網络**>**編輯**]。
3.  在 [ **網络組態** ] 頁面上的 [ **輸出功能**] 底下，選取 [ **啟用** 虛擬網络整合]。
4.  選取您要整合的虛擬網路和委派子網。
5.  選取 [ **儲存**]。 虛擬網路已整合。

[](https://learn.microsoft.com/zh-tw/azure/api-management/integrate-vnet-outbound#optional-test-virtual-network-integration)

（選擇性）測試虛擬網路整合
-------------

如果您有裝載在虛擬網路中的 API，您可以將它匯入管理實例，並測試虛擬網路整合。 如需基本步驟，請參閱 [匯入和發佈 API](https://learn.microsoft.com/zh-tw/azure/api-management/import-and-publish)。



--- 
# 參考
- [Azure APIM 搭配 Azure 虛擬網路 | Microsoft Learn](https://learn.microsoft.com/zh-tw/azure/api-management/virtual-network-concepts)