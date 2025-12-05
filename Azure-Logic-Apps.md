[[_TOC_]]

# Deploying Logic App Standard resource through DevOps pipeline
![image.png](/.attachments/image-ca8df561-660c-42ea-8f6f-98c15c3b69b8.png)


# 建立邏輯應用程式
符合CAP規範，必須選用 標準版的工作流程服務方案
![image.png](/.attachments/image-e6b67e04-f062-4b1f-ba4d-e6e0f7e3ca71.png)


## 建立服務連線
建立目的端服務連線，注意: **要選 Service Pricinpal**
![image.png](/.attachments/image-0f341398-b45c-4398-8169-d373368e774a.png)
![image.png](/.attachments/image-d237197a-bd90-4592-a5a8-4a601564019a.png)
![image.png](/.attachments/image-31cee11c-9d31-433c-b30b-c8b75c78940c.png)
![image.png](/.attachments/image-31ed855f-7755-4012-8d5f-50777e2e74de.png)
完成
![image.png](/.attachments/image-c584747a-7e78-476a-b0b6-3ad14a403bc5.png)
Azure應用程式註冊也會建立一筆服務
![image.png](/.attachments/image-c1913279-a059-4cbf-bbb3-1a85df71920d.png)

## 執行 IaaC

定義環境變數 IaC variables
pipeline-vars.yml

```
variables:

  projectName: 'logicappsample'
  suffix: 'dev'

  # Resource Group
  resourceGroupLocation: 'SoutheastAsia'
  resourceGroupName: 'rg-logicapps-poc02'

  # Storage - make sure this is unique!
  storageName: 'stlogicappspoc02'
  appServicePlanName: 'APS-logicapps-poc02'

  #  Logic App
  logicAppName: 'logic-poc02'

  # API Connections - if you change this name, you must change the reference in workflow.json
  # blobConnectionName: 'azureblob'
  o365ConnectionName: 'office365'
  microsoftformsConnectionName: 'microsoftforms'

  # Project artifacts
  logicAppCIArtifactName: 'logicapp_publish_artifact'

```


建立 Azure Logic Apps 服務，包含名稱、定價、地區....
iac-pipeline.yml

```
trigger: none

pr: none

variables:
- name: artifactName
  value: deploy_artifacts
- name: devEnvironment
  value: dev

stages:
- stage: Builds
  displayName: 'Publish IaC Artifacts'
  jobs:
  - job: Build
    pool:
      vmImage: ubuntu-latest
    steps:
    - task: CopyFiles@2
      displayName: 'Copy ARM templates'
      inputs:
        sourceFolder: 'azure-devops-sample/deploy'
        targetFolder: '$(Build.ArtifactStagingDirectory)'
    - publish: '$(Build.ArtifactStagingDirectory)'
      artifact: $(artifactName)

- stage: DEV
  displayName: 'DEV Deployment'
  variables:
  - template: ./variables/pipeline-vars.yml
  jobs:
  - template: ./templates/template-iac-logicapp.yml
    parameters:
      environment: $(devEnvironment)
      # TODO: Fill in with the name of your Azure service connection
      serviceConnection: 'WALSIN-Azure-Sandbox-LogicApps'    

```

運行範本，會建立**環境**
template-iac-logicapp.yml
![image.png](/.attachments/image-d44b8f1f-938a-461f-b328-8d0a4ca50afa.png)
![image.png](/.attachments/image-3ae17786-2e64-4a27-8536-b9491dd0a4d0.png)

template-iac-logicapp.yml

