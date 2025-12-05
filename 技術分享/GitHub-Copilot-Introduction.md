## GitHub Copilot 簡介

[[_TOC_]]
> GitHub Copilot是 GitHub 和 OpenAI 合作開發的一個人工智慧工具，使用者在使用Visual Studio Code、Microsoft Visual Studio、Vim、Cursor或 JetBrains整合式開發環境時可以通過GitHub Copilot自動補全代碼

GitHub Copilot 是世界上首個大規模 AI 開發人員工具，可協助您用更少的精力更快地撰寫程式碼。 GitHub Copilot 從註解和程式碼中提取內容，即時建議個別行和整個函式。

研究發現，GitHub Copilot 可以協助開發人員更快地編碼，專注於解决更大的問題，順暢工作的時間更長，並對自己的工作感到更滿足。

GitHub Copilot 的產生式預先訓練語言模型由 OpenAI 建立，由 OpenAI Codex 提供。 Visual Studio Code、Visual Studio、Neovim 和整合式開發環境 (IDE) 的 JetBrains 套件有擴充可用。

[GitHub Copilot 簡介]: https://learn.microsoft.com/zh-tw/training/modules/introduction-to-github-copilot/	"Microsoft GitHub Copilot 訓練"
[GitHub Copilot WIKI]: https://zh.wikipedia.org/zh-tw/GitHub_Copilot	"WIKI-GitHub Copilot"

------



## GitHub Copilot 運用時機

![image.png](/.attachments/image-2a614d2a-ae8f-45bd-82be-c91caeb27612.png)

#### 執行 : Coding

1. Convert comments to code 將註解轉換為代碼
2. Autofill for repetitive code  自動填充重複使用的代碼
3. Show alternatives 顯示替代方案

#### 單元測試時 : Unit testing

1. 單元測試 
2. 查找代碼錯誤 
3. Debugging 
4. Code review 
5. AI Pull Requests

#### 維護時 : Maintenance

1. Refactoring code 重構
2. Reviewing code 檢閱
3. Documentation 應該是 Code documentation 指記錄代碼過程



## GitHub Copilot - 使用於 Visual Studio

