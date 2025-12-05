[[_TOC_]]

# 建置步驟說明
1. 準備VM (包含已安裝的服務)
1. 登入VM 進行一般化作業
1. 製作影像檔模板
1. 建立VM from 模板

# 注意事項
1. 主要服務、應用服務 放在 **OS Disk**
1. 一般資料、Logs、檔案使用 **Data Disk** 並開啟共用磁碟


---
# 製作模板       
1. 登入您的 Windows VM。
1. 開啟命令提示字元視窗，以系統管理員身分執行。
1. 刪除 panther 目錄 (C:\Windows\Panther)。
1. 然後將目錄變更為 %windir%\system32\sysprep，然後執行：
`C:\Windows\System32\Sysprep\sysprep.exe /oobe /generalize /shutdown`

![image.png](/.attachments/image-5ebe6d67-a008-4274-9f17-70e45d300444.png)

      
1. 執行sysprep處理的時候可能會出現報錯
![image.png](/.attachments/image-aeff8160-28dd-41ce-806b-693fd9bca72f.png)

      
1. 在Powershell中使用指令查詢系統當前的程式，確認有無語言包，若有的話必需先移除(紅框處)
`Get-AppxPackage -name "Microsoft.LanguageExperiencePac*"`
![image.png](/.attachments/image-a7c5fc7e-c43d-4635-b29f-492fd13738ea.png)

      
1. 在Powershell中使用指令移除這個有問題的程式。重新查詢，系統已經沒有這個有問題的程式
`Remove-AppxPackage -allusers -package "Microsoft.LanguageExperiencePackzh-TW_22621.88.295.0_neutral__8wekyb3d8bbwe"`
      
1. 重新執行一般化就可正常的進行，如果還是提示錯誤，就按照流程重新查找出現問題的程式並移除
![image.png](/.attachments/image-c8c90e83-337c-4d70-a583-809c6576ab9e.png)
![image.png](/.attachments/image-a5c54e73-e6b0-4293-bb47-1b2ffe3822ab.png)

      
1. **當Sysprep完成VM一般化時，VM將會關閉。不要重新啟動VM**
在Sysprep完成後，請使用Cloud Shell將虛擬機器的狀態設定為 [已一般化]
在 Azure Portal > Azure Power Shell
`Set-AzContext -Name $Name -Subscription $Subscription -Tenant $Tenant`
`Set-AzVm -ResourceGroupName $rgName -Name $vmName -Generalized`

![image.png](/.attachments/image-aeb5d8ce-05fa-4e51-a80f-c22907194822.png)

![image.png](/.attachments/image-50bc74f7-394b-4813-88a6-644434d0b6a2.png)

![image.png](/.attachments/image-f976ad9d-a2d3-4f99-8f1a-80540bfac4ce.png)

![image.png](/.attachments/image-871be988-333d-4cb4-ab30-49241730f6ad.png)


# 建立 Image 模板
## 到 模板VM > 擷取 > 映像
![image.png](/.attachments/image-076e6f64-b0f6-42bd-8eb7-fb74835e18ad.png)

## 建立 映像
共用映像至 Azure Computer Gallery: 是
新增 Azure Computer Gallery
![image.png](/.attachments/image-54084754-f546-48a6-9b24-dfdd157e3f74.png)

## 建立 虛擬機器映像定義
自訂以下內容: 
- 定義名稱
- 發行者
- 供應項目
- SKU

![image.png](/.attachments/image-c2bd8f73-d2eb-444f-b88b-637434e7afb3.png)
## 填入 版本號碼
![image.png](/.attachments/image-4f571197-bce3-4d4a-b795-5b24c8fb9324.png)

## 部署
![image.png](/.attachments/image-f2cab54e-ef02-4894-9e1c-e886014f3450.png)

## 完成 Azure Computer Gallery
![image.png](/.attachments/image-4b219126-84b8-4699-a5a3-d4568206d5bd.png)
![image.png](/.attachments/image-dd97ba8e-f9bf-48d3-9a1b-fe50007d1eb2.png)

## 完成 VM映像定義
![image.png](/.attachments/image-adffddc7-8122-4f75-b1b2-29cd885c32a4.png)


