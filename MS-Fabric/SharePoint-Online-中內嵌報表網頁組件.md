[[_TOC_]]


在 SharePoint Online 中內嵌報表網頁組件
=============================

SharePoint Online 的 Power BI 報表網頁組件，可讓您在 SharePoint Online 的網頁中內嵌互動式 Power BI 報表。
當您使用 [內嵌於 SharePoint Online] 選項時，內嵌的報表會透過[資料列層級安全性 (RLS)](https://learn.microsoft.com/zh-tw/fabric/security/service-admin-row-level-security)，遵循所有項目許可權和資料安全性，因此您可輕鬆地建立安全的內部入口網站。

[](https://learn.microsoft.com/zh-tw/power-bi/collaborate-share/service-embed-report-spo#prerequisites)

先決條件
----

若要讓**在 SharePoint Online 內嵌報表**運作：
*   SharePoint Online 的 Power BI Web 組件需要[新式網頁](https://support.office.com/article/Allow-or-prevent-creation-of-modern-site-pages-by-end-users-c41d9cc8-c5c0-46b4-8b87-ea66abc6e63b)。
*   若要使用內嵌報表，使用者必須登入 Power BI 服務，才能啟用其 Power BI 授權。
*   若要在 SharePoint Online 中內嵌網頁元件，您需要 Power BI Pro 或 Premium Per User (PPU) 授權。
*   針對 [組織內嵌（使用者擁有數據）](https://learn.microsoft.com/zh-tw/power-bi/developer/embedded/embedded-analytics-power-bi#embed-for-your-organization) 客戶，需要相當於 [F64 或更新版本的](https://learn.microsoft.com/zh-tw/fabric/enterprise/licenses#capacity-and-skus) SKU，才能讓免費的 Power BI 使用者取用報表。 如果您有小於 F64 的 SKU，則檢視內嵌內容的每個使用者都需要 Pro 授權或 Premium Per User （PPU）。
*   對於使用< c0 >嵌入式功能（應用程式擁有資料）的客戶，終端用戶不需要任何授權要求。
*   SharePoint 內嵌現在在空中間距環境中受到支援。


內嵌報表
----

若要將報表嵌入 SharePoint Online，您必須取得報表 URL，並將它與 SharePoint Online 的 Power BI Web 組件搭配使用。

[](https://learn.microsoft.com/zh-tw/power-bi/collaborate-share/service-embed-report-spo#get-a-report-url)

### 取得報表 URL

1.  開啟 Power BI 服務中的報表。
    
2.  在 [檔案] 功能表上，選取 [內嵌報表]> [SharePoint Online]。
    ![顯示 [更多選項] 功能表的螢幕擷取畫面，其中已醒目提示 SharePoint Online。](https://learn.microsoft.com/zh-tw/power-bi/collaborate-share/media/service-embed-report-spo/power-bi-more-options-sharepoint-online.png)
    
3.  從對話方塊複製報表 URL。
    ![[內嵌連結] 對話框的螢幕擷取畫面，其中已醒目提示報表連結。](https://learn.microsoft.com/zh-tw/power-bi/collaborate-share/media/service-embed-report-spo/powerbi-embed-link-sharepoint.png)
    

[](https://learn.microsoft.com/zh-tw/power-bi/collaborate-share/service-embed-report-spo#add-the-power-bi-report-to-a-sharepoint-online-page)

### 將 Power BI 報表新增至 SharePoint Online 頁面

1.  在 SharePoint Online 中開啟目標頁面，然後選取 [編輯]。
    ![SharePoint 編輯頁面的螢幕擷取畫面，其中已醒目提示編輯選項。](https://learn.microsoft.com/zh-tw/power-bi/collaborate-share/media/service-embed-report-spo/powerbi-sharepoint-edit-page.png)
    或者在 SharePoint Online 中選取 [頁面]>[+ 新增]>[網站頁面]，以建立新的現代化網站頁面。
    ![SharePoint 視窗的螢幕擷取畫面。頁面會在瀏覽窗格中醒目提示。已選取 [網站] 頁面。](https://learn.microsoft.com/zh-tw/power-bi/collaborate-share/media/service-embed-report-spo/powerbi-sharepoint-new-page.png)
    
2.  選取**+**下拉功能表中的 []。 在 [資料分析] 區段中，選取 **Power BI** 網頁元件。
    ![[資料分析] 區段的螢幕擷取畫面，其中顯示已選取 Power BI。](https://learn.microsoft.com/zh-tw/power-bi/collaborate-share/media/service-embed-report-spo/powerbi-sharepoint-new-web-part.png)
    
3.  選取**新增報告**。
    ![SharePoint 新增報表對話方塊的螢幕擷取畫面，要求您在頁面上包含報表，並顯示 [新增報表] 按鈕。](https://learn.microsoft.com/zh-tw/power-bi/collaborate-share/media/service-embed-report-spo/add-powerbi-report.png)
    
4.  將先前複製的報表 URL 貼入 **Power BI 報表連結**欄位中。 報表會自動載入。
    ![醒目提示 Power BI 報表連結的 SharePoint 新網頁元件屬性螢幕擷取畫面。](https://learn.microsoft.com/zh-tw/power-bi/collaborate-share/media/service-embed-report-spo/powerbi-sharepoint-new-web-part-properties.png)
    
5.  選取 [發佈]，讓 SharePoint Online 使用者能看見您所做的變更。
    ![Power Power BI 報表連結的螢幕擷取畫面，其中顯示已選取 [發佈] 選項。](https://learn.microsoft.com/zh-tw/power-bi/collaborate-share/media/service-embed-report-spo/powerbi-sharepoint-report-loaded.png)


授與報告存取權
-------

在 SharePoint Online 中內嵌報表，並不會自動授與使用者檢視報表的權限，您必須設定 Power BI 的檢視權限。

 重要
請務必檢閱可以看到 Power BI 服務內報表的成員，並將存取權授與沒有列出的成員。

有兩種方式可提供 Power BI 的報表存取權。

[](https://learn.microsoft.com/zh-tw/power-bi/collaborate-share/service-embed-report-spo#in-a-microsoft-365-group)

### 在 Microsoft 365 群組中

如果使用 Microsoft 365 群組來建置 SharePoint Online 小組網站，將使用者列為 **Power BI 服務內工作區**及 **SharePoint 頁面**的成員。

[](https://learn.microsoft.com/zh-tw/power-bi/collaborate-share/service-embed-report-spo#share-directly-with-users)

### 直接與使用者共用

在應用程式中內嵌報表，並將其直接與使用者共用。

 注意
*   您需要 Power BI Pro 或 Premium Per User (PPU) 授權，才能在工作區中建立儀表板。
*   若要與「Microsoft 免費使用者」共用，您需要將該工作區設在「Premium 容量」中。

1.  在工作區中建立報表。
    
2.  發佈應用程式並加以安裝。 您必須安裝應用程式，使其可以存取用於在 SharePoint Online 中進行內嵌的報表 URL。
    
3.  所有使用者也都需要安裝應用程式。 您也可以使用**自動安裝應用程式**功能。 在 Power BI 管理入口網站中，系統管理員可以啟用[推送應用程式](https://learn.microsoft.com/zh-tw/fabric/admin/service-admin-portal-app#push-apps-to-end-users)，讓應用程式預安裝給使用者。
    ![Power BI 管理入口網站的螢幕擷取畫面，其中已自動選取 [安裝應用程式]。](https://learn.microsoft.com/zh-tw/power-bi/collaborate-share/media/service-embed-report-spo/install-app-automatically.png)
    
4.  開啟應用程式並前往報表。
    
5.  從應用程式安裝的報表複製內嵌報表 URL。 請勿使用來自工作區的原始報表 URL。
    
6.  在 SharePoint Online 中建立新的小組網站。
    
7.  將先前複製的報表 URL 新增至 Power BI Web 組件。
    
8.  新增要使用您建立的 Power BI 應用程式中 SharePoint Online 頁面上之資料的所有使用者和/或群組。
    
     注意
    若要看到 SharePoint 頁面上的報表，使用者或群組需要存取 SharePoint Online 頁面，及 Power BI 應用程式中的報表。
    
現在使用者可以移至 SharePoint Online 中的小組網站，並在頁面上檢視報表。

[](https://learn.microsoft.com/zh-tw/power-bi/collaborate-share/service-embed-report-spo#multifactor-authentication)

多重要素驗證
------

如果您的 Power BI 環境需要您使用多重要素驗證進行登入，您可能會受要求使用安全性裝置進行登入，以驗證您的身分識別。 如果您未使用多重要素驗證登入 SharePoint Online，就會發生這種情況。 您的 Power BI 環境需要安全性裝置來驗證帳戶。

 注意
Power BI 不支援使用 Microsoft Entra ID 2.0 的多重要素驗證。 使用者會看到一則錯誤訊息。 如果使用者使用其安全性裝置再次登入 SharePoint Online ，則可以檢視報表。

[](https://learn.microsoft.com/zh-tw/power-bi/collaborate-share/service-embed-report-spo#web-part-settings)

網頁組件設定
------

這裡是您可以針對 SharePoint Online 的 Power BI Web 組件調整的設定：
![SharePoint 新增網頁元件屬性對話方塊的螢幕擷取畫面，其中已醒目提示 Power BI 報表連結。](https://learn.microsoft.com/zh-tw/power-bi/collaborate-share/media/service-embed-report-spo/powerbi-sharepoint-web-part-properties.png)

展開資料表

| 屬性 | 說明 |
| --- | --- |
| 頁面名稱 | 設定 Web 組件的預設頁面。 從下拉式清單中選取一個值。 如果沒有顯示任何頁面，可能是您的報表只有一個頁面，或您所貼上的 URL 包含頁面名稱。 從 URL 移除報表區段，以選取特定頁面。 |
| 顯示器 | 調整報表納入 SharePoint Online 頁面的方式。 |
| 顯示導覽窗格 | 顯示或隱藏頁面導覽窗格。 |
| 顯示篩選窗格 | 顯示或隱藏篩選窗格。 |

[](https://learn.microsoft.com/zh-tw/power-bi/collaborate-share/service-embed-report-spo#reports-that-dont-load)

未載入的報表
------

如果未在 Power BI Web 組件內載入您的報表，您可能會看到下列訊息：
![SharePoint 頁面的螢幕擷取畫面，其中 Power Bi 報表顯示內容無法使用訊息。](https://learn.microsoft.com/zh-tw/power-bi/collaborate-share/media/service-embed-report-spo/powerbi-sharepoint-report-not-found.png)
有兩個常見的原因會導致此訊息的出現。
1.  您沒有報表存取權。
2.  報表已刪除。
請連絡 SharePoint Online 頁面擁有者，以協助解決問題。

[](https://learn.microsoft.com/zh-tw/power-bi/collaborate-share/service-embed-report-spo#licensing)

授權
--

在 SharePoint 中檢視報表的使用者需要 **Power BI Pro 授權或 Premium Per User (PPU) 授權**，或者內容需要位於 **[Power BI Premium 容量 (EM 或 P SKU)](https://learn.microsoft.com/zh-tw/power-bi/enterprise/service-premium-what-is#capacities-and-skus)** 的工作區中。

[](https://learn.microsoft.com/zh-tw/power-bi/collaborate-share/service-embed-report-spo#known-issues-and-limitations)

已知問題與限制
-------

*   錯誤: 「發生錯誤，嘗試登出再登入，然後重新前往此頁面。 相互關聯識別碼: 未定義，HTTP 回應狀態: 400，伺服器錯誤碼 10001，訊息: 缺少重新整理權杖」
    如果您收到這個錯誤，請嘗試下列其中一個步驟來進行疑難排解：
    *   登出再登入 SharePoint。 請務必關閉所有瀏覽器視窗，然後再重新登入。
        
    *   若您的使用者帳戶需要多重要素驗證 (MFA)，請使用 MFA 裝置 (手機應用程式、智慧卡等等) 登入 SharePoint。
        
*   不支援 Azure B2B 來賓用戶帳戶，無論是個人或群組成員。 使用者看到的 Power BI 標誌會顯示載入的部分，但不會顯示報表。
    
*   檢視內嵌在 SharePoint Online 中的 Power BI 報表時，用戶無法切換 Power BI 環境。
    
*   Power BI 與 SharePoint Online 支援的當地語系化語言不盡相同。 因此，您可能不會在內嵌報表中看到適當的當地語系化。
    
*   如果您使用 Internet Explorer 10，可能會遇到問題。 此處的連結[支援 Power BI 的瀏覽器](https://learn.microsoft.com/zh-tw/power-bi/fundamentals/power-bi-browsers)。
    
*   此 Web 組件不支援傳統的 SharePoint 伺服器。
    
*   SharePoint Online 網頁元件不支援 [URL 篩選條件](https://learn.microsoft.com/zh-tw/power-bi/collaborate-share/service-url-filters)。
    
*   您無法使用 Power BI 網頁元件來檢視或存取內嵌在 SharePoint 網站頁面中的 Power BI Apps。 若要存取內嵌的Power BI報表，請先在Power BI服務中存取應用程式，再存取SharePoint網站頁面。




[在 SharePoint Online 中內嵌報表網頁組件 - Power BI | Microsoft Learn](https://learn.microsoft.com/zh-tw/power-bi/collaborate-share/service-embed-report-spo)
