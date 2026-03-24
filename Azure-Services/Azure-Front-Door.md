
[[_TOC_]]


# 說明

Azure Front Door 
新式雲端 CDN，可隨時隨地為使用者提供最佳化體驗。

Azure Front Door 是一種新式雲端內容傳遞網路 (CDN) 服務，可針對您的內容和應用程式提供高效能、可擴縮性及安全的使用者體驗。

Diagram of Azure Front Door architecture
![image.png](/.attachments/image-10cb5159-118a-42e3-a79b-c176189d5f2e.png)

![image.png](/.attachments/image-dbcc56b9-0305-480a-81f0-bd20686e0e6f.png)

![image.png](/.attachments/image-af15e285-b046-4b8d-87cc-643f0174f3da.png)


## 服務比較
----

下表提供 Azure Front Door 與 Azure CDN 服務之間的比較。

| 功能和最佳化 | Front Door 標準 | Front Door 進階 | Front Door (傳統) | Microsoft 的 CDN 標準版 (傳統版) |
| --- | --- | --- | --- | --- |
| **傳遞和加速** |  |  |  |  |
| 靜態檔案傳遞 | ✓ | ✓ | ✓ | ✓ |
| 動態網站傳遞 | ✓ | ✓ | ✓ |  |
| **網域和憑證** |  |  |  |  |
| 自訂網域 | ✓ - 以 DNS TXT 記錄為基礎的網域驗證 | ✓ - 以 DNS TXT 記錄為基礎的網域驗證 | ✓ - 以 CNAME 為基礎的驗證 | ✓ - 以 CNAME 為基礎的驗證 |
| 預先驗證的網域與 Azure PaaS 服務整合 | ✓ | ✓ |  |  |
| HTTPS 支援 | ✓ | ✓ | ✓ | ✓ |
| 自訂網域 HTTPS | ✓ | ✓ | ✓ | ✓ |
| 使用您自己的憑證 | ✓ | ✓ | ✓ | ✓ |
| 支援的 TLS 版本 | TLS1.3、TLS1.2、TLS1.0 | TLS1.3、TLS1.2、TLS1.0 | TLS1.3、TLS1.2、TLS1.0 | TLS1.3、TLS 1.2、TLS 1.0/1.1 |
| **快取** |  |  |  |  |
| 查詢字串快取 | ✓ | ✓ | ✓ | ✓ |
| 快取管理（清除、規則和壓縮） | ✓ | ✓ | ✓ | ✓ |
| 快速清除 |  |  |  |  |
| 資產預先載入 |  |  |  |  |
| 快取行為設定 | ✓ - 使用標準規則引擎 | ✓ - 使用標準規則引擎 | ✓ - 使用標準規則引擎 | ✓ - 使用標準規則引擎 |
| **路由** |  |  |  |  |
| 原點負載平衡 | ✓ | ✓ | ✓ | ✓ |
| 路徑型路由 | ✓ | ✓ | ✓ | ✓ |
| 規則引擎 | ✓ | ✓ | ✓ | ✓ |
| 伺服器變數 | ✓ | ✓ |  |  |
| 規則引擎中的規則運算式 | ✓ | ✓ |  |  |
| URL 重新導向/重寫 | ✓ | ✓ | ✓ | ✓ |
| IPv4/IPv6 雙重堆疊 | ✓ | ✓ | ✓ | ✓ |
| HTTP/2 支援 | ✓ | ✓ | ✓ | ✓ |
| 非計量付費路由喜好設定 | 不需要，因為資料從 Azure 來源傳輸至 AFD 是免費，而且路徑會直接連接 | 不需要，因為資料從 Azure 來源傳輸至 AFD 是免費，而且路徑會直接連接 | 不需要，因為資料從 Azure 來源傳輸至 AFD 是免費，而且路徑會直接連接 | 不需要，因為資料從 Azure 來源傳輸至 CDN 是免費，而且路徑會直接連接 |
| 原始連接埠 | 所有 TCP 連接埠 | 所有 TCP 連接埠 | 所有 TCP 連接埠 | 所有 TCP 連接埠 |
| 可自訂的規則式內容傳遞引擎 | ✓ | ✓ | ✓ | ✓ 使用標準規則引擎 |
| 行動裝置規則 | ✓ | ✓ | ✓ | ✓ 使用標準規則引擎 |
| **安全性** |  |  |  |  |
| 自訂 Web 應用程式防火牆 (WAF) 規則 | ✓ | ✓ | ✓ |  |
| Microsoft 受控規則集 |  | ✓ | ✓ - 僅默認規則集 1.1 或更少 |  |
| Bot 保護 |  | ✓ | ✓ - 僅限 Bot 管理員規則集 1.0 |  |
| 來源的私人連結連線 |  | ✓ |  |  |
| 地理位置篩選 | ✓ | ✓ | ✓ | ✓ |
| 權杖驗證 |  |  |  |  |
| DDOS 保護 | ✓ | ✓ | ✓ | ✓ |
| 網域前端區塊 | ✓ | ✓ | ✓ | ✓ |
| **分析和報告** |  |  |  |  |
| 監視計量 | ✓ (比傳統更多的計量) | ✓ (比傳統更多的計量) | ✓ | ✓ |
| 進階分析/內建報告 | ✓ | ✓ - 包含 WAF 報告 |  |  |
| 未經處理記錄 - 存取記錄和 WAF 記錄 | ✓ | ✓ | ✓ | ✓ |
| 健康狀態探查記錄 | ✓ | ✓ |  |  |
| **容易使用** |  |  |  |  |
| 輕鬆與 Azure 服務 (例如儲存體和 Web Apps) 整合 | ✓ | ✓ | ✓ | ✓ |
| 透過 REST API、.NET、de.js 或 PowerShell 進行管理 | ✓ | ✓ | ✓ | ✓ |
| 壓縮 MIME 類型 | 可設定 | 可設定 | 可設定 | 可設定 |
| 壓縮編碼 | gzip、brotli | gzip、brotli | gzip、brotli | gzip、brotli |
| Azure 原則整合 | ✓ | ✓ | ✓ |  |
| Azure Advisor 整合 | ✓ | ✓ |  | ✓ |
| 使用 Azure Key Vault 的受控識別 | ✓ | ✓ |  |  |
| **定價** | [Azure Front Door 定價](https://azure.microsoft.com/pricing/details/frontdoor/) |  |  | [Azure CDN 定價](https://azure.microsoft.com/pricing/details/cdn/) |
| 簡化的價格 | ✓ | ✓ |  | ✓ |



## Azure Front Door 定價
-------------------

Azure Front Door 是安全的雲端 CDN 服務，可加速內容傳遞，同時保護來自網路威脅的應用程式、API 與網站。Azure Front Door 合併了傳統 CDN、全域負載平衡、動態站台加速與安全性的功能，包括 Azure Web 應用程式防火牆 (WAF) 與 DDoS。 ​Azure Front Door 定價可在兩個層級中提供使用:
**Azure Front Door Standard** 針對內容傳遞進行了最佳化，提供靜態與動態內容加速、全域負載平衡、SSL 卸載、網域與憑證管理、增強的流量分析，以及基本安全性功能。​
根據 Azure Front Door Standard 的功能來建置 **Azure Front Door Premium**，並新增跨 WAF 的廣泛安全性功能、BOT 保護、Azure Private Link 支援、與 Microsoft 威脅情報的整合，以及安全性分析。WAF 與 Private Link 定價包含在 Azure Front Door Premium 中。
Azure Front Door Standard/Premium 根據下列定價維度計費:​
1.  基本費用 (也就是依小時計算的固定費用)
2.  從 Edge 到用戶端的輸出資料傳輸
3.  從 Edge 到原點的輸出資料傳輸
4.  從用戶端到 Front Door 邊緣位置的輸入要求
5.  從 Azure 資料中心的來源免費將資料傳輸到 Front Door 的邊緣位置


### 基本費用 (僅針對使用的時數按小時計費)

![image.png](/.attachments/image-9af6a9e8-a33c-47b8-99e2-b809fa96f928.png)
![image.png](/.attachments/image-3cf17c65-b30c-45f2-9b6a-4b8261e86afa.png)
![image.png](/.attachments/image-5c323c3c-5a67-4505-9241-fc2bc0faeaad.png)
![image.png](/.attachments/image-7625b129-1d5b-4ca3-a22d-8665f71179e4.png)


# 服務建置步驟

本快速入門會引導您完成使用 Azure 入口網站建立 Azure Front Door 設定檔的流程。 您有兩個選項可建立 Azure Front Door 設定檔：快速建立和自訂建立。 [快速建立] 選項可讓您完成設定檔的基本設定，而 [自訂建立] 選項可讓您使用更進階的設定來自訂設定檔。
在本快速入門中，您會使用 [自訂建立] 選項來建立 Azure Front Door 設定檔。 您必須先將兩個應用程式服務部署為原點伺服器。 然後，您可以設定 Azure Front Door 設定檔，以根據特定規則將流量路由至您的應用程式服務。 最後，您可以藉由存取 Azure Front Door 前端主機名來測試應用程式服務的連線能力。

使用 Azure 入口網站 的 Front Door 部署環境圖表。
![image.png](/.attachments/image-9179ca0a-9498-490f-981b-a2b79868f116.png)

---

## 事前準備
### 訂閱帳戶: WALSIN-Azure-AppSecZone
![image.png](/.attachments/image-593cebc1-9913-4646-b5cf-ab9194fe8ca6.png)

### 資源提供者: Microsoft.Cdn
![image.png](/.attachments/image-abbd28e4-62fe-4197-98fd-2c8a34ecc358.png)


### 名稱: afd-walsin
![image.png](/.attachments/image-a655cca5-ac47-491e-95e2-bc8e06bb21ac.png)
![image.png](/.attachments/image-84ebe402-7d6f-4aef-8fd3-d8d813e297a9.png)
![image.png](/.attachments/image-cae7d7de-1b96-4681-87fc-d0da85907f7c.png)
![image.png](/.attachments/image-0fa9f4b0-355b-4ea9-9f61-d680883d3c9c.png)

---


# DNS 區域
## 說明

使用 Azure 入口網站 在 Azure Front Door 上設定自定義網域

使用 Azure Front Door 傳遞應用程式時，自定義網域可讓您自己的域名出現在使用者要求中。 此可見度可以增強客戶的便利性，並支援品牌努力。
根據預設，建立 Azure Front Door Standard/Premium 配置檔和端點之後，端點主機是 的 `azurefd.net`子域。 例如，URL 看起來可能像 `https://contoso-frontend-mdjf2jfgjf82mnzx.z01.azurefd.net/activeusers.htm`。
若要讓您的 URL 更容易使用且品牌化，Azure Front Door 可讓您建立自定義網域的關聯。 如此一來，您的內容可以使用 URL 中的自定義網域來傳遞，例如 `https://www.contoso.com/photo.png`，而不是預設的 Azure Front Door 網域。

![image.png](/.attachments/image-e373d8d7-687c-4c15-8ee8-b8901455f194.png)



---

# Front Door 設定步驟

## 建立 網域
輸入自訂網域名稱
![image.png](/.attachments/image-ea68e43d-e384-42c4-a38e-8ecfb06b261b.png)
建立成功後，會產生 TXT 紀錄名稱 及 紀錄值
![image.png](/.attachments/image-2f1d2d1f-ce1d-4f38-b65e-dfe4dfe4e054.png)

## DNS 區域，建立 TXT
註冊域名
註冊TXT，填入 紀錄名稱 及 記錄值，
![image.png](/.attachments/image-3d56197c-7a91-45a5-acdf-8068f8d71de8.png)
![image.png](/.attachments/image-784566f0-beb6-4660-a1d5-0f1b093b6843.png)


## 建立 Front Door 管理員 / 端點
會產生 端點主機名稱
![image.png](/.attachments/image-41d99d91-59c6-4e6a-9df0-0666d738732a.png)

## DNS 區域，建立 CNAME
註冊 CNAME，別名為 端點主機名稱 
![image.png](/.attachments/image-e8f1a500-22f2-49b3-942b-9655f89f647a.png)
![image.png](/.attachments/image-347eba53-98c0-483a-852f-49f02afcbb4d.png)

## 建立 路由 & 來源群組
![image.png](/.attachments/image-9396a602-f1d1-48b1-b4b5-aff6f6187bdb.png)
![image.png](/.attachments/image-b9752e7e-5918-4b71-a8a1-b4bdfc0bbce5.png)
![image.png](/.attachments/image-a4c7e21a-a87d-463e-99ae-87b533d7a508.png)

- 設定來源 origin groups
- 來源類型: 自訂
- 主機名稱: APGW 外部IP
- 來源主機標頭: 系統域名
- 憑證主體名稱驗證: 停用

**新版設定**
![image.png](/.attachments/image-8908db31-daa4-4250-926e-69c96e5ecdf5.png)

**舊版設定**
![image.png](/.attachments/image-2a593a81-5a07-4e64-9184-3d6fc636efb6.png)


## 建立 規則集
![image.png](/.attachments/image-d8348dac-cda9-4a3d-8d4b-2e702bca0be4.png)

# 建立端點與路由之間的關聯
![image.png](/.attachments/image-a1c0d12c-76e5-4969-94fa-18642f5553af.png)

# 完成
![image.png](/.attachments/image-bb12f8d6-ff93-47a8-b344-9267893cc70d.png)



---

# AFD 綁定 SSL
## SSL憑證 放在 Azure Keyvault 
![image.png](/.attachments/image-d7e91dce-cb9a-4ea7-a9bc-2b0232792804.png)

## 存入SSL憑證
![image.png](/.attachments/image-ca33394f-4fd7-4521-942c-86ef57189149.png)

## 身分識別
![image.png](/.attachments/image-8cffc676-a350-4cf1-8d37-3dd483a7e8a5.png)

## 祕密
![image.png](/.attachments/image-0bf8cc0f-d69f-4e36-84dc-0761b1cf8402.png)
![image.png](/.attachments/image-564c3bbe-191e-4a83-8ff5-43ecd38d4ffa.png)






---
# 參考
[Azure Front Door 與 Azure CDN 服務之間的比較 | Microsoft Learn](https://learn.microsoft.com/zh-tw/azure/frontdoor/front-door-cdn-comparison)

[定價 - Front Door | Microsoft Azure](https://azure.microsoft.com/zh-tw/pricing/details/frontdoor/#pricing)

[快速入門：使用 Azure 入口網站 建立 Azure Front Door](https://learn.microsoft.com/zh-tw/azure/frontdoor/create-front-door-portal?tabs=quick)




