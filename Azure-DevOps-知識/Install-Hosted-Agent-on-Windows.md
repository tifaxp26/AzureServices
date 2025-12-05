[[_TOC_]]

# 注意事項
要注意 Azure VM SKU 變更後，要確認是否支援 Docker Desktop


# 安裝 VM      
- VM 安全性要選標準
- 開啟hyper-V

![image.png](/.attachments/image-ea862309-592b-478b-895b-02f7a8566710.png)

# 安裝 VSTS Agent
![image.png](/.attachments/image-b5772e7c-42d0-4044-8122-937864b9d2a4.png)

- 啟用 Local System 角色
![image.png](/.attachments/image-c71fe06c-02a4-45a4-aa35-7603a83cf860.png)

- 確認服務註冊成功
![image.png](/.attachments/image-545c9536-5ce3-4898-a263-a5d9a59ee88b.png)


# 安裝 Docker Desktop

- 檢查 docker command
![image.png](/.attachments/image-f7ea362b-10af-4609-915a-d8e4cb7f5184.png)
![image.png](/.attachments/image-bbc1f2ef-5ced-4377-87cd-a2ebab6389f1.png)
![image.png](/.attachments/image-f75e6f0f-3bf6-4cb7-9f9f-097dcdbb5368.png)

- Docker 環境設定
![image.png](/.attachments/image-59509880-e586-4c32-9b65-895cd9342826.png)
![image.png](/.attachments/image-cefc55b7-a250-49f8-8384-65dba14c9336.png)
![image.png](/.attachments/image-9d3d7818-300b-4591-9341-eec05f737681.png)

- 切換 linux container
![image.png](/.attachments/image-16d207ef-2905-478d-bbe8-bea2483e4b4e.png)

- 變更 image Path
![image.png](/.attachments/image-2513f249-aa92-4033-bf3d-aafea92371da.png)

# 安裝 Git
![image.png](/.attachments/image-ce37e873-04a3-4df3-8b7a-772839ec5ee5.png)

# 安裝 Node.JS
![image.png](/.attachments/image-a2b9cb42-df19-492a-9f71-57ca8d8a5d5e.png)
![image.png](/.attachments/image-5e294515-707e-4662-ae2b-44cafd3924d2.png)


# 設定Docker Always On

### 方法一：使用 Docker Desktop 內建設定（登入時啟動）

Docker Desktop 提供一個選項，允許在使用者登入 Windows 帳戶時自動啟動：
1.  打開 Docker Desktop    
2.  點擊右上角的齒輪圖示進入「設定」    
3.  在「一般（General）」頁籤中，勾選「登入電腦時啟動 Docker Desktop（Start Docker Desktop when you log in）」
4.  點擊「Apply & Restart」以保存設定
此設定適用於使用者登入後自動啟動 Docker Desktop

### 方法二：使用 Windows 工作排程器在系統啟動時啟動 Docker Desktop

如果您希望在系統啟動時（即使尚未登入使用者帳戶）自動啟動 Docker Desktop，可以使用 Windows 的工作排程器來實現：
1.  以系統管理員身份開啟 PowerShell。    
2.  執行以下命令以建立一個在系統啟動時執行的排程任務：
   
`$Action = New-ScheduledTaskAction -Execute "C:\Program Files\Docker\Docker\Docker Desktop.exe" $Trigger = New-ScheduledTaskTrigger -AtStartup $Principal = New-ScheduledTaskPrincipal -UserId "SYSTEM" -RunLevel Highest Register-ScheduledTask -TaskName "Start Docker Desktop" -Action $Action -Trigger $Trigger -Principal $Principal`
    

這段腳本會建立一個名為「Start Docker Desktop」的排程任務，該任務在系統啟動時以最高權限執行 Docker Desktop。

![image.png](/.attachments/image-8df73971-1835-47bc-b311-8a1d6b46c07f.png)
![image.png](/.attachments/image-efd67834-1e0c-4260-b8c1-6d15909c75cb.png)
![image.png](/.attachments/image-221c8bf4-f58c-4047-a5f0-d25089b50c7e.png)
![image.png](/.attachments/image-b19a8e2b-ec69-4acc-aa99-ac350b2a663b.png)

