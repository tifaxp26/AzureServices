[[_TOC_]]

## 檢查內部套件網路
![image.png](/.attachments/image-5505d8b7-2c23-4ace-a373-18f97a48a726.png)

## Maven 參數說明
![image.png](/.attachments/image-d7e14077-2985-408e-9c46-c03f7e9c0a8b.png)

## Options
-X -B -s $(Agent.BuildDirectory)/s/settings.xml
![image.png](/.attachments/image-30e0d372-9bd9-4373-9050-05466732ae98.png)

## Advance
選擇JDK版本
設定 MAVEN_OPTS
Dmaven.test.skip=true -Xmx2048m -Xms2048m -Xss64m 
![image.png](/.attachments/image-9dac27c5-d086-4cef-8de6-5c3614c24f5f.png)

## Build ok
![image.png](/.attachments/image-8762a77f-cfbd-402d-8868-de21d6130e4a.png)


## 參考文件

https://stackoverflow.com/questions/66096856/replace-settings-xml-in-azure-devops-task-group

https://medium.com/@openthedidi2004/maven%E7%AD%86%E8%A8%98-%E4%BA%8C-settings-xml-fd788d7f11ed

https://stackoverflow.com/questions/72529113/use-settings-file-in-azure-dev-ops-pipeline-maven-task-to-resolve-dependencies