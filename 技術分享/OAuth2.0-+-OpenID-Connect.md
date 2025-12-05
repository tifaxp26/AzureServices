[[_TOC_]]

# 說明
在通訊協議層級瞭解 OAuth 或 OpenID Connect （OIDC）不需要使用 Microsoft 身分識別平台。 不過，當您使用身分識別平臺將驗證新增至應用程式時，您將會遇到通訊協定條款與概念。 當您使用 Microsoft Entra 系統管理中心時，我們的文件和驗證連結庫，瞭解一些基本概念可協助您的整合和整體體驗。

# OAuth 2.0 中的角色
四方通常會參與 OAuth 2.0 和 OpenID Connect 驗證和授權交換。 這些交換通常稱為 驗證流程 或 驗證流程。
<IMG  src="https://learn.microsoft.com/zh-tw/entra/identity-platform/media/v2-flows/protocols-roles.svg"  alt="顯示 OAuth 2.0 角色的圖表"/>

## 授權伺服器 - Microsoft 身分識別平台 是授權伺服器。 也稱為識別提供者或IdP，它會安全地處理使用者的資訊、其存取權，以及驗證流程中合作對象之間的信任關係。 授權伺服器會在使用者登入後發出應用程式與 API 用來授與、拒絕或撤銷資源存取權的安全性令牌（授權）。

## 用戶端 - OAuth 交換中的用戶端是要求存取受保護資源的應用程式。 用戶端可以是在伺服器上執行的 Web 應用程式、在使用者的網頁瀏覽器中執行的單頁 Web 應用程式，或呼叫另一個 Web API 的 Web API。 您通常會看到稱為 用戶端應用程式、 應用程式或 應用程式的用戶端。

## 資源擁有者 - 驗證流程中的資源擁有者通常是應用程式使用者，或 OAuth 術語中的使用者 。 使用者「擁有」您應用程式代表其存取的受保護資源（其數據）。 資源擁有者可以授與或拒絕您的應用程式（用戶端）對其所擁有的資源存取權。 例如，您的應用程式可能會呼叫外部系統的 API，以從該系統上的設定檔取得使用者的電子郵件位址。 其配置檔數據是使用者擁有外部系統上的資源，用戶可同意或拒絕應用程式存取其數據的要求。

## 資源伺服器 - 資源伺服器 裝載或提供資源擁有者數據的存取權。 通常，資源伺服器是前端數據存放區的 Web API。 資源伺服器依賴授權伺服器來執行驗證，並使用授權伺服器發出的持有人令牌中的資訊來授與或拒絕資源存取權。

## 語彙基元
驗證流程中的合作物件會使用 持有人令牌 來保證、驗證及驗證主體（使用者、主機或服務），以及授與或拒絕受保護資源的存取權（授權）。 Microsoft 身分識別平台 中的持有人令牌會格式化為 JSON Web 令牌 （JWT）。

## 身分識別平臺會使用三種類型的持有人令牌作為 安全性令牌：

- 存取令牌 - 存取令牌 是由授權伺服器發行給用戶端應用程式。 用戶端會將存取令牌傳遞至資源伺服器。 存取令牌包含授權伺服器授與客戶端的許可權。

- 標識碼令牌 - 識別元令牌 是由授權伺服器發行給用戶端應用程式。 用戶端在登入使用者時使用標識碼令牌，並取得其基本資訊。

- 重新整理令牌 - 用戶端會使用重新整理令牌或 RT，向授權伺服器要求新的存取權和標識元令牌。 您的程式代碼應該將重新整理令牌及其字串內容視為敏感數據，因為它們僅供授權伺服器使用。

## 應用程式註冊
您的用戶端應用程式需要一種方式，才能信任 Microsoft 身分識別平台 所發行的安全性令牌。 建立信任的第一個步驟是 註冊您的應用程式。 當您註冊應用程式時，身分識別平臺會自動指派一些值，而其他則會根據應用程式的類型進行設定。

其中兩個最常參考的應用程式註冊設定如下：

- 應用程式 （用戶端） 識別碼 - 也稱為 應用程式識別碼 和 用戶端識別碼，此值會由身分識別平臺指派給您的應用程式。 用戶端識別碼會唯一識別身分識別平臺中的應用程式，並包含在平台問題的安全性令牌中。

- 重新導向 URI - 授權伺服器會在完成其互動之後，使用重新導向 URI 將資源擁有者 的使用者代理程式 （網頁瀏覽器、行動應用程式）導向至另一個目的地。 例如，在使用者向授權伺服器進行驗證之後。 並非所有客戶端類型都使用重新導向 URI。

您的應用程式註冊也會保存您將用於程式代碼中用來取得標識符和存取令牌之驗證和授權 端點 的相關信息。

## 端點
Microsoft 身分識別平台 會使用 OAuth 2.0 和 OpenID Connect （OIDC） 1.0 的標準相容實作來提供驗證和授權服務。 身分識別平臺等符合標準的授權伺服器會提供一組 HTTP 端點，供驗證流程中的合作對象用來執行流程。

當您註冊或設定應用程式時，會自動產生應用程式的端點 URI。 您在應用程式程式代碼中使用的端點取決於應用程式的類型，以及它應該支援的身分識別（帳戶類型）。

兩個常用的端點是 授權端點 和 令牌端點。 以下是 和 token 端點的authorize範例：

