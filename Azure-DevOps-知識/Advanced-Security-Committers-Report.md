[[_TOC_]]


利用 Graph API 取得 Azure DevOps Repos 的Commit 紀錄、Commit 人員 等資訊



# Security
使用Basic Token 驗證方式

## 取得 Azure DevOps PAT
![image.png](/.attachments/image-2b2070aa-e758-4e93-ba48-4e365bc7a9e4.png)

## 使用Postman測試
輸入 UserName / Password
![image.png](/.attachments/image-4414d278-d99e-4f56-867e-07edecbbd336.png)


# Commits - Get Commits

https://learn.microsoft.com/en-us/rest/api/azure/devops/git/commits/get-commits?view=azure-devops-rest-7.1&tabs=HTTP



# 製作Power BI



Power Query

![image.png](/.attachments/image-abf05783-07bf-4dc4-b3f1-a13646e1cb46.png)

```
let   
    // 參數設置
    organization = "WalsinDigitalInformationCenter", // 替換為你的實際參數值
    project = "L5DIP", // 替換為你的實際參數值
    repositoryId = "L5DIP.DataVerifyModule", // 替換為你的實際參數值
    
    // 獲取當前日期
    currentDate = DateTime.LocalNow(),
    
    // 計算三個月前的日期
    startDate = Date.ToText(Date.AddMonths(Date.From(currentDate), -3), "yyyy-MM-dd"),
    
    // 設定結束日期為今天
    endDate = Date.ToText(Date.From(currentDate), "yyyy-MM-dd"),
    
    // 設定 API URL，包含時間區間
    url = Text.Format("https://dev.azure.com/#{0}/#{1}/_apis/git/repositories/#{2}/commits?searchCriteria.fromDate=#{3}&searchCriteria.toDate=#{4}&api-version=7.1-preview.1", {organization, project, repositoryId, startDate, endDate}),
    
    // 發送請求並獲取 JSON 數據
    Source = Json.Document(Web.Contents(url)),


    ResultData = if  Source[count] > 0 then 
        let
            // 將 JSON 轉換為表格（假設 API 返回的 JSON 是一個對象，其中包含一個名為 'value' 的陣列）
            commits = Source[value],            
            // 將數據展開為表格形式
            Table = Table.FromList(commits, Splitter.SplitByNothing(), null, null, ExtraValues.Error),  
            ExpandedTable = Table.ExpandRecordColumn(Table, "Column1", {"committer", "comment", "committerDate"}),
            // 進一步展開 committer 欄位
            ExpandedCommitter = Table.ExpandRecordColumn(ExpandedTable, "committer", {"name", "email", "date"})
        in 
            ExpandedCommitter

        else
            #table({"name", "email", "date", "comment", "committerDate"}, {})
in
    ResultData
```







