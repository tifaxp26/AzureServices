[[_TOC_]]


# 自訂網域

授權 App Service 從保存庫讀取
根據預設，App Service 資源提供者無法存取您的金鑰保存庫。 
若要使用金鑰保存庫進行憑證部署，您必須授權資源提供者 (App Service) 對金鑰保存庫的讀取權限。 
您可以使用存取原則或角色型存取控制 （RBAC） 來授與存取權。
`az role assignment create --role "Key Vault Certificate User" --assignee "abfa0a7c-a6b6-4736-8310-5855508787cd" --scope /subscriptions/b6f8e1f5-8ad9-4897-8ea0-592917514f74/resourcegroups/rg-afd/providers/Microsoft.KeyVault/vaults/akv-walsin`

![image.png](/.attachments/image-0a6c2ce4-678c-463b-8ae2-ebe9c792ac1f.png)
![image.png](/.attachments/image-2f89ce6d-08d3-400f-860a-a2b1d3694e2d.png)
