[[_TOC_]]

# 建議使用時機
------

| 你的情況 | 建議選擇 |
| --- | --- |
| 不確定未來要用什麼 SKU / 地區 | **選「節省方案」**（最彈性） |
| 要經常調整 VM 類型、橫向擴展 | **節省方案更適合** |
| 明確知道要用固定 VM 長期跑 | **選「保留實例」折扣更高** |
| 正在大量使用 Functions、App Services | **節省方案支援這些服務，RI 不支援** |
| 想簡化成本管理、提升使用率 | **節省方案優先** |




# 購買保留項目
Azure 預訂協助您通過承諾使用多項 Azure 資源一年或三年期的方案來節省成本。 在您簽訂購買保留名額的承諾之前，請務必先審查以下各節，以做好購買準備。

## 誰可以購買保留
-------

若要保留，您必須具備 Enterprise (MS-AZR-0017P 或 MS-AZR-0148P) 或隨用隨付訂用帳戶 (MS-AZR-0003P 或 MS-AZR-0023P) 或隨用隨付 (MS-AZR-0003P 或 MS-AZR-0023P) 或 Microsoft 客戶合約的 Azure 訂用帳戶擁有者角色或保留購買者角色。

雲端解決方案提供者可使用 Azure 入口網站或[合作夥伴中心](https://learn.microsoft.com/zh-tw/partner-center/azure-reservations)來購買 Azure 保留。 CSP 合作夥伴可以在客戶授權時，在合作夥伴中心為他們購買保留。 如需詳細資訊，請參閱[代表您的客戶購買 Microsoft Azure 預訂](https://learn.microsoft.com/zh-tw/partner-center/azure-reservations-buying)。 或者，一旦合作夥伴授權給終端客戶，且客戶具有保留購買者角色，其就可在 Azure 入口網站中購買保留。

如果您有模擬 Azure 訂用帳戶擁有者角色或保留購買者角色的自訂角色，則無法購買保留。 您必須使用內建的擁有者或內建的保留購買者角色。

Enterprise 合約 (EA) 客戶可在 **Azure 入口網站**中停用[保留執行個體](https://portal.azure.com/#blade/Microsoft_Azure_GTM/ModernBillingMenuBlade/BillingAccounts)原則選項，以將購買權限限制於 EA 管理員。 若要變更設定，請流覽至 [ **原則]** 功能表。
Microsoft 客戶合約 (MCA)，帳單設定檔擁有者可以在 [azure 入口網站](https://portal.azure.com/#blade/Microsoft_Azure_GTM/ModernBillingMenuBlade/BillingAccounts)中停用**保留執行個體**原則選項，以限制保留購買。 從 2025 年 6 月開始，即使 Azure 入口網站中停用保留執行個體原則選項，Microsoft 客戶合約 (MCA)、帳單設定檔和計費帳戶擁有者仍能夠購買保留。 若要變更設定，請流覽至 [帳單配置檔] 底下的 **[**原則**]** 功能表。

EA 管理員或帳單設定檔擁有者必須至少有一個 EA 或 MCA 訂用帳戶的擁有者或保留購買者存取權，才能購買保留。 當企業希望由一個集中的團隊來購買預訂時，此選項非常有用。

保留折扣僅適用於與透過 Enterprise、雲端解決方案提供者 (CSP)、Microsoft 客戶合約購買的訂用帳戶相關聯的資源，以及採用隨用隨付費率的個別方案。


## 設定保留範圍
------
您可以將保留範圍設定為訂用帳戶或資源群組。 設定保留的範圍是選擇保留所省下的成本適用在哪些地方。 當您將保留範圍設定為資源群組時，保留折扣僅適用於該資源群組，而不會適用於整個訂用帳戶。

### 保留範圍設定選項

視您的需求而定，您有四個選項可設定保留範圍：
*   **單一資源群組範圍** - 只會將保留折扣套用至所選資源群組中的相符資源。
*   **單一訂用帳戶範圍** - 會將保留折扣套用至所選訂用帳戶中的相符資源。
*   **共用範圍** - 會將保留折扣套用至計費內容中合格訂用帳戶的相符資源。 如果訂用帳戶已移至不同的計費內容，則權益不再套用至訂用帳戶。 其會繼續套用至計費內容中的其他訂用帳戶。
    *   針對 Enterprise 合約客戶，計費內容為註冊。 保留共用範圍會在註冊中包含多個 Microsoft Entra 租用戶。
    *   針對 Microsoft 客戶合約客戶，計費範圍是帳單設定檔。 保留共用範圍可以在一個計費設定檔中包含多個 Microsoft Entra 租用戶。
    *   針對採用隨用隨付費率方案的個人訂閱，計費範圍包括所有由帳戶管理員所創建的符合資格的訂閱。
*   **管理群組** - 將保留折扣套用至管理群組和計費範圍所包含的訂用帳戶清單中的相符資源。 管理群組範圍適用於整個管理群組階層中的所有訂用帳戶。 若要為管理群組購買保留，您必須至少有管理群組的讀取權限，並且是計費訂用帳戶上的保留擁有者或保留購買者。
雖然 Azure 會對使用量套用保留折扣，但會依下列順序處理保留：
1.  具有單一資源群組範圍的保留
2.  具有單一訂用帳戶範圍的保留
3.  已將範圍設定為管理群組的保留
4.  具有共用範圍 (多個訂用帳戶) 的保留，如先前所述


## 購買保留
----

當您購買預訂時，會使用目前的 UTC 日期和時間來記錄交易。
您可以透過 Azure 入口網站、API、PowerShell 和 CLI 購買預訂。 當您準備好進行預訂時，請閱讀下列適用的文章：
*   [應用程式服務](https://learn.microsoft.com/zh-tw/azure/cost-management-billing/reservations/prepay-app-service)
*   [App Service - JBoss EA 整合式支援](https://learn.microsoft.com/zh-tw/azure/cost-management-billing/reservations/prepay-jboss-eap-integrated-support-app-service)
*   [Azure 備份](https://learn.microsoft.com/zh-tw/azure/backup/backup-azure-reserved-pricing-optimize-cost)
*   [Azure Cache for Redis](https://learn.microsoft.com/zh-tw/azure/azure-cache-for-redis/cache-reserved-pricing)
*   [Azure Data Factory](https://learn.microsoft.com/zh-tw/azure/data-factory/data-flow-understand-reservation-charges?toc=/azure/cost-management-billing/reservations/toc.json)
*   [適用於 MySQL 的 Azure 資料庫](https://learn.microsoft.com/zh-tw/azure/mysql/concept-reserved-pricing)
*   [適用於 PostgreSQL 的 Azure 資料庫](https://learn.microsoft.com/zh-tw/azure/postgresql/concept-reserved-pricing)
*   [Azure Blob 儲存體](https://learn.microsoft.com/zh-tw/azure/storage/blobs/storage-blob-reserved-capacity?toc=/azure/cost-management-billing/reservations/toc.json)
*   [Azure Files](https://learn.microsoft.com/zh-tw/azure/storage/files/files-reserve-capacity?toc=/azure/cost-management-billing/reservations/toc.json)
*   [Azure VMware 解決方案](https://learn.microsoft.com/zh-tw/azure/azure-vmware/reserved-instance?toc=/azure/cost-management-billing/reservations/toc.json)
*   [Azure Cosmos DB](https://learn.microsoft.com/zh-tw/azure/cosmos-db/cosmos-db-reserved-capacity?toc=/azure/cost-management-billing/reservations/toc.json)
*   [Azure OpenAI](https://learn.microsoft.com/zh-tw/azure/cost-management-billing/reservations/azure-openai)
*   [Azure SQL Edge](https://learn.microsoft.com/zh-tw/azure/cost-management-billing/reservations/prepay-sql-edge)
*   [Databricks](https://learn.microsoft.com/zh-tw/azure/cost-management-billing/reservations/prepay-databricks-reserved-capacity)
*   [資料總管](https://learn.microsoft.com/zh-tw/azure/data-explorer/pricing-reserved-capacity?toc=/azure/cost-management-billing/reservations/toc.json)
*   [專用主機](https://learn.microsoft.com/zh-tw/azure/virtual-machines/prepay-dedicated-hosts-reserved-instances)
*   [適用於雲端的 Defender - 預先購買](https://learn.microsoft.com/zh-tw/azure/defender-for-cloud/prepurchase-plan?toc=/azure/cost-management-billing/reservations/toc.json)
*   [磁碟儲存體](https://learn.microsoft.com/zh-tw/azure/virtual-machines/disks-reserved-capacity)
*   [Microsoft Fabric](https://learn.microsoft.com/zh-tw/azure/cost-management-billing/reservations/fabric-capacity)
*   [Microsoft Sentinel - 預先購買](https://learn.microsoft.com/zh-tw/azure/sentinel/billing-pre-purchase-plan?toc=/azure/cost-management-billing/reservations/toc.json) (英文)
*   [Azure BareMetal 上的 Nutanix](https://learn.microsoft.com/zh-tw/azure/cost-management-billing/reservations/nutanix-bare-metal)
*   [Red Hat OpenShift](https://learn.microsoft.com/zh-tw/azure/cost-management-billing/reservations/prepay-red-hat-openshift)
*   [SAP HANA 大型執行個體](https://learn.microsoft.com/zh-tw/azure/cost-management-billing/reservations/prepay-hana-large-instances-reserved-capacity)
*   [軟體方案](https://learn.microsoft.com/zh-tw/azure/virtual-machines/linux/prepay-suse-software-charges?toc=/azure/cost-management-billing/reservations/toc.json)
*   [SQL Database](https://learn.microsoft.com/zh-tw/azure/azure-sql/database/reserved-capacity-overview?toc=/azure/cost-management-billing/reservations/toc.json)
*   [Synapse Analytics - 資料倉儲](https://learn.microsoft.com/zh-tw/azure/cost-management-billing/reservations/prepay-sql-data-warehouse-charges)
*   [Synapse Analytics - 預先購買](https://learn.microsoft.com/zh-tw/azure/cost-management-billing/reservations/synapse-analytics-pre-purchase-plan)
*   [虛擬機器](https://learn.microsoft.com/zh-tw/azure/virtual-machines/prepay-reserved-vm-instances?toc=/azure/cost-management-billing/reservations/toc.json)
*   [虛擬機器軟體](https://learn.microsoft.com/zh-tw/azure/cost-management-billing/reservations/buy-vm-software-reservation)


## 購買或每月付款的 Azure 保留
-----------------
您可以選擇每月付款的方式進行預訂。 不同於需支付全額的預先購買，每月付款選項會將保留的總費用平均分配至期限中的每一個月份。 預付和每月付款的保留總費用是相同的，當您選擇按月支付時，您不需要支付任何額外費用。

如果是使用 Microsoft 客戶合約 (MCA) 購買保留，則您的月付金額可能會依當地貨幣的當月市場匯率而異。

每月付款不適用於：Databricks、Synapse Analytics - 預先購買、SUSE Linux 保留、Red Hat 方案及 Azure Red Hat OpenShift 授權。


### 檢視已支付的款項
您可以使用 API、使用量資料和成本分析來檢視已支付的款項。 針對每月付款的預約，使用量資料和預約費用 API 中的頻率值會標示為**定期**。 針對預先付款的保留，此值會顯示為**一次性**。

成本分析會在預設檢視中顯示每月的購買。 在 [費用類型] 上套用 [購買]，以及在 [頻率] 上套用 [定期] 來進行篩選，即可查看所有購買。 若只要檢視保留，請篩選 [保留]。
![](https://learn.microsoft.com/zh-tw/azure/cost-management-billing/reservations/media/prepare-buy-reservation/cost-analysis.png)

### 交換和退款

就像其他保留一樣，您可以針對以每月計費購買的保留進行退款或交換。

當您以每月付款交換保留時，新保留的總成本必須高於傳回保留的剩餘付款。 交換沒有其他限制或費用。 您可以交換預先付款的保留，以購買按月計費的新保留。 不過，新保留的存留期值應該大於退回保留的比例分配值。

如果您取消按月付款的保留，則取消的未來付款將累計到 50,000 美元的退款限額。

如需有關交換和退款的詳細資訊，請參閱 [Azure 保留的自助式交換和退款](https://learn.microsoft.com/zh-tw/azure/cost-management-billing/reservations/exchange-and-refund-azure-reservations)。

[](https://learn.microsoft.com/zh-tw/azure/cost-management-billing/reservations/prepare-buy-reservation#reservation-notifications)


### 保留通知
----

根據您支付 Azure 訂用帳戶的方式，我們會透過電子郵件，將保留通知傳送給貴組織中的下列使用者。 系統會針對各種事件傳送通知，包括：
*   購買
*   即將到期的保留期限
*   過期
*   續約
*   取消
*   範圍變更
通知會傳送給下列使用者：
*   具有 EA 訂用帳戶的客戶
    *   通知會傳送給 EA 通知連絡人、EA 管理員、保留擁有者和保留管理員。
*   具有 Microsoft 客戶合約 (Azure 方案) 的客戶
    *   通知會傳送給保留擁有者和保留管理員。
*   雲端解決方案提供者和新的商務合作夥伴
    *   通知會傳送給由合作夥伴法律資訊帳戶設定所識別的主要聯絡合作夥伴。 如需如何更新合作夥伴帳戶設定主要聯繫人電子郵件地址的詳細資訊，請參閱[驗證或更新公司設定檔資訊](https://learn.microsoft.com/zh-tw/partner-center/update-your-partner-profile#update-your-legal-business-profile)。
*   使用隨用隨付費率的個別訂用帳戶客戶
    *   電子郵件會傳送給已設定為帳戶管理員、保留擁有者和保留管理員的使用者。



--- 

範例:
- 選擇服務項目: App Service
- 選擇付款方式: 每月/一次付清
- 選擇付款週期: 1年/3年
- 確認後購買

![image.png](/.attachments/image-0bb797fa-43ff-4485-b35c-d692d41db4ef.png)

![image.png](/.attachments/image-95bb2336-09cd-48c2-bc01-556fcc2b8d6a.png)

![image.png](/.attachments/image-dd046e6d-c2e0-4fae-a3cf-42fd2fcb45ce.png)

![image.png](/.attachments/image-a6c8e0be-0525-4c9b-8fac-1a484f26f916.png)


參考
https://learn.microsoft.com/zh-tw/azure/cost-management-billing/reservations/prepare-buy-reservation


# 節省方案
透過承諾在一或三年內每小時固定支出金額，您可以在全球範圍內的選取計算服務上節省成本，並在達到每小時承諾用量之前享有較低的價格。

- 針對所選服務，與隨用隨付價格相比，最多可節省 65%；選擇較長期方案還能獲得更多節省費用。新增其他節省費用供應項目 (例如 [Azure Hybrid Benefit](https://azure.microsoft.com/zh-tw/pricing/offers/hybrid-benefit))，可省下更多成本。
- 無論地區、執行個體資料數列或作業系統為何，均可節省開支，而最大的節省項目會自動優先套用。現代化您的工作負載，並隨時間依照您的需求變更而持續進行節省。
- 選擇一年或三年期限，根據您最近的使用量，以個人化建議來設定方案的每小時金額。預先全額支付或每月分期支付，無須額外費用。跨訂閱、資源群組、管理群組或整個 Azure 帳戶套用節省。

## 涵蓋的服務
#### Azure 虛擬機器
佈建 Windows 和 Linux VM 只需短短幾秒鐘的時間。
`虛擬機器不包括 BareMetal 基礎結構 和 Av1 系列。`

#### Azure App Service
為 Web 和行動裝置快速建立強大的雲端應用程式
`適用於計算的 Azure 節省方案只能套用至 App Service 升級進階 v3 方案和升級隔離式 v2 方案。`

#### Azure Functions 進階方案
透過端對端的開發體驗，執行事件驅動的無伺服器程式碼函式。

#### Azure 容器執行個體
啟動具有 Hypervisor 隔離的容器。

#### Azure 專用主機
使用專用實體伺服器來裝載適用於 Windows 和 Linux 的 Azure VM。

#### Azure 容器應用程式
使用無伺服器容器來建置及部署現代化應用程式和微服務。

#### 適用於企業的 Azure Spring 應用程式
使用 Microsoft 和 VMware 提供之完全受管理的服務來建置和部署 Spring Boot 應用程式。

參考
https://azure.microsoft.com/zh-tw/pricing/offers/savings-plan-compute#footnote-3