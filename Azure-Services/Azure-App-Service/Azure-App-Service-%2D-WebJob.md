[[_TOC_]]

https://github.com/projectkudu/kudu/wiki/WebJobs
# 概述
WebJobs 是一項 Azure App Service 功能，可讓您在與 Web 應用程式、API 應用程式或行動應用程式相同的執行個體中執行程式或指令碼。 
使用 WebJobs 不需要額外費用。

Azure WebJobs SDK 可以搭配 WebJobs 使用來簡化程式設計工作。 
Linux 上的 App Service尚不支援 WebJobs。 如需詳細資訊，請參閱 什麼是 WebJobs SDK。


# 指令碼或程式支援的檔案類型
支援下列檔案類型：

.cmd、.bat、.exe (使用 Windows 命令提示字元)
.ps1 (使用 PowerShell)
.sh (使用 Bash)
.php (使用 PHP)
.py (使用 Python)
.js (使用 Node.js)
.jar (使用 Java)

------


# 專案說明
## 排程定義檔 Settings.job
必要檔案，定義工作排程
![image.png](/.attachments/image-f2f5a9cb-2184-479a-aa0a-4cdf6739cc2c.png)


# CI - 部分 (DevOps **Pipelines**)

------

## 1. 確保使用 **[Asp.net](http://Asp.net) Core 模板**

------

![image.png](/.attachments/image-b21a56f5-0510-4282-a044-4ccca3d60a4c.png)

![image.png](/.attachments/image-117ad5ec-f1e0-489b-bf57-a4c28a612364.png)

![image.png](/.attachments/image-d547b4af-f25d-44eb-8a5e-8ea4bb46211c.png)

## 2. **Pipeline**

------

- Job 裡面的 **Parameters** 改成你的 **專案檔**

![image.png](/.attachments/image-d9d08e6b-ac98-46bd-8d1c-c8e5126f1040.png)

## 3. **Publish** :

------

- Job 裡面的 **Arguments** 改成

  > -configuration $(BuildConfiguration) --output $(Build.BinariesDirectory)/publish_output/App_Data/jobs/triggered

- 取消勾選 **[Publish web projects]** 以及 **[Zip published projects]**

![image.png](/.attachments/image-b62dfdac-ad88-48a9-8139-ce8b345d033a.png)

## 4. Archive files

------

- Job 裡面的 **Root folder or file to archive** 改成

  > $(Build.BinariesDirectory)/publish_output

- 取消勾選 **[Prepend root folder name to archive paths]**

![image.png](/.attachments/image-0aa868ab-d0f2-46a0-bc0d-882f3c63cd17.png)

## 5. (選用) 自動觸發

![image.png](/.attachments/image-b801bd17-6920-4be4-bbaa-bd7e90b12197.png)

# CD - 部分 (DevOps Releases)

------

![image.png](/.attachments/image-16025268-cb35-4b4b-85c2-fc56a11d35a0.png)

**分為 : Artifacts 與 Stages**

------

1. **Artifacts 是去取剛剛存的ZIP**

   > **Artifacts 右上角的 閃電 可以做 自動觸發**
   >
   > ![image.png](/.attachments/image-5f2ed0de-364f-4faa-ba1d-928997c08876.png)

2. **Stages 是去佈署的動作**

   - 新增 **[Azure App Service deployment] 並 設定 連結**

     ![image.png](/.attachments/image-eadddf8f-c454-496f-9ccb-93e0c01ad6fb.png)

   - **Deploy Azure App Service 修改 [Additional Deployment Options]**
![image.png](/.attachments/image-f2ef87c8-614e-4513-b333-97d4f5615cd0.png)

# 成果圖

------
![image.png](/.attachments/image-0903db97-306e-44a4-b46d-cf6c0401bd92.png)
![image.png](/.attachments/image-fd5689c7-91c2-4488-ab62-d3b5f8bb9b0b.png)
![image.png](/.attachments/image-60bda4de-1d2d-4d8a-837e-2055fd391da4.png)