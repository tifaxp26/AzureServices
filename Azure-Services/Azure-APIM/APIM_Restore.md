[[_TOC_]]

# 說明
APIM 刪除後，雖然可以從Portal restore，
但是 AAD 中的企業應用程式 卻無法自動還原，
且Portal也查找不到刪除紀錄，
只能執行 Script 查找與還原。

**必須角色為: "全域管理員"**

API物件 (主體) 識別碼: 61c7f0b6-eacb-4454-a6e2-91f5d029bf68

使用 Azure CLI - PowerShell

# 【方式一: Restore-AzureADMSDeletedDirectoryObject】
Import-Module AzureAD
Install-Module AzureAD
Connect-AzureAD
Get-AzureADMSDeletedDirectoryObject -Id <id>
Restore-AzureADMSDeletedDirectoryObject -Id <id>


# 【方式二: connect-MgGraph】
connect-MgGraph -Scopes "Application.ReadWrite.All"
Get-MgDirectoryDeletedItem -DirectoryObjectId <id>
Restore-MgDirectoryDeletedItem -DirectoryObjectId <id>

![image.png](/.attachments/image-6fe608e8-884e-410f-99f5-52c4ac7aa8d9.png)
![image.png](/.attachments/image-d475446d-a06a-4364-bb22-89c39d5b1773.png)


# 【方式三: graph-explorer】
https://graph.microsoft.com/v1.0/directory/deletedItems/<id>/restore

![image.png](/.attachments/image-8614faa2-5e45-4cd3-acc9-47431b3930e3.png)


但是...
根據官方文件說明，表示服務是不可被刪除，
疑似MS服務更新 造成異常...
![image.png](/.attachments/image-d1063f44-9285-4868-852f-e21c96e7231c.png)
目前即使是全域管理員也不能還原服務了
https://learn.microsoft.com/en-us/troubleshoot/azure/entra/entra-id/app-integration/unable-delete-app


# 使用者自訂受控身分識別
採用 使用者自訂受控身分識別
建立 受控識別
![image.png](/.attachments/image-45380dd1-0c3d-497a-882f-88dba7a0caaf.png)

設定 APIM 使用者受控識別
![image.png](/.attachments/image-ce353c0e-8053-4910-8797-456a544aaaec.png)

APIM 物件ID 會變更
![image.png](/.attachments/image-99f79737-590b-47a9-9229-e6db070f35cd.png)

可以在 AAD > 企業應用程式 搜尋到此服務
![image.png](/.attachments/image-82a38772-f4bc-4b97-97f5-e18859b4f7ec.png)
