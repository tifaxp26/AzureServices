[[_TOC_]]

# 概觀
Azure SQL 受控執行個體是一項 PaaS 服務，與最新的 Enterprise Edition SQL Server 資料庫引擎具有近 100 個% 相容性。 
它提供原生 [虛擬網路 （VNet）](https://learn.microsoft.com/zh-tw/azure/virtual-network/virtual-networks-overview) 實作，可解決常見的安全性問題，以及有利於現有 SQL Server 客戶的 [商務模型](https://azure.microsoft.com/pricing/details/sql-database/) 。 
SQL 受控執行個體可讓現有的 SQL Server 客戶將其內部部署應用程式隨即轉移至雲端，而應用程式和資料庫變更最少。 
同時，SQL 受控執行個體會提供 PaaS 的所有功能 (自動修補和版本的更新、[自訂備份](https://learn.microsoft.com/zh-tw/azure/azure-sql/managed-instance/automated-backups-overview?view=azuresql)、[高可用性](https://learn.microsoft.com/zh-tw/azure/azure-sql/managed-instance/high-availability-sla-local-zone-redundancy?view=azuresql))，可以大幅降低管理負擔和擁有權總成本 (TCO)。
SQL 受控執行個體是專為想要將大量應用程式從內部部署或 IaaS、自行建置或 ISV 提供的環境移轉至完全受控 PaaS 雲端環境的客戶所設計，並儘可能減少移轉工作。 
透過使用 [Azure Arc 中的 SQL Server 遷移體驗](https://learn.microsoft.com/zh-tw/sql/sql-server/azure-arc/migrate-to-azure-sql-managed-instance)，或管理 [實例連結](https://learn.microsoft.com/zh-tw/azure/azure-sql/managed-instance/managed-instance-link-feature-overview?view=azuresql)，客戶可以將現有的 SQL Server 資料庫或實例移轉至 Azure SQL 管理實例，後者與 SQL Server 相容，並以原生 VNet 支援，完全隔離客戶實例。

透過軟體保證，您可以使用適用於 [SQL Server 的 Azure 混合式權益](https://azure.microsoft.com/pricing/hybrid-benefit/)，在 SQL 受控執行個體上以折扣費率配置現有授權。 對於需要高度安全性和程式設計介面豐富的 SQL Server 執行個體而言，SQL 受控執行個體是雲端中最佳的移轉目的地。

如需移轉選項和工具的詳細資訊，請參閱[移轉概觀：將 SQL Server 移轉至 Azure SQL 受控執行個體](https://learn.microsoft.com/zh-tw/azure/azure-sql/migration-guides/managed-instance/sql-server-to-managed-instance-overview?view=azuresql)。
下圖概述 SQL 受控執行個體的主要優勢：
![image.png](/.attachments/image-f36a01cf-ea87-4b85-9ac7-763f7d5c76e2.png)

### 商務智慧

Azure SQL 受控執行個體沒有原生內建的商業智慧套件，但您可以使用下列服務：
*   **SQL Server Integration Services （SSIS）** 是 [Azure Data Factory PaaS](https://learn.microsoft.com/zh-tw/azure/data-factory/tutorial-deploy-ssis-packages-azure) 的一部分。
*   **SQL Server Analysis Services （SSAS）** 是 Azure 中的個別 [PaaS](https://learn.microsoft.com/zh-tw/azure/analysis-services/analysis-services-overview) 服務。
*   **SQL Server Reporting Services （SSRS）** 時，您可以改用 [Power BI 編頁報表](https://learn.microsoft.com/zh-tw/power-bi/paginated-reports/paginated-reports-report-builder-power-bi) ，或在 Azure 虛擬機器上裝載 SSRS。 雖然 SQL 受控執行個體無法將 SSRS 作為服務執行，但它可以使用 SQL Server 驗證，裝載安裝在 Azure 虛擬機器上的報告伺服器的 [SSRS 目錄資料庫](https://learn.microsoft.com/zh-tw/sql/reporting-services/install-windows/ssrs-report-server-create-a-report-server-database#database-server-version-requirements) 。

請參考 詳細說明:
https://learn.microsoft.com/zh-tw/azure/azure-sql/managed-instance/sql-managed-instance-paas-overview?view=azuresql

---

# Azure SQL 受控執行個體的連線架構
=====================
## 概觀
在 SQL 受控執行個體中，執行個體會放置在 Azure 虛擬網路內，以及專用於 SQL 受控執行個體的子網路內。 此部署提供：

- 安全的虛擬網路本機 (VNet 本機) IP 位址。
- 將內部部署網路連線到 SQL 受控執行個體的能力。
- 將 SQL 受控執行個體連線到連結伺服器或其他內部部署資料存放區的能力。
- 將 SQL 受控執行個體連線到 Azure 資源的能力。


Azure SQL 受控執行個體的虛擬叢集連線結構。
![image.png](/.attachments/image-12d5883d-a9c7-4d8f-9858-2f4cd1323c83.png)
因為 Azure SQL MI 網路架構的設計
會先過一層 Gateway 服務，再到 instance ，
所以會拿所在的 subnet IP 多組。

請參考文件
https://learn.microsoft.com/zh-tw/azure/azure-sql/managed-instance/connectivity-architecture-overview?view=azuresql#communication-overview



# 建立 Azure SQL 受控執行個體

![image.png](/.attachments/image-2d397526-545a-47c8-85ff-297d83f421fb.png)

## 建立 SQL 受控執行個體
![image.png](/.attachments/image-f21af75d-ceb8-4d86-b28c-3bf2b7d05d77.png)

## 基本
- 命名規範: sqlmi-{服務名稱}-{環境}
![image.png](/.attachments/image-e9fcedf2-37ee-4301-9a1d-f9d6a1401f3b.png)
![image.png](/.attachments/image-c3f35d02-6706-41dd-884f-d9016ee07ff7.png)
- 選擇服務SKU、預估成本
![image.png](/.attachments/image-975d8e4b-f3e2-489e-8bd8-c3a89a4d5ffc.png)
![image.png](/.attachments/image-f6d2cd9a-b6c0-43e3-9163-78599d60c988.png)

## 網路
- 選擇 VNET/SUBNET
- 連接類型: 選 重新導向(Default)
就是整合 GW Node + MI Node 
- 公用端點: 停用
![image.png](/.attachments/image-5424ed7b-98b6-4daf-876d-2dc01d2fcc52.png)

## 安全性
![image.png](/.attachments/image-8dc4c285-5ae0-45f7-a77b-6097b4a5af74.png)

## 其他設定
- 定序: Chinese_Taiwan_Stroke_CI_AS
- 時區: UTC+8 Taipei
![image.png](/.attachments/image-d7b9bdee-2793-4ae5-aced-48c9b14ab71a.png)


---

# 網路 相關設定

概觀
--

Azure SQL 受控執行個體是由服務元件所組成，這些服務元件裝載在一 _組專用的隔離虛擬機器_ 上，這些虛擬機器放置在 [虛擬叢集](https://learn.microsoft.com/zh-tw/azure/azure-sql/managed-instance/virtual-cluster-architecture?view=azuresql) 裝載的一或多個虛擬機器 （VM） 群組內，並部署在 Azure [虛擬網路](https://learn.microsoft.com/zh-tw/azure/virtual-network/virtual-networks-overview)內。
虛擬叢集關聯虛擬網路的單一子網路，可託管一或多個 SQL 受控執行個體。 可部署於子網路的執行個體數目取決於子網路大小 (子網路範圍)。
當您建立 SQL 受控執行個體時，Azure 會根據所選的服務層級配置虛擬機器數目。 由於這些虛擬機器關聯您的子網路，因此其需 IP 位址。 Azure 可以分配更多的虛擬機，以確保在定期操作和服務維護期間的高可用性。 子網路所需的 IP 位址數目通常大於該子網路的 SQL 受控執行個體數目。

[](https://learn.microsoft.com/zh-tw/azure/azure-sql/managed-instance/vnet-subnet-determine-size?view=azuresql#determine-subnet-size)

判斷子網路大小
-------

請仔細規劃 SQL 受控執行個體部署的子網路大小。
根據設計，每個 SQL 受控執行個體在子網路需要至少 32 個 IP 位址。 在定義子網路 IP 範圍時，您可使用至少 /27 的子網路遮罩。
在判斷子網路大小時，請參考以下考量清單：
*   實例相關考量事項：
    *   SQL 受控執行個體數目
    *   執行個體的服務層級
*   虛擬叢集相關考量：
    *   硬體設定
    *   維護時段設定
*   管理作業相關考量：
    *   計劃擴大/縮小或變更服務層級、硬體組態或維護時段
使用下列參數來協助形成計算：
*   Azure 會針對自己的需求使用子網路的五個 IP 位址。
*   每個 [虛擬機群組](https://learn.microsoft.com/zh-tw/azure/azure-sql/managed-instance/virtual-cluster-architecture?view=azuresql#number-of-vm-groups) 會分配另外八個位址。
*   每個 SQL 受控執行個體使用的位址數目取決於服務層級。
    *   泛用 SQL 管理實例使用兩個位址
    *   業務關鍵 SQL 受控執行個體使用五個位址
*   每個擴展請求都會暫時將配置給待擴展的執行個體的地址數量加倍。

**重要**
**當資源存在於子網路中時，不支援變更子網路位址範圍。 因此，最好使用較大的子網路而不是較小的子網路，以防止未來出現問題。**

務必參考微軟文件說明
https://learn.microsoft.com/zh-tw/azure/azure-sql/managed-instance/vnet-subnet-determine-size?view=azuresql

### 單一部署實例

下表針對部署到每個服務層級子網路的單一執行個體顯示所需的 IP 位址數目：

| **服務層級** | **Azure 使用量**1 | **VM 群組使用量**2 | **實例使用量** | **區域備援 （ZR）** | **總計**3 |
| --- | --- | --- | --- | --- | --- |
| 常規用途 | 5 | 8 | 2 | 0 | 15 |
| 業務關鍵 | 5 | 8 | 5 | 2 | 18（20 含 ZR） |

1 Azure 所使用的位址會在子網路中的所有執行個體之間共用。  
2 虛擬機器群組使用的位址會在放置在相同群組內的執行個體之間共用。  
3 執行個體使用的位址總數。 為業務關鍵服務層級中的執行個體啟用區域備援時，會配置其他 IP 位址。
新增執行個體至子網路會增加執行個體所使用的位址數目，因此會增加位址總數。


### 多重執行個體子網路

本節中的公式計算子網路中多個實例所需的位址數目。 此公式會考量在後續執行個體建立或更新要求期間建立新 VM 群組的可能性，以及虛擬叢集的維護時段和硬體需求。
使用下列公式，根據執行個體數目計算 IP 位址總數：
`5 + (gp * 4) + (bc * 10) + (bc_zr * 2) + (vmg * 8)`，其中
*   gp = 一般用途執行個體數目
*   bc = 關鍵業務實例數目
*   bc_zr = 區域冗餘關鍵業務個體數目
*   vmg = 不同虛擬機器群組的數目
下列清單說明此公式使用的數目：
*   5 是 Azure 保留的 IP 位址數量
*   每個一般用途執行個體 4 個位址 （初始部署 2 個，最終擴展作業 2 個）
*   每個關鍵業務執行個體 10 個位址 （初始部署 5 個，最終擴展作業 5 個）
*   每個虛擬機器群組 8 個位址

#### 範例 1

您打算將三個一般用途執行個體和兩個業務關鍵執行個體部署至相同子網路。 所有執行個體都有相同的維護時段、在相同的硬體組態上執行，而且都不是區域冗餘。
將這些值代入公式會產生以下方程式： `5 + (3 * 4) + (2 * 10) + 0 + (1 * 8) = 45`
由於 IP 範圍以 2 的冪定義，因此若要支援 45 個 IP 位址，因此子網路需要此部署的最小 IP 範圍為 64 （2^6）。 請保留具有 /26 子網路遮罩的子網路。

[](https://learn.microsoft.com/zh-tw/azure/azure-sql/managed-instance/vnet-subnet-determine-size?view=azuresql#example-2)

#### 範例 2

您計劃將七個實例總共部署到相同的子網、四個一般用途和三個業務關鍵實例。 三個是在具有預設維護時段 （VM 群組 1） 的標準系列硬體上執行的開發/測試執行個體，而其餘四個則處於生產環境中，在具有週末維護時段 （VM 群組 2） 的進階系列硬體上執行。 其中兩個業務關鍵執行個體是區域備援。
將這些值代入公式會產生以下方程式： `5 + (4 * 4) + (3 * 10) + (1 * 2) + (2 * 8) = 69`
由於 IP 範圍以 2 的冪定義，因此若要支援 69 個 IP 位址，因此子網路需要此部署的最小 IP 範圍為 128 （2^7）。 您需要保留一個子網路，其子網路遮罩為 /25。


## 子網路
![image.png](/.attachments/image-6b347a74-328d-4604-8bfd-f57d2b4ef6bb.png)
![image.png](/.attachments/image-4cb207bb-5978-4bf2-b64b-e0ee52e19c37.png)
![image.png](/.attachments/image-c45e36df-55e4-4b90-b963-848b80c9d70f.png)

- 建議大小: /26
- 委派對象: Microsoft.Sql/managedInstances
- 路由表 (服務內建)
![image.png](/.attachments/image-f582e0a2-6d03-4e43-b6e3-9bfe56aa5a3f.png)

注意!! 
如果使用內部網路(VNET/Subnet) 或 私人端點(Private Link) 則需要有此路由規則
![image.png](/.attachments/image-54672e47-6e4b-4bfb-84e8-3d99dc1a67e9.png)
如過使用外部網路(Public network) 則不需要此路由規則!


- NSG (服務內建)
![image.png](/.attachments/image-d9050bb4-379e-4a09-a49c-84c2f34adc73.png)





Deploying managed instances and instance pools into private subnets is not supported. (代碼: DeploymentIntoPrivateSubnetsNotAllowed)
![image.png](/.attachments/image-9ea2aef3-89e8-4dc4-bf29-ac62001b8204.png)
私人子網路: 停用 (目前不支援)
![image.png](/.attachments/image-ae6200b4-ee17-4338-b63a-41eb0f6fcea8.png)


- 部署
![image.png](/.attachments/image-583d928d-6bf6-466e-8502-9f1c632dec20.png)

- 部署完成
![image.png](/.attachments/image-250d00d8-d551-4e59-9c33-33e620c9cc17.png)


# 設定私人端點

## 建立私人端點
![image.png](/.attachments/image-804f7e88-059e-4b60-8a09-d8bc174b78b6.png)

## 建立私人DNS區域
注意: 
名稱為: privatelink.{亂數}.database.windows.net
要另外新增一組 private DNS Zone

![image.png](/.attachments/image-23df6520-92aa-4798-84a8-344406c0f9bf.png)

## 測試
![image.png](/.attachments/image-7ea1e1a6-8593-4e3d-888f-454201b5b31d.png)

-----

# 企業防火牆 開通

## 開通 內網連線 - Private Link (最佳方案)
* Private IP
* port: 1433

## 開通 內網連線 - VNET/Subnet
* FQDN : slqmi-sandbox.f5e79f13af60.database.windows.net
* MI 所在的 SUBNET: 10.101.150.128/26
* port: 1433


## 開通 外網連線 - Public Network
<此方案未來不會採用>
* FQDN: slqmi-sandbox.public.f5e79f13af60.database.windows.net
* 外部IP: 4.193.119.184
* port: 3342

內網連線
![image.png](/.attachments/image-52b2adf8-688b-4ba1-83cb-8da3491ceef8.png)

可以跨資料庫查詢、JOIN
![image.png](/.attachments/image-10527ec4-0085-4f00-98d7-0ec91467622b.png)



# 開始 / 停止 服務
新版 SQL MI 提供停止服務，注意: 開始 需要約20分鐘。
![image.png](/.attachments/image-c9f14b5d-b9be-4e78-ad77-de8016b583b2.png)

## 設定排程 自動開始與關閉
![image.png](/.attachments/image-74ef7487-89cc-4abd-8a28-2f37717639a9.png)



# Azure SQL 受控執行個體中的備份還原資料庫

匯出匯入的檔案(bak) 要放在Azure Blob Storage
要注意匯出的檔案，要關閉加密功能
![image.png](/.attachments/image-002d2895-454b-430d-a7fc-0dbdfd240ef8.png)
![image.png](/.attachments/image-67fdc686-f535-4478-950a-45e0e7aa6458.png)

## 還原
將來源檔案放在 Blob Container
![image.png](/.attachments/image-b1040163-5fd6-435a-a6ae-53bb8bb1573f.png)
進行還原作業
![image.png](/.attachments/image-68e6b573-d940-4e99-a2dd-5e9333d876e6.png)
![image.png](/.attachments/image-7d5f324b-a755-42e5-8d5f-19fbe953c967.png)
![image.png](/.attachments/image-d8ac72dd-c4e1-4c1b-a6f6-fbb8e4b67724.png)


---
# 參考文件
## [快速入門：建立 Azure SQL 受控執行個體](https://learn.microsoft.com/zh-tw/azure/azure-sql/managed-instance/instance-create-quickstart?view=azuresql&tabs=azure-portal)

## [Azure SQL 受控執行個體的連線架構](https://learn.microsoft.com/zh-tw/azure/azure-sql/managed-instance/connectivity-architecture-overview?view=azuresql#network-requirements) 

## [必要子網路大小和範圍](https://learn.microsoft.com/zh-tw/azure/azure-sql/managed-instance/vnet-subnet-determine-size?view=azuresql)

## [從 Azure SQL 受控執行個體中的備份還原資料庫](https://learn.microsoft.com/zh-tw/azure/azure-sql/managed-instance/recovery-using-backups?view=azuresql&tabs=azure-portal)

