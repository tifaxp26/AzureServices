![AzureCLI_APIM_01.png](/.attachments/AzureCLI_APIM_01-7407337c-9185-47ea-b4a0-1bb47519ed86.png)

![AzureCLI_APIM_02.png](/.attachments/AzureCLI_APIM_02-a9b3eb93-8701-4924-9000-1eb06387230b.png)

![AzureCLI_APIM_03.png](/.attachments/AzureCLI_APIM_03-08fa366e-2372-4784-9d53-9c4c854ec94f.png)

``` PowerShell
# PowerShell
# 設定 Azure AD 憑證
$tenantId = "<YourTenantId>"
$clientId = "<YourClientId>"
$clientSecret = "<YourClientSecret>"
$resource = "https://management.azure.com/"

 

# 登入 Azure AD
Connect-AzAccount -TenantId $tenantId -ClientId $clientId -ClientSecret $clientSecret

 

# 設定 APIM 相關信息
$apimResourceGroup = "<APIMResourceGroup>"
$apimName = "<APIMName>"

 

# 定義需要更新的 OpenAPI 定義文件路徑列表
$openApiDefinitions = @(
    "path/to/your/definition1.json",
    "path/to/your/definition2.json",
    # 添加更多路徑...
)

 

# 更新每個 OpenAPI 定義
foreach ($definitionPath in $openApiDefinitions) {
    $apiId = "<APIId>"  # 根據你的情況設定 API Id
    $newOpenApiDefinition = Get-Content -Path $definitionPath -Raw

 

    # 呼叫管理 API，上傳新的 OpenAPI 定義
    $apiVersion = "" #這裡可能要去 apim 看其他規則
    $uri = "https://management.azure.com/subscriptions/$subscriptionId/resourceGroups/$apimResourceGroup/providers/Microsoft.ApiManagement/service/$apimName/apis/$apiId/openApi?api-version=$apiVersion"
    $body = @{
        swaggerUrl = ""
        value = $newOpenApiDefinition
    } | ConvertTo-Json

 

    Invoke-AzRestMethod -Uri $uri -Method Put -Headers @{ "Authorization" = "Bearer $((Get-AzAccessToken -ResourceUrl $resource).Token)" } -Body $body

 

#################################################
#下面這些是刪除舊的api相關的

 

# 設定 APIM 相關信息
$apimResourceGroup = "<APIMResourceGroup>"
$apimName = "<APIMName>"
$apiId = "<APIId>"

 

# 取得現有的 API 版本列表
$apiVersionList = Get-AzApiManagementApiVersionSet -ResourceGroupName $apimResourceGroup -ServiceName $apimName -ApiId $apiId

 

# 保留的最新版本數量
$keepLatestVersions = <數字>

 

# 刪除舊的 API 版本
$versionsToDelete = $apiVersionList.Versions | Sort-Object -Descending | Select-Object -Skip $keepLatestVersions
foreach ($version in $versionsToDelete) {
    Write-Host "Deleting API version $version"
    Remove-AzApiManagementApiVersion -ResourceGroupName $apimResourceGroup -ServiceName $apimName -ApiId $apiId -Version $version
}
```






