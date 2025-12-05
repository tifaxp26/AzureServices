[[_TOC_]]

# 摘要

**Shift-Left Security**
=======================
![Shift-Left Security](https://miro.medium.com/v2/resize:fit:700/1*Hm2fGzgNAcuej0T_KnlbTw.png)

適用於 Azure DevOps 的 GitHub 進階安全性會將 GitHub 進階安全性功能套件新增至 Azure Repos。

適用於 Azure 的 GitHub 進階安全性包括：
- 秘密掃描推播保護：檢查程式代碼推送是否包含公開秘密的認可，例如認證
- 秘密掃描存放庫掃描：掃描您的存放庫，並尋找意外認可的公開秘密
- 相依性掃描 – 搜尋 開放原始碼 相依性中的已知弱點（直接和可轉移）
- 程式代碼掃描 – 使用 CodeQL 靜態分析引擎來識別程式代碼層級的應用程式弱點，例如 SQL 插入式和驗證略過


![image.png](/.attachments/image-93c81edb-5c0e-4e89-ac31-0d2adfda8314.png)



依下列順序新增工作：

1. Advanced Security Initialize CodeQL
1. Advanced Security AutoBuild （語言相依） 或以您自己的自訂建置步驟取代此專案
1. Advanced Security Perform CodeQL Analysis

<IMG  src="https://learn.microsoft.com/zh-tw/azure/devops/repos/security/media/code-scanning-config-classic-tasks.png?view=azure-devops#lightbox"/>

此外，您必須指定您在初始化程式碼QL 工作中要分析的語言。 如果指定的語言為 cpp, java, csharp 或 swift, 自訂或 AutoBuild 建置步驟，則為必要專案。 若為其他語言，如果包含 AutoBuild，則步驟會順利完成，而不需要執行任何動作。

若要產生警示，請使用包含程式碼掃描工作的管線執行您的第一次掃描。

# 進階安全性計費
若要存取結果並使用azure DevOps 功能的GitHub Advanced Security，您需要授權。 每個作用中認可者至少一個已啟用進階安全性的存放庫會耗用一個授權， 每個作用中認可者每個月的成本為 $49美元。 
**如果認可者在過去 90 天內已將程式碼認可至存放庫，則會被視為作用中。**

進階安全性會直接計費至與 Azure DevOps 組織相關聯的 Azure 訂用帳戶。 帳單是每月計量。 每日費用會根據組織中每天的作用中認可者總數，發出給您的 Azure 訂用帳戶。

主動認可者會在 Azure 訂用帳戶之間重復資料刪除。 只要這些組織與相同的 Azure 訂用帳戶相關聯，使用者就可以參與多個存放庫或組織。

![image.png](/.attachments/image-f63c10c5-81fb-4829-a68a-586a5eb15cba.png)


<hr/>

# 掃描功能說明

## 祕密掃描

工程系統中公開的認證可為攻擊者提供容易利用的機會。 為了抵禦此威脅， 適用於 Azure DevOps 的 GitHub 進階安全性工具會掃描原始碼中的認證和其他敏感性內容。 推送保護也可防止任何認證在一開始外洩。

儲存機制的秘密掃描會掃描歷程記錄中可能已存在於原始程式碼中的任何秘密，並推送保護可防止任何新的秘密在原始程式碼中公開。

適用於 Azure DevOps 的 GitHub 進階安全性可與 Azure Repos 搭配運作。 如果您想要搭配 GitHub 存放庫使用 GitHub 進階安全性，請參閱 GitHub 進階安全性。

### 關於秘密掃描警示
啟用進階安全性時，它會掃描存放庫中是否有各種服務提供者所發出的秘密，併產生秘密掃描警示。

如果存取資源需要配對的認證，則只有在相同檔案中偵測到配對的兩個部分時，秘密掃描才會建立警示。 配對可確保最重要的洩漏不會隱藏在部分洩漏的相關信息後方。 配對比對也有助於減少誤判，因為配對的兩個元素都必須一起使用才能存取提供者的資源。

Azure DevOps 中 Repos>進階安全性的 [進階安全性] 索引卷標是檢視安全性警示的中樞。 選取 [ 秘密] 索引 標籤以檢視秘密掃描警示。 您可以依狀態和秘密類型進行篩選。 您可以流覽至警示以取得詳細數據，包括補救指引。 啟用進階安全性之後，選取的存放庫就會開始掃描，包括所有歷程記錄認可。 經過一段時間后，警示就會在掃描進行時開始出現。

如果已重新命名分支，則結果不會有任何影響，最多可能需要 24 小時才會顯示新名稱。
<IMG  src="https://learn.microsoft.com/zh-tw/azure/devops/repos/security/media/secret-scanning-alerts.png?view=azure-devops"  alt="Screenshot showing active secret scanning alerts"/>

若要補救公開的秘密，請使公開的認證失效，並在其位置建立新的認證。 然後，應該安全地儲存新建立的秘密，以不直接將它推送回程式代碼的方式。 例如，秘密可以儲存在 Azure 金鑰保存庫。 大部分的資源都有主要和次要認證。 除非另有說明，否則將主要認證變換至次要認證的方法完全相同。


## 相依性掃描
GitHub Advanced Security for Azure DevOps 中的 相依性掃描會偵測原始程式碼中使用的開放原始碼元件，並偵測是否有任何相關聯的弱點。 來自開放原始碼元件的任何發現弱點會標示為警示。

適用于 Azure DevOps 的 GitHub 進階安全性可與 Azure Repos 搭配運作。 如果您想要搭配 GitHub 存放庫使用 GitHub 進階安全性，請參閱 GitHub 進階安全性 。

### 關於相依性掃描
相依性掃描會產生任何開放原始碼元件、直接或可轉移的警示，發現程式碼所依賴的弱點。 直接弱點是您程式碼直接使用的程式庫。 可轉移的相依性是直接相依性所使用的程式庫或其他軟體。

### 關於相依性掃描偵測
每當存放庫的相依性圖形變更，以及在包含建置新程式碼之相依性掃描工作的管線之後，就會儲存元件的新快照集。

針對使用中偵測到的每個易受攻擊元件，元件和弱點都會列在組建記錄中，並在 [進階安全性] 索引標籤中顯示為警示。只有 GitHub 檢閱並新增至 GitHub Advisory Database 的 諮詢會建立相依性掃描警示。 組建記錄包含個別警示的連結，以供進一步調查。 如需警示詳細資料的詳細資訊，請檢視修正相依性掃描警示。

組建記錄檔也包含每個偵測到弱點的基本資訊。 這些詳細資料包括嚴重性、受影響的元件、弱點的標題，以及相關聯的 CVE。

<IMG  src="https://learn.microsoft.com/zh-tw/azure/devops/repos/security/media/dependency-scanning-build-log.png?view=azure-devops"  alt="Screenshot of a dependency scanning build output"/>


## 程式碼掃描

GitHub Advanced Security for Azure DevOps 中的 程式代碼掃描可讓您分析 Azure DevOps 存放庫中的程序代碼，以找出安全性弱點和編碼錯誤。 會以警示形式提出分析所識別的任何問題。 程式碼掃描會使用 CodeQL 識別弱點。

CodeQL 是由 GitHub 所開發的程式碼分析引擎，可讓安全性檢查自動化。 您可以使用 CodeQL 分析程式碼，並以程式碼掃描警示型態顯示結果。

CodeQL 警示
GitHub 專家、安全性研究人員和社群參與者會撰寫和維護用於程式代碼掃描的預設 CodeQL 查詢。 查詢會定期更新，以改善分析並減少任何誤判結果。 查詢 開放原始碼，因此您可以檢視並參與 github/codeql 存放庫中的查詢。

如需有關 CodeQL 的更具體檔，請流覽 GitHub 上的 CodeQL 檔。


### 內建 CodeQL 查詢套件

GitHub 提供並維護幾種標準的內建查詢套件，可供所有支援 CodeQL 的語言使用。 

*   **`default` (預設)**：這是 GitHub 程式碼掃描 (Code Scanning) 的預設套件。它包含精選的高精確度 (high-precision) 安全性查詢，旨在將誤報 (false positives) 降至最低，通常只會報告嚴重性較高或確定性較高的問題。
*   **`security-extended` (擴充安全性)**：此套件包含 `default` 套件中的所有查詢，並增加了其他查詢。這些額外的查詢可能具有稍微低一點的精確度或嚴重性，但能涵蓋更廣泛的安全問題。在 GitHub 介面中，此套件通常被稱為「Extended」套件。
*   **`security-and-quality` (安全與品質)**：這個套件在 `security-extended` 的基礎上，進一步包含了針對程式碼可維護性和可靠性方面的額外查詢。

### CodeQL 同時支援編譯和解譯的語言，而且可以在以支援的語言撰寫的程式代碼中找到弱點和錯誤。

- C/C++
- C#
- Go
- Java
- JavaScript/TypeScript
- 科特林 （beta）
- Python
- Ruby
- Swift

<IMG  src="https://learn.microsoft.com/zh-tw/azure/devops/repos/security/media/code-scanning-build-log.png?view=azure-devops"  alt="Screenshot of code scanning publish results task"/>

### 程式碼掃描警示
適用於 Azure DevOps 程式代碼掃描警示的 GitHub 進階安全性包含程式碼層級應用程式弱點警示的存放庫的程式代碼掃描旗標。

若要使用程式代碼掃描，您必須先設定 Azure DevOps 的 GitHub 進階安全性。

Azure DevOps 中 Repos 底下的 [進階安全性] 索引標籤是用來檢視程式代碼掃描警示的中樞。 選取 [ 程序代碼掃描] 索引 標籤以檢視掃描警示。 您可以依分支、狀態、管線、規則類型和嚴重性進行篩選。

如果重新命名管線或分支，則結果不會有任何影響，最多可能需要 24 小時才會顯示新名稱。
<IMG  src="https://learn.microsoft.com/zh-tw/azure/devops/repos/security/media/code-scanning-alerts.png?view=azure-devops"  alt="Screenshot of code scanning alerts for a repository"/>


<hr>

# Advanced Security 結果報告
## Dependencies OpenSource元件檢查

Dependencies著重於識別專案依賴項（主要是開源依賴項）中的漏洞。 它會尋找這些依賴項中的已知安全性問題並發出警報，以便您可以更新或取代它們。 警報包括有關易受攻擊的軟體包的資訊、導致漏洞的根依賴關係，以及有關如何解決該漏洞的指南。 此掃描涵蓋直接依賴項（儲存庫中明確包含的元件）和傳遞依賴項（直接依賴項使用的元件）。 問題的嚴重性根據 GitHub Advisory Database 提供的 CVSS 分數進行分級，將其分類為低、中、高或嚴重。Azure DevOps 的 GitHub 進階安全性相依性掃描 - Azure Repos | Microsoft Learn
Azure DevOps 的 GitHub 進階安全性相依性掃描 - Azure Repos
為 Azure DevOps 設定 GitHub 進階安全性的相依性掃描

![image.png](/.attachments/image-9c42b55f-dc35-43ac-87dd-86313fcdee6e.png)

## Code scanning 靜態分析，弱點檢查
code scanning直接針對專案的原始程式碼，搜尋潛在的安全漏洞和編碼錯誤。 它使用 CodeQL 等工具來找出問題，並提供有關每個警報的詳細信息，包括程式碼中的位置、問題描述、漏洞範例和建議的修正步驟。 針對違反特定規則的每個程式碼實例產生警報，並提供嚴重性級別，範圍從低到嚴重。 這有助於確定需要立即關注的問題的優先順序。適用於 Azure DevOps 的 GitHub 進階安全性程式代碼掃描警示

![image.png](/.attachments/image-fd0cc243-21e8-4237-a9f1-ef7b6aef7ff8.png)

## Secrets 敏感資料檢查
![image.png](/.attachments/image-dda87217-0687-46bf-9f09-e1483c02a283.png)


## Dependencies (OpenSource Scan)

- Result
![image.png](/.attachments/image-ac5629f1-d6d8-4f5c-9002-9c0578b7d0c8.png)

- Overview
![image.png](/.attachments/image-50826af5-8447-4f89-9370-3998bac6ca89.png)

- Detections
![image.png](/.attachments/image-f748680e-fb29-49f2-8ef1-eaf64e3ff706.png)

## Code scanning
- Result
![image.png](/.attachments/image-d8cf90b5-5164-4762-9c59-02d3815d6a9c.png)
- Overview
![image.png](/.attachments/image-1959f945-050c-4588-bd82-fa2893adeb72.png)

## Secrets
- Result
![image.png](/.attachments/image-25e7550c-abe3-4107-ab21-7592f1d30505.png)
- Overview
![image.png](/.attachments/image-0a7ea1c8-242c-4249-8e87-849ca7553041.png)


<hr />

# 實作
## 如何設定
以 DataPlatform 為例:
選擇 code-scanning
![image.png](/.attachments/image-56b89ce0-9f06-43af-8235-3c02cf5a92e9.png)

## 檢測結過
紀錄在 Advance Security 畫面中
![image.png](/.attachments/image-5d2f30e3-fb6d-4450-8f02-17fd56d5bc48.png)

# 進階設定 - Library
## 變數群組
![image.png](/.attachments/image-644050ca-3cad-4e92-8a92-5768e30c8eb8.png)
![image.png](/.attachments/image-a29d245e-78dd-4d29-bcd8-656e89fb5c24.png)




# 完整pipeline 設定
![image.png](/.attachments/image-0938c29d-42f9-4ce0-8fe8-d96fc3763a81.png)
![image.png](/.attachments/image-a8e1ec21-5d52-4cd6-b8d6-02a9c96f9df0.png)


# 匯出報告 csv
參考套件
1. https://github.com/microsoft/GHAzDO-Resources/blob/main/src/csv-report/README.md
1. 新增檔案 ghazdo-csv-report.ps1
1. pipelines 新增 job

## 安全檔案
將 ghazdo-csv-report.ps1 放到 Secure Files
![image.png](/.attachments/image-4edfc858-04c4-4f31-adc4-af7b94b0468b.png)


## 產生PAT
由於跨專案，需要 PAT 授權
btain a Personal Access Token (PAT) with the following scopes:
- Advanced Security - Read
- Code - Read (needed by default ... for Scope= organization or project lookup of repository list )
- Projects - Read (for Scope= organization)

## 參數
`-pat "yazmk7hc7gmtdob5tzrwhrdur5ivjcjm2dzqs----------" -orgUri "https://dev.azure.com/WalsinTeam" -project "WalsinAdvancedSecurity" -repository "BaseDefender" -reportName "ghazdo-report-$(Get-Date -Format "yyyyMMdd").1.csv"`
![image.png](/.attachments/image-0e3b1cec-139d-4def-ad7c-e67e629cdb77.png)

## CI success & publish artifacts
![image.png](/.attachments/image-3e931ef8-ca7a-4f95-b36e-76dc8ae69309.png)

## how to download files
publish artifacts
到 summary panel > Relate infomation
![image.png](/.attachments/image-494eee1a-0a4f-4c65-97c5-af5c51fb9075.png)
![image.png](/.attachments/image-d47a9b31-0261-4d2a-964e-31f3d2455eb3.png)


## Email 通知+附件
由於 email task 需要 run on windows，
因此新增 pipeline 銜接上一步驟。
![image.png](/.attachments/image-61484219-cab4-49c0-88af-949af39a3b24.png)

採用 Azure Community Services 做 mail 通知
![image.png](/.attachments/image-98f92094-9c47-46bd-890c-4663cb8b5d49.png)



# 顯示 commiters
Connect to Remote Git Repository
git init .
git remote add origin 你的信箱@https://WalsinTeam@dev.azure.com/WalsinTeam/DataPlatform/_git/Walsin.DataPlatform

`git shortlog -sne --all --since="2024-05-12" --until="2024-08-12"`



# 參考
- https://learn.microsoft.com/zh-tw/azure/devops/repos/security/configure-github-advanced-security-features?view=azure-devops&tabs=classic

- [Walsin GitHub Advanced Security on Azure DevOps.pdf](/.attachments/Walsin%20GitHub%20Advanced%20Security%20on%20Azure%20DevOps-4f7029d2-3357-45ba-8f1f-b1f8cf795978.pdf)

- https://ithelp.ithome.com.tw/articles/10335777

- https://docs.github.com/en/code-security/code-scanning/managing-your-code-scanning-configuration/codeql-query-suites

- https://github.com/microsoft/GHAzDO-Resources/blob/main/src/csv-report/README.md
