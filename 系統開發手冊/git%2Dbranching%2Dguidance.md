[[_TOC_]]


採用 Git 分支策略


# 讓您的分支策略保持簡單
讓您的分支策略保持簡單。 從下列三個概念建置您的策略：

- 所有新功能和 Bug 修正均需使用功能分支。
- 使用提取要求將功能分支合併至主要分支。
- 保持高品質的最新主要分支。

擴充這些概念並避免錯錯的策略，將會導致小組的版本控制工作流程一致且容易遵循。

# 為您的工作使用功能分支
開發您的功能，並根據主要分支修正功能分支中的 Bug。 這些分支也稱為 主題分支。 功能分支會將進行中的工作與主要分支中已完成的工作隔離。 建立和維護 Git 分支的成本較低。 即使是小型修正和變更也應該有自己的功能分支。

<IMG  src="https://learn.microsoft.com/zh-tw/azure/devops/repos/git/media/branching-guidance/featurebranching.png?view=azure-devops"  alt="基本分支工作流程的影像"/>

為所有變更建立功能分支會讓檢閱歷程記錄變得簡單。 查看分支中所做的認可，並查看合併分支的提取要求。

# 依慣例命名您的功能分支
針對您的功能分支使用一致的命名慣例，以識別在分支中完成的工作。 您也可以在分支名稱中包含其他資訊，例如建立分支的人員。

## 命名功能分支的一些建議：

users/username/description
users/username/workitem
bugfix/description
feature/feature-name
feature/feature-area/feature-name
Hotfix/描述


