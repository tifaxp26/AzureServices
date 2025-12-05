[[_TOC_]]


# Azure SQL 稽核
Azure SQL 稽核會追蹤資料庫事件，並將其寫入 Azure 儲存體帳戶、Log Analytics 工作區或事件中樞中的稽核記錄。

## 啟用 Azure SQL 稽核
儲存體驗證類型: 受控識別
![image.png](/.attachments/image-cb223c81-85bb-43cf-b9bb-cc10c1b8ea3b.png)


## 儲存體帳戶 存取控制 (IAM)
adlsdataplatform01

儲存體 Blob 資料參與者
![image.png](/.attachments/image-564b430d-a1fe-4ae8-898d-5a8c8c161664.png)
容器: sqldbauditlogs
![image.png](/.attachments/image-48b81ee9-69cc-457c-84cf-85d697cd1e78.png)

## 如何查看 稽核紀錄擋
使用SSMS + Statement
![image.png](/.attachments/image-6da6b4de-9b6c-4cbf-bddf-d9ef42dfa1dc.png)
![image.png](/.attachments/image-7ad2457a-5e9c-4909-9866-7f59275bc771.png)

```
SELECT *
FROM sys.fn_get_audit_file(
    'https://adlsdataplatform01.blob.core.windows.net/sqldbauditlogs/sql-dataplatform-prod-01/master/SqlDbAuditing_ServerAudit_NoRetention/2025-04-15/03_00_01_082_436.xel',
    DEFAULT,
    DEFAULT
);
```



```
SELECT *
FROM sys. fn_get_audit_file_v2(
    'https://<storage_account>.blob.core.windows.net/sqldbauditlogs/server_name/database_name/SqlDbAuditing_ServerAudit/',
    DEFAULT,
    DEFAULT,
    '2024-11-17T08:40:40Z',
    '2024-11-17T09:10:40Z')

sys.fn_get_audit_file_v2 (Transact-SQL) - SQL Server | Microsoft Learn
```


example
```
SELECT TOP 10 *
FROM sys.fn_get_audit_file(
    'https://mystorage.blob.core.windows.net/sqldbauditlogs/ShiraServer/MayaDB/SqlDbAuditing_Audit/2017-07-14/10_45_22_173_1.xel',
    DEFAULT,
    DEFAULT
)
WHERE server_principal_name = 'admin1'
ORDER BY event_time;
GO
```

---

# 資料庫匯出/匯入
## 必要角色
該資源群組
- SQL Server 參與者
- SQL DB 參與者
- 網路參與者 (可略)
- 儲存體帳戶參與者

![image.png](/.attachments/image-c80a3c56-bd88-4e09-a490-282fe61cafd4.png)



## Blob Storage Network
因服務封鎖在內部網路中，必須開啟 Public network access、Enable from selected networks
![image.png](/.attachments/image-a2cafc5f-a2e0-479a-a757-7cb48e4c43fc.png)
![image.png](/.attachments/image-2209608f-f75f-42ae-83d1-dd79a865ac90.png)



## 資料庫匯出作業
輸入匯出名稱
選擇目的端訂閱帳戶
勾選 使用私人連線
選擇 目的端Blob Storage
輸入資料庫登入帳密
![image.png](/.attachments/image-2cc15fa7-8491-4cd4-9de8-785ca7b040b1.png)

## Approval Blob Private Link
系統會在 Blob Storage 建立 Private Link，進行繪出檔案存放的地方。
需要同意後才會進行作業。
![image.png](/.attachments/image-9418bc43-31d5-4633-8ef3-216b1133ab0a.png)

## Approval SQL Private Link
![image.png](/.attachments/image-6170fd96-2244-4cf7-9fa3-f32d71884bff.png)


## 查看匯出/入紀錄
到 SQL Server > 資料管理 > 匯出/入紀錄
![image.png](/.attachments/image-937b082d-c6af-43a5-80b8-d9527a52f468.png)


## 使用 AZ Cloud Shell 刪除匯出入作業

- 步驟一
AZ Portal 開啟 Cloud Shell
並取得訂閱授權
![image.png](/.attachments/image-adb1d585-b8cd-46e3-8d80-837f52eb011e.png)

- 步驟二
```
function Cancel-AzSQLImportExportOperation
{
    param
    (
        [parameter(Mandatory=$true)][string]$ResourceGroupName
        ,[parameter(Mandatory=$true)][string]$ServerName
        ,[parameter(Mandatory=$true)][string]$DatabaseName
    )

    $Operation = Get-AzSqlDatabaseActivity -ResourceGroupName $ResourceGroupName -ServerName $ServerName -DatabaseName $DatabaseName | Where-Object {($_.Operation -like "Export*" -or $_.Operation -like "Import*") -and $_.State -eq "InProgress"}
    
    if(-not [string]::IsNullOrEmpty($Operation))
    {
        do
        {
            Write-Host -ForegroundColor Cyan ("Operation " + $Operation.Operation + " with OperationID: " + $Operation.OperationId + " is now " + $Operation.State)
            $UserInput = Read-Host -Prompt "Should I cancel this operation? (Y/N)"
        } while($UserInput -ne "Y" -and $UserInput -ne "N")

        if($UserInput -eq "Y")
        { 
            "Canceling operation"
            Stop-AzSqlDatabaseActivity -ResourceGroupName $ResourceGroupName -ServerName $ServerName -DatabaseName $DatabaseName -OperationId $Operation.OperationId
        }
        else 
        {"Exiting without cenceling the operation"}
        
    }
    else
    {
        "No import or export operation is now running"
    }
}
```

- 步驟三
```
Cancel-AzSQLImportExportOperation
```

## 資料庫匯入作業

選來源端 Blob 備份檔案
勾選私人連結
資料庫名稱
定序: Chinese_Taiwan_Stroke_CI_AS
![image.png](/.attachments/image-396f8626-11fa-4a44-85eb-64885d122ee3.png)

進行部署
![image.png](/.attachments/image-f0871b1b-3cb8-45b1-b5db-7a1b126f89b4.png)

依樣要同意 Blob/SQL Private Link
![image.png](/.attachments/image-b5878c6a-24f1-4b20-b397-12216e257495.png)
![image.png](/.attachments/image-04bd0540-70e8-4b46-be66-84e4dfe61593.png)

匯入完成
![image.png](/.attachments/image-fa0e3b40-4204-4900-b383-a66d5b2c4a8e.png)



-------------------------------
# 參考
- https://learn.microsoft.com/en-us/sql/relational-databases/system-functions/sys-fn-get-audit-file-transact-sql?view=sql-server-ver16&tabs=sqldb
