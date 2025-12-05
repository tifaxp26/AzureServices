[[_TOC_]]


# 監視 AKS 資料參考

當您有依賴 Azure 資源的重要應用程式和商務程序時，您會想要監視這些資源的可用性、效能和操作。 本文說明 AKS 所產生的監視資料，並使用 Azure 監視器 進行分析 。 Azure 監視器為所有使用到 Azure 監視器的 Azure 服務提供通用功能，若您對這些功能不甚熟悉，請參閱使用 Azure 監視器監視 Azure 資源。



# 監視資料
AKS 會產生與從 Azure 資源監視資料中所述 的其他 Azure 資源相同的監視資料類型。 
如需 AKS 所建立計量和記錄的詳細資訊，請參閱 監視 AKS 資料參考 。 
其他 Azure 服務和功能 將會收集其他資料，並啟用其他分析選項，如下圖所示。

<IMG  src="https://learn.microsoft.com/zh-tw/azure/aks/media/monitor-aks/aks-monitor-data.png"  alt="Diagram of collection of monitoring data from AKS."/>


<br>
<br>


| 來源 | 描述 |
|--|--|
| 平台計量 | 系統會自動針對 AKS 叢集收集平臺計量 ，不收費。 您可以使用計量總 管來分析這些計量 ，或將其用於 計量警示 。 |
| Prometheus 計量 | 當您 為叢集啟用計量擷 取時， Prometheus 計量 是由 適用于 Prometheus 的 Azure 監視器受控服務所收集，並儲存在 Azure 監視器工作區 中 。 使用 Azure 受控 Grafana 和 Prometheus 警示 中 預先建置的儀表板 來分析它們。 |
| 活動記錄 | 系統會自動針對 AKS 叢集收集活動記錄 檔，而不需要任何費用。 這些記錄會追蹤資訊，例如建立叢集或有設定變更的時間。 將 活動記錄傳送至 Log Analytics 工作區 ，以與其他記錄資料一起分析。 |
| 資源記錄 | AKS 的控制平面記錄 會實作為資源記錄。 建立診斷設定 ，以將它們傳送至 Log Analytics 工作區 ，您可以在其中使用 Log Analytics 中的 記錄查詢來分析和警示它們。 |
| 容器深入解析 | 容器深入解析會從叢集收集各種記錄和效能資料，包括 stdout/stderr 資料流程，並將其儲存在 Log Analytics 工作區 和 Azure 監視器計量 中。 使用容器深入解析隨附的檢視和活頁簿，或使用 Log Analytics 和 計量總管 來分析此資料。 |
|  |  |




# Azure 入口網站中的 [監視] 概觀頁面
[ 概觀] 頁面上的 [ 監視 ] 索引標籤可讓您快速開始檢視每個 AKS 叢集中Azure 入口網站中的監視資料。 這包括具有節點集區分隔之叢集通用計量的圖表。 按一下上述任何圖表，以進一步分析計量總管 中的資料 。

[概 觀] 頁面也包含目前 叢集受控 Prometheus 和 容器深入解析 的連結。 如果您尚未啟用這些工具，系統會提示您這麼做。 您也可以在畫面頂端看到橫幅，建議您啟用其他功能來改善叢集的監視。

<IMG  src="https://learn.microsoft.com/zh-tw/azure/aks/media/monitor-aks/overview.png"  alt="Screenshot of AKS overview page."/>


# 資源記錄
AKS 叢集的控制平面記錄會實作為 Azure 監視器中的資源記錄 。 
在您建立診斷設定以將記錄路由傳送至一或多個位置之前，不會收集並儲存資源記錄。 
您通常會將它們傳送至 Log Analytics 工作區，這是儲存容器深入解析的大部分資料的地方。
<IMG  src="https://learn.microsoft.com/zh-tw/azure/aks/media/monitor-aks/diagnostic-setting-categories.png"  alt="Screenshot of AKS diagnostic setting dialog box."/>

AKS 支援 Azure 診斷模式 或 資源記錄的資源特定模式 。 這會指定 Log Analytics 工作區中傳送資料的資料表。 Azure 診斷模式會將所有資料傳送至 AzureDiagnostics 資料表 ，而資源特定模式會將資料傳送至 AKS 稽 核、 AKS 稽核管理員 和 AKS 控制平面 ，如資源記錄 的 資料表所示。

基於下列原因，建議使用資源特定的模式：AKS：

- 資料更容易查詢，因為它位於 AKS 專用的個別資料表中。
- 支援設定為 基本記錄 ，以節省大量成本。



#範例記錄查詢

重要

當您從 AKS 叢集的功能表中選取 [記錄 ] 時，Log Analytics 會開啟，並將查詢範圍設定為目前的叢集。 
這表示記錄查詢只會包含來自該資源的資料。 
如果您想要執行包含其他叢集資料或來自其他 Azure 服務之資料的查詢，請從 [Azure 監視器 ] 功能表選取 [ 記錄 ]。 
如需詳細資訊，請參閱 Azure 監視器 Log Analytics 中的記錄查詢範圍和時間範圍。