```
# Authorization endpoint - used by client to obtain authorization from the resource owner.
https://login.microsoftonline.com/<issuer>/oauth2/v2.0/authorize
# Token endpoint - used by client to exchange an authorization grant or refresh token for an access token.
https://login.microsoftonline.com/<issuer>/oauth2/v2.0/token

# NOTE: These are examples. Endpoint URI format may vary based on application type,
#       sign-in audience, and Azure cloud instance (global or national cloud).

#       The {issuer} value in the path of the request can be used to control who can sign into the application. 
#       The allowed values are **common** for both Microsoft accounts and work or school accounts, 
#       **organizations** for work or school accounts only, **consumers** for Microsoft accounts only, 
#       and **tenant identifiers** such as the tenant ID or domain name.
```

若要尋找您已註冊之應用程式的端點，請在 [Microsoft Entra 系統管理中心 ] 中瀏覽至：

身分識別>應用程式> 應用程式註冊<> YOUR-APPLICATION 端點>>


下一步
接下來，瞭解每個應用程式類型所使用的 OAuth 2.0 驗證流程，以及您可以在應用程式中用來執行這些流程的連結庫：

- 驗證流程和應用程式案例
- Microsoft 驗證程式庫 (MSAL)

我們強烈建議不要製作您自己的連結庫或原始 HTTP 呼叫來執行驗證流程。 Microsoft驗證連結庫更安全且更容易。 不過，如果您的案例阻止您使用我們的連結庫，或您想要深入瞭解 Microsoft 身分識別平台 的實作，我們有通訊協議參考：

- 授權碼授與流程 - 單頁應用程式 （SPA）、行動應用程式、原生 （桌面） 應用程式
- 用戶端認證流程 - 伺服器端進程、腳本、精靈
- 代理者 （OBO） 流程 - 代表使用者呼叫另一個 Web API 的 Web API
- OpenID Connect - 使用者登入、註銷和單一登錄 （SSO）



# 使用 Microsoft Entra ID 進行 OpenID Connect 驗證
OpenID Connect (OIDC) 是一種驗證通訊協定，以用於驗證的 OAuth2 通訊協定為基礎。 OIDC 會使用來自 OAuth2 的標準化訊息流程來提供識別服務。

OIDC 的設計目標是「化繁為簡、使不可能變成可能」。 OIDC 可讓開發人員在網站和應用程式之間驗證使用者，而不需要保留和管理密碼檔案。 這可以讓應用程式建立器透過安全的方式，驗證目前使用瀏覽器或原生應用程式連線至應用程式的人員身分。

使用者的驗證必須在將檢查使用者工作階段或認證的識別提供者上進行。 若要這樣做，您需要受信任的代理程式。 針對此目的，原生應用程式通常會啟動系統瀏覽器。 由於內嵌檢視無法防止應用程式窺探使用者密碼，因此不受信任。

除了驗證之外，還可以要求使用者同意。 同意是使用者允許應用程式存取受保護資源的明確權限。 同意與驗證不同，因為同意只需要針對資源提供一次即可。 同意會保持有效，直到使用者或系統管理員手動撤銷授與為止。

## 使用時機
需要使用者同意和網頁登入時。
<IMG  src="https://learn.microsoft.com/zh-tw/entra/architecture/media/authentication-patterns/oidc-auth.png"  alt="架構圖表"/>

## 系統元件
使用者：從應用程式要求服務。

受信任的代理程式：使用者與其互動的元件。 這個受信任的代理程式通常是網頁瀏覽器。

應用程式：應用程式或資源伺服器是資源或資料的所在位置。 應用程式會信任識別提供者，以安全地驗證及授權受信任的代理程式。

Microsoft Entra ID：OIDC 提供者也稱為識別提供者，可安全地處理與使用者資訊、使用者存取權，以及流程中合作對象彼此間信任關係有關的任何項目。 識別提供者會驗證使用者的身分識別、授與及撤銷資源的存取權，以及發行權杖。

<hr />


# Microsoft 身分識別平台和 OAuth 2.0 授權碼流程
https://learn.microsoft.com/zh-tw/entra/identity-platform/v2-oauth2-auth-code-flow

# Microsoft 身分識別平台 和 OAuth 2.0 隱含授與流程
https://learn.microsoft.com/zh-tw/entra/identity-platform/v2-oauth2-implicit-grant-flow



# OAuth2 極簡攻略（一）Implicit Flow
https://editor.leonh.space/2022/oauth2-implicit-flow/


# Microsoft 身分識別平台 中的 OAuth 2.0 和 OpenID Connect （OIDC）
https://learn.microsoft.com/zh-tw/entra/identity-platform/v2-protocols


# Authorization Code 核發流程
https://openhome.cc/Gossip/Spring/OAuth2CodeGrant.html

# Implicit 核發流程
https://openhome.cc/Gossip/Spring/OAuth2ImplicitGrant.html


# OAuth 2.0 筆記 (4.2) Implicit Grant Flow 細節
https://blog.yorkxin.org/posts/oauth2-4-2-implicit-grant-flow/

# OAuth 2.0 筆記 (4.1) Authorization Code Grant Flow 細節
https://blog.yorkxin.org/posts/oauth2-4-1-auth-code-grant-flow/



# 參考
- https://learn.microsoft.com/zh-tw/entra/identity-platform/v2-protocols
- https://learn.microsoft.com/zh-tw/entra/architecture/auth-oidc