```
# IaC Logic App resources deployment template

parameters:
- name: environment
  type: string
- name: serviceConnection
  type: string

jobs:
- deployment: deploy_logicapp_resources
  displayName: Deploy Logic App Resources
  pool:
    vmImage: ubuntu-latest
  environment: ${{ parameters.environment }}
  variables:
    deploymentMode: 'Incremental'
  strategy:
    runOnce:
      deploy:
        steps:
        - download: current
          artifact: $(artifactName)

        - task: AzureResourceGroupDeployment@2
          displayName: 'Deploy Logic App'
          inputs:
            azureSubscription: ${{ parameters.serviceConnection }}
            resourceGroupName: $(resourceGroupName)
            location: $(resourceGroupLocation)
            csmFile: '$(Pipeline.Workspace)/$(artifactName)/classic/logicapp-template.json'
            overrideParameters: '
              -location $(resourceGroupLocation)
              -environmentName ${{ parameters.environment }}
              -projectName $(projectName)
              -logicAppName $(logicAppName)
              -appServicePlanName $(appServicePlanName)
              -storageName $(storageName)'
            deploymentMode: $(deploymentMode)
            deploymentOutputs: 'LogicAppArmOutputs'

        - task: ARM Outputs@6
          displayName: 'ARM Outputs'
          inputs:
            ConnectedServiceNameSelector: 'ConnectedServiceNameARM'
            ConnectedServiceNameARM: ${{ parameters.serviceConnection }}
            resourceGroupName: $(resourceGroupName)
            whenLastDeploymentIsFailed: 'fail'

        - task: AzureResourceGroupDeployment@2
          displayName: 'Deploy Connectors'
          inputs:
            azureSubscription: ${{ parameters.serviceConnection }}
            resourceGroupName: $(resourceGroupName)
            location: $(resourceGroupLocation)
            csmFile: '$(Pipeline.Workspace)/$(artifactName)/connectors-template.json'
            overrideParameters: '
              -location $(resourceGroupLocation)              
              -connections_office365_name $(o365ConnectionName)
              -connections_microsoftforms_name $(microsoftformsConnectionName)
              -logicAppSystemAssignedIdentityTenantId $(logicAppSystemAssignedIdentityTenantId)
              -logicAppSystemAssignedIdentityObjectId $(logicAppSystemAssignedIdentityObjectId)'
            deploymentMode: $(deploymentMode)

        - task: ARM Outputs@6
          displayName: 'ARM Outputs Connections'
          inputs:
            ConnectedServiceNameSelector: 'ConnectedServiceNameARM'
            ConnectedServiceNameARM: ${{ parameters.serviceConnection }}
            resourceGroupName: $(resourceGroupName)
            whenLastDeploymentIsFailed: 'fail'

        - task: AzureCLI@2
          inputs:
            azureSubscription: ${{ parameters.serviceConnection }}
            scriptType: 'bash'
            scriptLocation: 'inlineScript'
            inlineScript: |
              az functionapp config appsettings set --name $(LAname) --resource-group  $(resourceGroupName) --settings "O365_CONNECTION_RUNTIMEURL=$(o365endpointurl)"                          
              az functionapp config appsettings set --name $(LAname) --resource-group  $(resourceGroupName) --settings "MICROSOFTFORMS_CONNECTION_RUNTIMEURL=$(microsoftformsendpointurl)"                          
              az functionapp config appsettings set --name $(LAname) --resource-group  $(resourceGroupName) --settings "WORKFLOWS_RESOURCE_GROUP_NAME=$(resourceGroupName)"              
            addSpnToEnvironment: true
            useGlobalConfig: true  

```
            

驗證、加上 連線範本

![image.png](/.attachments/image-38297c8d-b8e4-4b90-85aa-e0cc8b0e1bcc.png)
![image.png](/.attachments/image-7e795f94-2041-4d5a-9233-5273c92cc1f6.png)

