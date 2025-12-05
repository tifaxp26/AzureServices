

標記策略
最小建議標籤
下列標籤會引導雲端採用架構方法中的實作和流程。 這些方法中的許多最佳做法會根據下列標籤來示範雲端作業自動化和治理。




## 工作負載名稱
資源支援的工作負載名稱。
WorkloadName
- ControlCharts


## 資料分類	此資源所裝載資料的敏感度。	
DataClassification
- Non-business
- Public
- General
- Confidential
- Highly confidential


## 務關鍵性	
資源或所支援工作負載的業務影響。	
重要性
- Low
- Medium
- High
- Business unit-critical
- Mission-critical


## 業務單位	
您公司中擁有該資源所屬訂用帳戶或工作負載的最上層部門。 在小型組織中，此標記可能是單一公司或共用的頂層組織元素。	
BusinessUnit
- Finance
- Marketing
- Product XYZ
- Corp
- Shared


## 作業承諾	
為此工作負載或資源提供的作業支援層級。	
OpsCommitment

- Baseline only
- Enhanced baseline
- Platform operations
- Workload operations



# 其他常見的標記範例
使用下列標籤可提高 Azure 資源使用量的可見度。

## 應用程式名稱	
如果工作負載是在多個應用程式或服務之間進行細分，則會增加資料細微性。	
ApplicationName

- IssueTrackingSystem


## 核准者名稱	
負責核准此資源相關成本的人員。	
Approver

-chris@contoso.com


# 需要的/核准的預算	
針對此應用程式、服務或工作負載核准的金額。	
BudgetAmount

-$200,000

## 成本中心	
與此資源相關聯的會計成本中心。	
CostCenter

- 55332


## 災害復原	
應用程式、工作負載或服務的業務關鍵性。	
DR

- Mission-critical
- Critical
- Essential

## 專案的結束日期	
排定淘汰應用程式、工作負載或服務的日期。	
EndDate
- 2023-10-15


## 環境	
應用程式、工作負載或服務的部署環境。	
Env

- Prod
- Dev
- QA
- Stage
- Test

## 擁有者名稱	
應用程式、工作負載或服務的擁有者。	
擁有者

- jane@contoso.com


## 要求者名稱	
要求建立此應用程式的使用者。	
要求者

- john@contoso.com


## 服務類別	
應用程式、工作負載或服務的服務等級協定層級。	
ServiceClass

- Dev
- Bronze
- Silver
- Gold

## 專案的開始日期	
初次部署應用程式、工作負載或服務的日期。	
StartDate

- 2020-10-15



https://learn.microsoft.com/zh-tw/azure/cloud-adoption-framework/ready/azure-best-practices/resource-tagging