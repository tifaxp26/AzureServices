我先寫我看得懂的流程，要再寫詳細點請在底下留言0.<
# 建置流程
![image.png](/.attachments/image-cfb6d215-5864-406e-a977-3f624e452eef.png)

# Neo4j DB建置
## DB建置在Azure虛擬機上
### 虛擬機設定
- OS : Windows
- Windows內防火牆設定
 -設定路徑 : Control Panel\System and Security\Windows Defender Firewall\左下角Asvanced Setting
-右鍵Inbounded Rules -> New Rules ->開通port:7474,7687
![image.png](/.attachments/image-09bc4b55-25dd-4fd9-9ea1-0f898fc4d4a3.png)
### Neo4j設定
- Desktop 社區版
- Setting開通server.default_listen_address, server.default_advertised_address
server.default_advertised_addres的IP輸入私有IP(10開頭)
- 設定完後記得瀏覽器開http://公有IP(20開頭):7474查看有無啟動成功
![image.png](/.attachments/image-36b82705-7198-4c9b-91db-8d383115cb64.png)

# Flask站台建置
- Python套件: quart, botbuilder, py2neo, openai
- 如果要建置在appservice並且連到Azure bot記得BotFrameworkAdapterSettings要輸入app_id跟密碼，密碼在管理密碼那邊(注意建立密碼後要立即複製密碼，不然之後密碼會被遮蔽掉)
- app.run的host調成'0.0.0.0'
![image.png](/.attachments/image-7195d4a1-a071-420e-a984-4a92eb630f3e.png)

# Azure Devops設定
## Pipeline設定
- 注意Tags有無指到最新版本:latest
![image.png](/.attachments/image-0fc56f00-1273-4ee7-84ea-e76d8d9f661d.png)
## Release設定
![image.png](/.attachments/image-e0641061-178b-4c4a-bdd0-512ec39c8eed.png)


# Azure Appservice設定
## 部署中心設定
- 如果devops那段成功的話，會自動連接appservice
![image.png](/.attachments/image-21ebbb2f-0ffe-4902-980c-4e812e8ca9cb.png)

## 組態設定
- 密碼可以寫在組態裡不用寫在flask裡

## 儲存好後去概觀點擊預設網域讓docker pull到appservice上
## 去部屬中心的紀錄看flask站台的request情況

# Azure Bot設定
## 組態設定
- 主要是填訊息端點
![image.png](/.attachments/image-97ef93ce-f701-4755-8b23-9d219e13cf0d.png)

## 頻道設定
- 可用通道選擇TEAMS，開通後點擊”open in Teams”即可在TEAMS上操作BOT
![image.png](/.attachments/image-7951878e-12f4-455a-9576-47c2aec90601.png)
- 要分享BOT連結給別人點擊”取得機器人內嵌程式碼”，將裡面連結分享給其他人即可
![image.png](/.attachments/image-86356f04-054e-47b1-92a2-61243661bc12.png)

# Bot Framework設定
用於測試Flask串Azure Bot這段
## [下載BotFramework-Emulator](https://github.com/Microsoft/BotFramework-Emulator/releases/tag/v4.14.1)
## 點open bot後填上Appservice上概觀的預設網域，後面記得帶參數例如: https://app-walsin-neo4j-dev.azurewebsites.net /api/message
![image.png](/.attachments/image-eba9f289-7103-4e1a-849b-e86f7194204b.png)
## 本機端測試的話flask跟Emulator不用填App ID跟App password，如果要建置在appservice並且連到Azure bot記得BotFrameworkAdapterSettings要輸入app_id跟密碼，密碼在管理密碼那邊(注意建立密碼後要立即複製密碼，不然之後密碼會被遮蔽掉)
![image.png](/.attachments/image-643e2a10-4697-4791-90eb-2704995d0620.png)