connectors-template.json
```
{
	"$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
	"contentVersion": "1.0.0.0",
	"parameters": {
		"location": {
			"type": "string",
			"metadata": {
				"description": "The datacenter to use for the deployment."
			}
		},
		"logicAppSystemAssignedIdentityTenantId": {
			"type": "string"
		},
		"logicAppSystemAssignedIdentityObjectId": {
			"type": "string"
		},
		"connections_office365_name": {
			"defaultValue": "office365",
			"type": "String"
		},
		"connections_microsoftforms_name": {
			"defaultValue": "microsoftforms",
			"type": "String"
		}
	},
	"variables": {},
	"resources": [
		{
			"type": "Microsoft.Web/connections",
			"apiVersion": "2016-06-01",
			"name": "[parameters('connections_office365_name')]",
			"location": "[parameters('location')]",
			"kind": "V2",
			"properties": {
				"displayName": "office365",
				"api": {
					"id": "[concat('/subscriptions/',subscription().subscriptionId,'/providers/Microsoft.Web/locations/',parameters('location'),'/managedApis/',parameters('connections_office365_name'))]"
				}
			}
		},
		{
			"type": "Microsoft.Web/connections",
			"apiVersion": "2016-06-01",
			"name": "[parameters('connections_microsoftforms_name')]",
			"location": "[parameters('location')]",
			"kind": "V2",
			"properties": {
				"displayName": "microsoftforms",
				"api": {
					"id": "[concat('/subscriptions/',subscription().subscriptionId,'/providers/Microsoft.Web/locations/',parameters('location'),'/managedApis/',parameters('connections_microsoftforms_name'))]"
				}
			}
		},
		{
			"type": "Microsoft.Web/connections/accessPolicies",
			"apiVersion": "2016-06-01",
			"name": "[concat(parameters('connections_office365_name'),'/',parameters('logicAppSystemAssignedIdentityObjectId'))]",
			"location": "[parameters('location')]",
			"dependsOn": [
				"[resourceId('Microsoft.Web/connections', parameters('connections_office365_name'))]"
			],
			"properties": {
				"principal": {
					"type": "ActiveDirectory",
					"identity": {
						"tenantId": "[parameters('logicAppSystemAssignedIdentityTenantId')]",
						"objectId": "[parameters('logicAppSystemAssignedIdentityObjectId')]"
					}
				}
			}
		},
		{
			"type": "Microsoft.Web/connections/accessPolicies",
			"apiVersion": "2016-06-01",
			"name": "[concat(parameters('connections_microsoftforms_name'),'/',parameters('logicAppSystemAssignedIdentityObjectId'))]",
			"location": "[parameters('location')]",
			"dependsOn": [
				"[resourceId('Microsoft.Web/connections', parameters('connections_microsoftforms_name'))]"
			],
			"properties": {
				"principal": {
					"type": "ActiveDirectory",
					"identity": {
						"tenantId": "[parameters('logicAppSystemAssignedIdentityTenantId')]",
						"objectId": "[parameters('logicAppSystemAssignedIdentityObjectId')]"
					}
				}
			}
		}
	],
	"outputs": {
		"o365endpointurl": {
			"type": "string",
			"value": "[reference(resourceId('Microsoft.Web/connections', parameters('connections_office365_name')),'2016-06-01', 'full').properties.connectionRuntimeUrl]"
		},
		"microsoftformsendpointurl": {
			"type": "string",
			"value": "[reference(resourceId('Microsoft.Web/connections', parameters('connections_microsoftforms_name')),'2016-06-01', 'full').properties.connectionRuntimeUrl]"
		}
	}
}
```


      




完成
- 工作流程
- 連線

![image.png](/.attachments/image-2a97f331-8ac0-486c-84ee-51368a35a69d.png)
![image.png](/.attachments/image-b16a24c3-0a0f-43cb-9b65-d5a3569c9147.png)


## Workload CI
ci-pipeline.yml


```
trigger:
- main

pr: none

pool:
  vmImage: 'ubuntu-latest'

variables:
- template: variables/pipeline-vars.yml

jobs:
- job: logic_app_build
  displayName: 'Build and publish logic app'
  steps:
  - task: CopyFiles@2
    displayName: 'Create project folder'
    inputs:
      SourceFolder: '$(System.DefaultWorkingDirectory)/azure-devops-sample/logic'
      Contents: |
        **
      TargetFolder: 'project_output'

  - task: ArchiveFiles@2
    displayName: 'Create project zip'
    inputs:
      rootFolderOrFile: '$(System.DefaultWorkingDirectory)/project_output'
      includeRootFolder: false
      archiveType: 'zip'
      archiveFile: '$(Build.ArtifactStagingDirectory)/$(Build.BuildId).zip'
      replaceExistingArchive: true

  - task: PublishPipelineArtifact@1
    displayName: 'Publish project zip artifact'
    inputs:
      targetPath: '$(Build.ArtifactStagingDirectory)/$(Build.BuildId).zip'
      artifact: '$(logicAppCIArtifactName)'
      publishLocation: 'pipeline'
```



