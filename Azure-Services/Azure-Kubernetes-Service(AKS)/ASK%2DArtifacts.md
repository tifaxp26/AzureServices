[[_TOC_]]


# 使用 自定義Artifacts

## 建立PAT for Packaging
取得 token 並記錄保存
![image.png](/.attachments/image-48273d2d-5970-43a8-beb8-e08d81942f09.png)

## 到 Pipelines 設定 Variables
- 使用者
- Token
![image.png](/.attachments/image-e3a3c618-b9cd-4a95-991c-f56e53cea7a9.png)

## YAML 設定變數
- 使用者: arg1 
- Token: arg2
![image.png](/.attachments/image-0c60743b-a8b5-4dea-a400-dbf6d490d926.png)

## Docker Build
Docker V2版本命令，不能使用 BuildAndPush，因為無法傳入參數。
要分開兩個步驟執行 Build、Push

- Build 加入參數，如下
`--build-arg name=value`
![image.png](/.attachments/image-1fd2c7fa-0559-4de1-8d3f-67c0cf36a078.png)


# dockerfile 接收參數
## ARG、ENV 使用方式
```
FROM mcr.microsoft.com/dotnet/sdk:6.0 AS build
# 注意: 要在這個 FROM 之後，才接收到參數 
# 注意: 要在這個 FROM 之後，才能使用環境變數

ARG Nuget_CustomFeedUserName
ARG Nuget_CustomFeedPassword
# ENV CustomFeedUserName=$(Nuget_CustomFeedUserName)
# ENV CustomFeedPassword=$(Nuget_CustomFeedPassword)
```
![image.png](/.attachments/image-7bdaf8c1-8bdc-4450-b539-fc06af2bfc3c.png)

## nuget.config 接收參數
1. 定義 packageSourceCredentials
1. 接收變數 %Nuget_CustomFeedUserName%
![image.png](/.attachments/image-7208ec96-e425-4f89-bb7e-49417de0fd80.png)
