[[_TOC_]]


VM 使用 Public 連線方式


去回同路
要在FW 設定 DNAT，
請求由Internet 到 FW 進入 VNET 的VM
返回由VM 的 VNET 到 FW 回 Internet

![image.png](/.attachments/image-6ab4023e-2733-4587-83ea-1e34f62b1e6c.png)



【正確範例】
internet => FW DNAT => 端點入口 FW 公用IP:port => 端點VM private IP 10.100.101.2:3389
		    return => VNT => FW => internet
【錯誤範例】
client => VM public IP   20.200.20.2:3389
			return VENT => FW block


# 設定方式
## 設定 FW DNAT
![image.png](/.attachments/image-23573cd2-8786-4d70-9a31-8a61e8a2846d.png)

## 設定 VM NSG
選 Inbound 來源 ServiceTag: VNET
![image.png](/.attachments/image-fffb48a7-d2d5-4469-9545-90c078289f44.png)

## 設定路由表
可以忽略不用額外設定
![image.png](/.attachments/image-1be19825-a1a2-46e4-8d18-ff82a6ab0e8b.png)

# 參考

[上班基本功: SSH 混搭 Azure Firewall - 魂系架構 Phil's Workspace](https://blog.pichuang.com.tw/20240130-ssh-tunneling.html?h=firewa#_1)