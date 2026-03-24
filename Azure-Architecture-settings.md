[[_TOC_]]

![image.png](/.attachments/image-057d5b73-3b7d-46d1-af22-b83fcf5f0af0.png)

# IP Groups
## 建立IP群組
![image.png](/.attachments/image-1de0c71f-83b6-42bb-b173-023e2f16576e.png)
## 必要項目、注意事項
**資源群組: rg-pdns
訂用帳戶: WALSIN-Azure-Connectivity**
![image.png](/.attachments/image-1d15eea0-5098-4482-aab3-aa30ddb1a073.png)
## 指定 網路/子網路
![image.png](/.attachments/image-8b573309-4ad4-4412-832e-bb8ce93ec389.png)

# 網路安全性群組
建立 NSG，跟著服務走 即可。
![image.png](/.attachments/image-80ee7d5a-40ef-473a-b781-e01ca1db9d49.png)


# 路由表

除 VPN 串內網外，所有服務皆須到 AFW
設定如下
![image.png](/.attachments/image-3da312d2-0695-40b5-ad7f-1bd250baca77.png)
![image.png](/.attachments/image-23b8176b-50d0-4086-a2ab-6a60cf21dd07.png)

- 路由表中的prefix指的是目的端
- 來源端是路由表關聯的subnet
- 從來源端到目的端，需要經過的設備，就是next hop
![image.png](/.attachments/image-aed6347a-a24e-47f6-a9b0-2f051aad4c7f.png)

## 建立路由
![image.png](/.attachments/image-d796795d-3233-4e54-960f-1d2062269dff.png)
## 指到防火牆位置
![image.png](/.attachments/image-40d1bdfc-6ae2-4e74-b6df-ed5d6e9c4374.png)
## 選擇子網路 套用
![image.png](/.attachments/image-b9bcf48b-a143-4867-a3ce-4d4e463124f1.png)

## 如果服務有來自 apgw，則需要再設定 rt-appgw
![image.png](/.attachments/image-c80fdd56-ccd8-47c5-a559-b31043cb93e3.png)


# 建立虛擬網路
概觀
![image.png](/.attachments/image-424eb5b9-a150-4c1e-aff6-f1acf3e7fba1.png)
## 位址空間
![image.png](/.attachments/image-9abff548-4680-4745-bd4e-b01cf687ada5.png)
## 子網路
![image.png](/.attachments/image-d00c5145-ba7d-46ac-bfd2-3339f38fcb83.png)
## 對等互連
![image.png](/.attachments/image-f189caee-2d69-4b64-84a1-3af067bbc65a.png)

1. 串接至 VPN
![image.png](/.attachments/image-f834f399-1938-432c-82d8-ab95952f83ca.png)
![image.png](/.attachments/image-23f26273-b70c-4001-b5da-cac50008a5e6.png)

1. 串接至 DMZ
![image.png](/.attachments/image-bb1867fa-4ab5-42de-a899-243a6e2e0bcd.png)
![image.png](/.attachments/image-061c319b-d133-49e7-b42c-dfee6e5227c3.png)


# 建立 DNS 轉送規則集
選擇 虛擬網路連結
這樣網路會經由FW 再到DNS 訪問
![image.png](/.attachments/image-8cef9b95-1879-4a4d-ab9c-8022d24ffa82.png)


# 防火牆原則

## 建立 規則集合名稱
rcg-{專案名稱}
![image.png](/.attachments/image-0ebe599f-7536-407a-babf-9c501a48975e.png)

## 建立網路規則
![image.png](/.attachments/image-40e99317-3f2e-40ba-9450-71ab39486d77.png)

## 建立應用程式規則
![image.png](/.attachments/image-934832a0-a9cc-4cf8-a3bb-665857eabb8b.png)


# 跨 VNET 設定
## 新增目的端 IP Group
![image.png](/.attachments/image-c180dd1d-55e1-4e27-84df-b1ad37222db2.png)
## 建立 防火牆規則集合 network-rules
選擇來源 與 目的 ip Group 
![image.png](/.attachments/image-98fda3f6-ee64-4ee2-b52a-219e342a2915.png)


<hr />
編輯規則集合
network-allow-rule
210
Allow AnytoAny

![image.png](/.attachments/image-f5e38064-de6d-47b0-89ff-3bcecb77f6b6.png)


application-allow-rules
Allow AnytoAny
Http:80,Https:443
ifconfig.me
![image.png](/.attachments/image-bf2faba2-78bd-41de-8917-f22b1ef5d165.png)


# Azure Policy
正式環境 Policy 有條規範 Subnets should have a Network Security Group 
如果有AKS 服務，此Policy要暫時停用，才能設定AKS Subnet

管理群組 > policy > 豁免
![image.png](/.attachments/image-6c297d48-e85a-40a8-abe4-1fbf870de2d1.png)
![image.png](/.attachments/image-c704fd8f-00f3-4fa3-8b35-b0ef3d086a52.png)

移除豁免
![image.png](/.attachments/image-7c04f70d-a6f2-468d-983b-81a767af92de.png)
![image.png](/.attachments/image-fccce919-e596-4c0e-83fe-7a41fa74ae80.png)



