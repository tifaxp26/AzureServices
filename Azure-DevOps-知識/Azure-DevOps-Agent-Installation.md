[[_TOC_]]



```
請輸入 伺服器 URL > https://dev.azure.com/WalsinTeam/
輸入 驗證類型 (按 Enter 以取得 PAT) >  PAT
請輸入 個人存取權杖 > *******************

正在連線到伺服器 ...
>> 註冊代理程式:

代理程式集區: Walsin Pipelines
代理程式名稱: IDC-DS-DB Agent01
工作資料夾: _work
將代理程式作為服務執行? Y
輸入 啟用代理程式服務的SERVICE_SID_TYPE_UNRESTRICTED (是/否)? Y
```

![image.png](/.attachments/image-2a3684b7-a3df-464c-a199-39b072d4f4c3.png)

Agent安裝完畢後，會註冊到 Azure DevOps Agent Pool
![image.png](/.attachments/image-a34d213e-c3c3-414d-8f60-02acb7f06123.png)
![image.png](/.attachments/image-14de0bb1-4e95-4715-a477-f23bbf6ada04.png)



# 測試
Agent pool 選用 Walsin Pipelines
![image.png](/.attachments/image-300ebb57-def7-4e1c-802b-28deb194dc94.png)

運行時，可以看到 Agent jobs
![image.png](/.attachments/image-bf6193c4-c219-4e73-9ded-fc5fbffcfca8.png)



# 參考

```
SERVICE_SID_TYPE_UNRESTRICTED
0x00000001
建立服務進程時，服務 SID 會以下列屬性新增至服務進程令牌：SE_GROUP_ENABLED_BY_DEFAULT |SE_GROUP_OWNER。
```


https://learn.microsoft.com/zh-tw/windows/win32/api/winsvc/ns-winsvc-service_sid_info



# Azure VM Windos OS 安裝方式
注意事項: 
**安全性類型 一定要選 標準**

## install wsl2

## 安裝 Hyper-V
![image.png](/.attachments/image-504c6e20-9e0e-4aba-8865-b2d9d367edf6.png)

## 安裝 Docker

![image.png](/.attachments/image-021be03b-5be6-4ab8-b8c2-1bc62068a796.png)
![image.png](/.attachments/image-f6cb3a06-6fdc-4d0b-a140-de1fde53c4ea.png)
![image.png](/.attachments/image-dd7c60a5-1aa4-467d-87de-0020041fe956.png)

驗證安裝 
Power Shell
`docker`
![image.png](/.attachments/image-86f34be3-928d-4b28-815f-764dd50d2d5c.png)

`kubectl`
![image.png](/.attachments/image-59ffcfc9-5233-4c63-80d6-2a7a254b0a04.png)

開啟 Docker 服務
![image.png](/.attachments/image-b4ce62b3-edc1-42e7-af10-2be8f36f164b.png)

檢查設定
啟用 Expose daemon on tcp://localhost:2375 without TLS
![image.png](/.attachments/image-a945af8a-ec16-4e77-99c2-a0c3df04ab23.png)
![image.png](/.attachments/image-ee0397c4-54f5-4e18-80a1-60183979cda3.png)


## 服務改成Local System角色
![image.png](/.attachments/image-49048157-204e-4b5c-a617-203a910d8d3f.png)

## Switch to Windows/Linux Containers
![image.png](/.attachments/image-63263334-ea37-45f5-9c82-e6c9a6b84bbf.png)


## Download GIT for Windows
![image.png](/.attachments/image-891bafad-3974-49df-8e57-bfea1a673ad3.png)

## Download Node.js & NPM
![image.png](/.attachments/image-26288699-9933-4f8b-8edb-47b9c2df6de3.png)

![image.png](/.attachments/image-8927f249-7f53-46a0-963d-f8de6c2626bf.png)


## SwitchDaemon

```
With Powershell:
	Open Powershell as administrator
	Launch command: & 'C:\Program Files\Docker\Docker\DockerCli.exe' -SwitchDaemon

OR, with cmd:
	Open cmd as administrator
	Launch command: "C:\Program Files\Docker\Docker\DockerCli.exe" -SwitchDaemon
```

`Enable-WindowsOptionalFeature -Online -FeatureName $("Microsoft-Hyper-V", "Containers") -All`



# Clean Pipelines _work data
![image.png](/.attachments/image-aea5b0c2-0b38-4558-8e7c-1a230f222c66.png)