[關於適用于 Visual Studio 的 GitHub Copilot 擴充功能 - Visual Studio (Windows) | Microsoft Learn](https://learn.microsoft.com/zh-tw/visualstudio/ide/visual-studio-github-copilot-extension?view=vs-2022)

### 安裝條件

- Visual Studio 2022 17.4.4 版或更新版本 

- GitHub Copilot 訂用帳戶。
- 在功能表列上，選取 [ **擴充 > 功能管理擴充功能]。**
- 在 [搜尋] 方塊中，輸入 「GitHub Copilot」。
- 選取 **GitHub Copilot** 擴充功能，然後選取 [ **下載** ] 按鈕。
- 重新開機 Visual Studio 以完成安裝程式。

------



## 重點

### GitHub Copilot 的運作方式

> 用註解的方式建構需求程式

![Animated screenshot that shows the code completion capabilities of the GitHub Copilot extension.](https://learn.microsoft.com/zh-tw/visualstudio/ide/media/vs-2022/github-copilot-extension-example.gif?view=vs-2022)

1. 註解的方式建構需求程式
2. 預測接下來撰寫的程式
3. 顯示替代方案



### GitHub Copilot 的 重構

> 是適用于 Visual Studio 的 AI 型程式碼完成延伸模組，利用大量公開可用的程式碼資料集來提供內容感知程式碼建議、程式碼片段，甚至是整個函式。 相較于 IntelliCode 和 IntelliSense，它可搭配許多程式設計語言使用，並提供更進階的協助。



[Getting started with GitHub Copilot - GitHub Docs](https://docs.github.com/en/copilot/getting-started-with-github-copilot?tool=visualstudio)

## 定價

[**GitHub Copilot 定價** ](https://github.com/pricing)

![image.png](/.attachments/image-4ca6b983-3abc-4a02-a192-a354d5032654.png)



**[企業版 - VS Studio**](https://visualstudio.microsoft.com/zh-hant/vs/pricing/?tab=enterprise)

![image.png](/.attachments/image-e0195063-6617-4344-b644-1f9b26a14275.png)

## Reference resource:

·   [Github Enterprise Overview](https://nam06.safelinks.protection.outlook.com/?url=https%3A%2F%2Fdocs.github.com%2Fen%2Fenterprise-cloud%40latest%2Fadmin%2Foverview%2Fabout-github-for-enterprises&data=05|01|vickyliu%40microsoft.com|6f25b3e5e4e84df6293a08db4d3120df|72f988bf86f141af91ab2d7cd011db47|1|0|638188646666907989|Unknown|TWFpbGZsb3d8eyJWIjoiMC4wLjAwMDAiLCJQIjoiV2luMzIiLCJBTiI6Ik1haWwiLCJXVCI6Mn0%3D|3000|||&sdata=PIaAhAMsJ0tJJrGEIb%2Bio3C8tO2a7PCVi9xQvHtRNmM%3D&reserved=0)

·   [Github Advanced Security](https://nam06.safelinks.protection.outlook.com/?url=https%3A%2F%2Fdocs.github.com%2Fen%2Fenterprise-cloud%40latest%2Fget-started%2Flearning-about-github%2Fabout-github-advanced-security&data=05|01|vickyliu%40microsoft.com|6f25b3e5e4e84df6293a08db4d3120df|72f988bf86f141af91ab2d7cd011db47|1|0|638188646666907989|Unknown|TWFpbGZsb3d8eyJWIjoiMC4wLjAwMDAiLCJQIjoiV2luMzIiLCJBTiI6Ik1haWwiLCJXVCI6Mn0%3D|3000|||&sdata=MrYlkmmPjRriTGGOcc%2BoRjTbpery1js%2BRlJiueIDWrY%3D&reserved=0)

·   [Copilot Product Features and FAQ](https://nam06.safelinks.protection.outlook.com/?url=https%3A%2F%2Fgithub.com%2Ffeatures%2Fcopilot&data=05|01|vickyliu%40microsoft.com|6f25b3e5e4e84df6293a08db4d3120df|72f988bf86f141af91ab2d7cd011db47|1|0|638188646666907989|Unknown|TWFpbGZsb3d8eyJWIjoiMC4wLjAwMDAiLCJQIjoiV2luMzIiLCJBTiI6Ik1haWwiLCJXVCI6Mn0%3D|3000|||&sdata=IQZpxKmn%2Bc2Q%2BnZyfXGOE7BpBwCixMGcNHwXbp8317U%3D&reserved=0) 

·   [Quantifying the impact of Copilot](https://nam06.safelinks.protection.outlook.com/?url=https%3A%2F%2Fgithub.blog%2F2022-09-07-research-quantifying-github-copilots-impact-on-developer-productivity-and-happiness%2F&data=05|01|vickyliu%40microsoft.com|6f25b3e5e4e84df6293a08db4d3120df|72f988bf86f141af91ab2d7cd011db47|1|0|638188646666907989|Unknown|TWFpbGZsb3d8eyJWIjoiMC4wLjAwMDAiLCJQIjoiV2luMzIiLCJBTiI6Ik1haWwiLCJXVCI6Mn0%3D|3000|||&sdata=gD0XaGmd5FcwELT6YsispmJ1sSLR%2BMUo4G8xIvEgV%2Fk%3D&reserved=0) - Statistics shared on call 

·   [Research: How GitHub helps improve developer productivity](https://nam06.safelinks.protection.outlook.com/?url=https%3A%2F%2Fgithub.blog%2F2022-07-14-research-how-github-copilot-helps-improve-developer-productivity%2F&data=05|01|vickyliu%40microsoft.com|6f25b3e5e4e84df6293a08db4d3120df|72f988bf86f141af91ab2d7cd011db47|1|0|638188646666907989|Unknown|TWFpbGZsb3d8eyJWIjoiMC4wLjAwMDAiLCJQIjoiV2luMzIiLCJBTiI6Ik1haWwiLCJXVCI6Mn0%3D|3000|||&sdata=GhkNMLET4%2FOaqXbLUwI79Q%2F1qdisIRiV2GyuzbVJyaA%3D&reserved=0) - 

·   [GitHub Next ](https://nam06.safelinks.protection.outlook.com/?url=https%3A%2F%2Fgithubnext.com%2F&data=05|01|vickyliu%40microsoft.com|6f25b3e5e4e84df6293a08db4d3120df|72f988bf86f141af91ab2d7cd011db47|1|0|638188646666907989|Unknown|TWFpbGZsb3d8eyJWIjoiMC4wLjAwMDAiLCJQIjoiV2luMzIiLCJBTiI6Ik1haWwiLCJXVCI6Mn0%3D|3000|||&sdata=TVSAUK%2FZFMXLNzFQ7Zjz0IvfjHO4fhJAv3zNM2jzxyg%3D&reserved=0)- Future of Copilot. 

·   [Privacy Model and FAQ for Copilot](https://nam06.safelinks.protection.outlook.com/?url=https%3A%2F%2Fgithub.blog%2F2023-02-14-github-copilot-now-has-a-better-ai-model-and-new-capabilities%2F&data=05|01|vickyliu%40microsoft.com|6f25b3e5e4e84df6293a08db4d3120df|72f988bf86f141af91ab2d7cd011db47|1|0|638188646666907989|Unknown|TWFpbGZsb3d8eyJWIjoiMC4wLjAwMDAiLCJQIjoiV2luMzIiLCJBTiI6Ik1haWwiLCJXVCI6Mn0%3D|3000|||&sdata=Z%2Fe7t%2Fmh1MMY8C9X2DFUGXBjH73QAFb9OcYdyly8HOc%3D&reserved=0) -

·   [Vulnerability scanning in Copilot ](https://nam06.safelinks.protection.outlook.com/?url=https%3A%2F%2Fgithub.blog%2F2023-02-14-github-copilot-now-has-a-better-ai-model-and-new-capabilities%2F&data=05|01|vickyliu%40microsoft.com|6f25b3e5e4e84df6293a08db4d3120df|72f988bf86f141af91ab2d7cd011db47|1|0|638188646666907989|Unknown|TWFpbGZsb3d8eyJWIjoiMC4wLjAwMDAiLCJQIjoiV2luMzIiLCJBTiI6Ik1haWwiLCJXVCI6Mn0%3D|3000|||&sdata=Z%2Fe7t%2Fmh1MMY8C9X2DFUGXBjH73QAFb9OcYdyly8HOc%3D&reserved=0) - Security features / Benefits 

·   [Copilot Billing Information](https://nam06.safelinks.protection.outlook.com/?url=https%3A%2F%2Fdocs.github.com%2Fen%2Fbilling%2Fmanaging-billing-for-github-copilot%2Fabout-billing-for-github-copilot&data=05|01|vickyliu%40microsoft.com|6f25b3e5e4e84df6293a08db4d3120df|72f988bf86f141af91ab2d7cd011db47|1|0|638188646666907989|Unknown|TWFpbGZsb3d8eyJWIjoiMC4wLjAwMDAiLCJQIjoiV2luMzIiLCJBTiI6Ik1haWwiLCJXVCI6Mn0%3D|3000|||&sdata=ymU6cvTnQwvQcK7mVyR%2FguMoHk0Ns5qbW4pl7z%2Bss5o%3D&reserved=0) this explains the pricing. 

·   [Prerequisites for Copilot](https://nam06.safelinks.protection.outlook.com/?url=https%3A%2F%2Fdocs.github.com%2Fen%2Fcopilot%2Fgetting-started-with-github-copilot%2Fgetting-started-with-github-copilot-in-a-jetbrains-ide%23prerequisites&data=05|01|vickyliu%40microsoft.com|6f25b3e5e4e84df6293a08db4d3120df|72f988bf86f141af91ab2d7cd011db47|1|0|638188646666907989|Unknown|TWFpbGZsb3d8eyJWIjoiMC4wLjAwMDAiLCJQIjoiV2luMzIiLCJBTiI6Ik1haWwiLCJXVCI6Mn0%3D|3000|||&sdata=W5VTZ0c99cdC09GCLAA79HJFRYuuMKTsojOXo%2FV06f8%3D&reserved=0) 