![image.png](/.attachments/image-5614a49e-ebd1-45b9-967d-f8c5cd5b9b49.png)

## Workload CD
cd-pipeline.yml


```
trigger: none

pr: none

variables:
- name: devEnvironment
  value: dev

resources:
  pipelines:
  - pipeline: cipipeline
    # TODO: Update with the name of your CI pipeline
    source: 'LogicAppStandard-CI'
    trigger:
      branches:
      - main

stages:
- stage: DEV
  displayName: 'DEV Deployment'
  variables:
  - template: variables/pipeline-vars.yml
  jobs:
  - deployment: deploy_logicapp_resources
    displayName: Deploy Logic App
    pool:
      vmImage: ubuntu-latest
    environment: $(devEnvironment)
    variables:
      deploymentMode: 'Incremental'
    strategy:
      runOnce:
        deploy:
          steps:
          - task: AzureAppServiceSettings@1
            displayName: Azure App Service Settings
            inputs:
              azureSubscription: 'WALSIN-Azure-Sandbox-LogicApps'
              appName: '$(logicAppName)'
              resourceGroupName: '$(resourceGroupName)'
              
              # Add the app settings for the connections 
            
              appSettings: |
                [
                  {
                    "name": "serviceBus_connectionString",
                    "value": "test",
                    "slotSetting": false
                    },
                    {
                    "name": "azureFunctionOperation_functionAppKey",
                    "value": "test",
                    "slotSetting": false
                    },
                    {
                    "name": "azureFunctionOperation_functionResourceURI",
                    "value": "test",
                    "slotSetting": false
                    },
                    {
                    "name": "azureFunctionOperation_functionTriggerURI",
                    "value": "test",
                    "slotSetting": false
                    }
                    
                ]
          - task: AzureFunctionApp@1
            displayName: 'Deploy logic app workflows'
            inputs:
              # TODO: Fill in with the name of your Azure service connection
              azureSubscription: 'WALSIN-Azure-Sandbox-LogicApps'
              appType: 'functionApp'
              appName: '$(logicAppName)'
              package: '$(Pipeline.Workspace)/cipipeline/$(logicAppCIArtifactName)/$(resources.pipeline.cipipeline.runID).zip'
              deploymentMethod: 'zipDeploy'
```



![image.png](/.attachments/image-af193e90-2919-4caa-badb-41fc6224cf0f.png)


## 完整 pipelines
AzureLogicApps目錄下有
- IaC
- CI
- CD
![image.png](/.attachments/image-06d1c57e-20c1-48b5-b50f-3e3eff726bd0.png)
![image.png](/.attachments/image-2b53b865-2fb9-4be2-8124-3d4592b389b1.png)

## 驗證
回到 Azure Portal
會發現目的Logic Apps產生 工作流程的服務，與連線的服務!!
![image.png](/.attachments/image-289952ad-c1fb-4635-bf94-590dbbc9a326.png)
![image.png](/.attachments/image-6abdc34b-b7b8-46f6-9e78-dc0f7fbceb34.png)

### 驗證API連線
IaC 服務建好後，要再檢查 連線是否完成授權

檢查連線/存取原則
![image.png](/.attachments/image-2a7c8082-8cdb-4433-b580-b422960bd9d5.png)
將新服務加入授權
![image.png](/.attachments/image-90f23360-1430-453a-ab32-7bbebc212235.png)
![image.png](/.attachments/image-a49741e5-5c66-4962-bce1-4a4f81f7ce63.png)
連線服務完成授權
![image.png](/.attachments/image-818f8845-c92a-4cd5-a80e-4edd499bcee3.png)



## 問題
當出現 服務 版本重複時，應該是 舊連線版本 與 新連線版本 衝突，請以新服務為主。
![image.png](/.attachments/image-2567bcaf-3232-4ad1-911b-c6f5ee72d3cc.png)


# 參考
- https://techcommunity.microsoft.com/t5/azure-integration-services-blog/deploying-logic-app-standard-resource-through-devops-pipeline/ba-p/3541427
- https://github.com/ShreeDivyaMV/LogicAppStandard/blob/master/azure-devops-sample/.pipelines/classic/templates/template-iac-logicapp.yml