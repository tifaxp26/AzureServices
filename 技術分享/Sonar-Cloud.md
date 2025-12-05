[[_TOC_]]

# 說明
靜態診斷分析器 SonarCloud

SonarCloud 是雲端式程式碼品質與安全性服務。 SonarCloud 的主要功能包括：

支援 23 種程式設計與指令碼語言，包括 JAVA、JS、C#、C/C++、Objective-C、TypeScript、Python、ABAP、PLSQL 與 T-SQL。
根據功能強大的靜態程式碼分析器，有數千個規則可追蹤難以尋找的 Bug 與品質問題。
雲端式與熱門 CI 服務的整合，包括 Travis、Azure DevOps、BitBucket 與 AppVeyor。
深入的程式碼分析，以探索分支與提取要求中的所有來源檔案，協助達到綠色品質閘道並提升組建。
速度與可擴縮性。


# Sonar Cloud

## 註冊 Sonar Cloud
官網: https://sonarcloud.io/
![image.png](/.attachments/image-fe2d2a2f-7b6d-4769-b19a-888afd50eb76.png)

## 建立組織
- 組織名稱: WalsinTeam
- 設定 Azure DevOps PAT
![image.png](/.attachments/image-d6c9d965-4227-43a3-9b6c-d09da6828f58.png)

## 建立Key
- WalsinTeam
![image.png](/.attachments/image-76dbffd5-03c4-439a-8717-b6550a204d72.png)

## 選擇計畫
- Free Plan
![image.png](/.attachments/image-2be983a2-35f6-4ba5-8251-7755af89d2b4.png)




# Azure DevOps
## Azure Pipeline 建立CI
1. 新增服務
![image.png](/.attachments/image-849d40ab-f1a2-4a2d-a210-ad8b5049d636.png)

1. 新增 job: Prepare analysis on SonarCloud
![image.png](/.attachments/image-7d6556d2-b2c1-4649-b010-ccca60f49936.png)

1. 新增 job: Run Code Analysis
![image.png](/.attachments/image-a7819636-3574-4a61-8f8a-a48e0329f3ef.png)

1. 測試
![image.png](/.attachments/image-22ca7201-5c5a-47b8-b56c-9758860f4058.png)
![image.png](/.attachments/image-835a442d-abcf-4fe7-a5c8-ad155508aa08.png)


# Report
![image.png](/.attachments/image-60da3a71-ff98-40fc-be42-43c4edf246ff.png)


# 注意事項
## 掃描的分支需要是 default branch
![image.png](/.attachments/image-9d2ac400-fef6-4e43-9e30-69723dc14dc9.png)

# 參考
https://learn.microsoft.com/zh-tw/training/modules/static-analyzers/4-manage-technical-debt-sonarcloud-azure-devops