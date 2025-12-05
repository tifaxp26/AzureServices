[[_TOC_]]

# 說明
組織A 要使用 組織B 的 Azure服務

## 組織B 訂閱帳戶 授權 
角色: 參與者
![image.png](/.attachments/image-99fd70ea-f7c2-4bdd-80b9-f496c66857d7.png)

在 Portal 切換目錄
![image.png](/.attachments/image-f3ccfbb5-e3d2-498e-a6df-cc3e691849fa.png)

檢查 目的端服務 訂閱帳戶
![image.png](/.attachments/image-545b3499-c9b8-4a02-b92f-2db10c989247.png)

檢查 目的端服務 ACR
![image.png](/.attachments/image-62658199-e0ef-424d-aa5f-5b0cba18dcc1.png)

## 進到 Azure DevOps
1. 進到 專案設定
進到 Service Connections 功能
建立 新 Service Connections，選 Azure Reource Manager
![image.png](/.attachments/image-00602504-6495-40e4-8716-a110edc9708c.png)
![image.png](/.attachments/image-158df618-d254-4091-ba14-c2000f0cf51b.png)

2. Identity type: 要選 手動設定
Credential: 要選 Workload identity federation  (同盟認證)
系統、服務會自動更新 密碼
![image.png](/.attachments/image-6317894d-309a-4a82-99ad-4b6442c7c01e.png)

3. Environment: 選 Azure Cloud
Server Url: 預設 
Scope Level: 選 Subscription
Subscription ID: 輸入 組織B 的訂閱帳戶 ID
Subscription Name: 輸入 組織B 的訂閱帳戶 Name
![image.png](/.attachments/image-dd8ba1d2-fda7-4af6-839f-972883963435.png)

4. Authentication 
我們使用 應用程式註冊 的方式，進行授權
Applcation (client) ID: 輸入 組織B 提供的 應用程式註冊 Client_ID
Directory (tenant) ID: 輸入 組織B 提供的 組織_ID
ARM 服務 會自動產生 
Issuer 及 Subject identifier 要將此資訊提供給 組織B 進行設定
![image.png](/.attachments/image-574675ba-6869-47ed-961f-fad41814be07.png)

5. 組織B 進到 Azure 應用程式註冊
進行同盟認證
![顯示 [同盟認證] 索引卷標的螢幕快照。](https://learn.microsoft.com/zh-tw/azure/devops/pipelines/release/approvals/media/federated-credentials-choice.png?view=azure-devops)

![顯示選取同盟認證案例的螢幕快照。](https://learn.microsoft.com/zh-tw/azure/devops/pipelines/release/approvals/media/federated-credential-scenario.png?view=azure-devops)

![Azure DevOps 和 Azure 入口網站 中同盟認證的比較螢幕快照。](https://learn.microsoft.com/zh-tw/azure/devops/pipelines/release/approvals/media/copy-federated-credential.png?view=azure-devops)

6. 建立完畢
![image.png](/.attachments/image-53c89619-b179-4b1e-819d-6fd9f5353dca.png)

7. 到 Pipelines > Release 
資料來源 選 Azure Container Registy，
要能選到 ARM Service Connection
![image.png](/.attachments/image-0983fed6-baca-4300-9e72-b67cf83807de.png)


# 參考
https://learn.microsoft.com/zh-tw/azure/devops/pipelines/release/configure-workload-identity?view=azure-devops&tabs=app-registration