如果叢集的 診斷設定使用 Azure 診斷模式，AKS 的資源記錄會儲存在 AzureDiagnostics 資料表中 。 
您可以使用 Category 資料行來區分不同的記錄 。 
如需每個類別的描述，請參閱 AKS 參考資源記錄 。


描述	記錄查詢
- 計算每個類別的記錄（Azure 診斷模式）	
AzureDiagnostics | where ResourceType == 「MANAGEDCLUSTERS」 | summarize count（） by Category
- 所有 API 伺服器記錄（Azure 診斷模式）	
AzureDiagnostics | where Category == 「kube-apiserver」
- 時間範圍內的所有 kube-audit 記錄（Azure 診斷模式）	
let starttime = datetime（「2023-02-23」）：
let endtime = datetime（「2023-02-24」）：
AzureDiagnostics
 | 其中 TimeGenerated between（starttime..endtime）
 | where Category == 「kube-audit」
 | extend event = parse_json（log_s）
 | extend HttpMethod = tostring（event.verb）
 | extend User = tostring（event.user.username）
 | extend Apiserver = pod_s
 | extend SourceIP = tostring（event.sourceIPs[0]）
 | project TimeGenerated， Category， HttpMethod， User， Apiserver， SourceIP， OperationName， event
- 所有稽核記錄（資源特定模式）	
AKSAudit
- 排除取得和列出稽核事件的所有稽核記錄（資源特定模式）	
AKSAudit管理員
- 所有 API 伺服器記錄（資源特定模式）	
AKSControlPlane | where Category == 「kube-apiserver」



# 警示
在監視資料中發現重大狀況時，Azure 監視器會主動通知您。 如此便能在您的客戶注意到之前，先在您的系統中識別問題並加以對應。 可在 [計量]、[記錄]、[活動記錄] 中設定警示。 不同類型的警示各有優缺點。

# 計量警示
下表列出 AKS 叢集的建議計量警示規則。 您可以選擇在建立叢集時自動啟用這些警示規則。 這些警示是以叢集的平臺計量 為基礎 。

條件	描述
- CPU 使用量百分比 > 95	
  在所有節點的平均 CPU 使用量超過閾值時引發。
- 記憶體工作集百分比 > 100	
  在所有節點的平均工作集超過閾值時引發。


# 停用監控(Prometheus)
`az aks update --disable-azure-monitor-metrics -n WalsinAKSTest01 -g rg-walsin-development`

<br>

# Disable Container insights
`az aks disable-addons -a monitoring -n AZ-AKSP01 -g rg-l5-prod`

<br>


# 停用 Kubernetes 叢集的監視
https://learn.microsoft.com/zh-tw/azure/azure-monitor/containers/kubernetes-monitoring-disable?tabs=cli



# Container Log 瘦身
## 資料收集規則
找到 AKS 資料收集規則
![image.png](/.attachments/image-cb4eb7e3-27fc-482d-b139-06bd69108d46.png)

## 匯出範本
![image.png](/.attachments/image-e7638451-c6fb-4f3f-8c46-c54709318ae8.png)
![image.png](/.attachments/image-ee4d0737-050a-4711-ac0d-86f7310d2197.png)

## 編輯範本

```
,
{
	"streams": [
		"Microsoft-ContainerLogV2"
	],
	"destinations": [
		"ciworkspace"
	],
	"transformKql": "source | where ContainerName !in ('controller', 'l5dip-portal', 'l5dip-dataverifymodule', 'l5dip-workermanager', 'l5dip-workermodule', 'l5dip-myjobmodule', 'l5dip-formdesign-mongo')"
}
```

![image.png](/.attachments/image-27cca954-b573-4fed-bbda-811ecf23317a.png)

## 檢查
確認資料量已排除 寫入。
```
ContainerLogV2
| where TimeGenerated > datetime("2025-05-05 05:20:00") and TimeGenerated <= datetime("2025-05-05 06:20:00")
| where ContainerName in ('controller', 'l5dip-portal', 'l5dip-dataverifymodule', 'l5dip-workermanager', 'l5dip-workermodule', 'l5dip-myjobmodule', 'l5dip-formdesign-mongo')
| summarize count() by bin(TimeGenerated,10m)
| order by TimeGenerated
```

![image.png](/.attachments/image-fa282d6b-b032-46a4-ba07-d2be37ff0ef5.png)


## 變更 資料表方案
分析 => 基本
![image.png](/.attachments/image-e60d2a2d-16df-426e-85e0-b82ed4aaf7bd.png)

費用差很多
![image.png](/.attachments/image-e9ba994e-0a71-4cb6-9301-50311a532c46.png)


# 參考
https://learn.microsoft.com/zh-tw/azure/aks/monitor-aks

