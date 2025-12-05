[[_TOC_]]



# Azure APIM 串接 Azure OpenAI Service

## 建立 Azure OpenAI 服務
![image.png](/.attachments/image-b4b386b2-d510-454a-a8f5-e8e94e6de5f3.png)

## Basic
選擇 AOAI Instance
選擇版本
顯示名稱/名稱
描述
基底URL
產品
![image.png](/.attachments/image-4b1313a5-acf8-4f58-a783-0bcabe7dba1c.png)

## 原則
設定管理 Token 消費計量
設定追蹤 Token 消費計量 並 記錄到 Application Insights
![image.png](/.attachments/image-fb5cca4e-30fe-4ab4-af32-3629f792f845.png)

## 完成
![image.png](/.attachments/image-3872b3b2-4298-42ff-ae0c-891d2dc33167.png)

## 調整設定

![image.png](/.attachments/image-473de7cd-2a69-4390-950e-ebb541c0b7ea.png)
### 變更 Header Name 為 Ocp-Apim-Subscription-Key
![image.png](/.attachments/image-6ec5dcaa-a500-4390-84a0-58ff18d71a41.png)
### 開啟 Application Insights 紀錄
![image.png](/.attachments/image-6c10f827-c436-40d1-8e2a-f86845a476bb.png)
### 開啟 Azure Monitor 紀錄
![image.png](/.attachments/image-cfdde95f-3000-4a33-83a3-171977a203f0.png)


## 測試
![image.png](/.attachments/image-1350af2f-8656-4fc9-926c-c36430ec6dc8.png)
![image.png](/.attachments/image-708925b5-9550-421d-bac4-d181cbb3294a.png)
![image.png](/.attachments/image-5a59c3c3-0eed-40f2-8418-d51ce2ba7340.png)
![image.png](/.attachments/image-bbb1f4d5-8feb-442d-b747-e8a416033adf.png)
![image.png](/.attachments/image-7a0a13a9-aa04-4116-98ce-10dacee9eb29.png)

# 錯誤訊息
## Status 429
429 Too Many Requests
![image.png](/.attachments/image-29d218ef-d6bd-4be2-8547-65e9569c0b64.png)


# Policy
## 驗證 header Email formatter

```
<set-variable name="isEmailValid" value="@{
	// The regular expression pattern to match.
	var pattern = @"^[\w+-]+@+walsin.com$";
	var input = context.Request.Headers.GetValueOrDefault("User-Email", "defaultNullNotAllowed");
	
	// Instantiate the regular expression object.
	Regex r = new Regex(pattern, RegexOptions.IgnoreCase);
	
	// Match the regular expression pattern against a text string.
	Match m = r.Match(input);
	if(m.Success)
	{
		return "true";
	}
	return "false";            
}" />
<set-header name="is-email-allowed" exists-action="append">
	<value>@( (string)context.Variables["isEmailValid"] )</value>
</set-header>
<check-header name="is-email-allowed" failed-check-httpcode="403" failed-check-error-message="Email格式不符合!" ignore-case="false">
	<value>true</value>
</check-header>
```



# Azure OpenAI
aoai-walsin-rpa-dev 
## 網路功能
使用VNET 及 特定服務來源
來源APIM 固定IP: 57.155.55.71
![image.png](/.attachments/image-cc997af6-1b33-4740-9fa3-8a019234065a.png)



# APIM Backends
如果服務有跨訂閱 或 出現 以下 錯誤訊息時，
需到 Backends 設定相關授權

```
authentication-managed-identity (0.008 ms)
{
    "message": "Managed service identity is not enabled for this service and authentication-managed-identity policy cannot be used."
}
authentication-managed-identity (0.592 ms)
{
    "messages": [
        null,
        "Managed service identity must be configured to use authentication-token policy."
    ]
}
```

![image.png](/.attachments/image-9830ccda-833f-44a9-b6dc-efb9ce38ac9a.png)
開啟 驗證憑證鏈結/驗證憑證名稱
![image.png](/.attachments/image-97184113-85de-4772-8b0d-961660bf3721.png)
![image.png](/.attachments/image-6097f86a-6d1b-401a-8536-1f2f8750ea45.png)


# 參考
- [APIM 原則參考](https://learn.microsoft.com/zh-tw/azure/api-management/azure-openai-emit-token-metric-policy)



# Azure OpenAI Service Load Balancing with Azure API Management
=============================================================

1.  A user makes a request to a deployed Azure API Management API that is configured using the Azure OpenAI API specification.
    *   The API Management API is configured with a policy that uses a static, round-robin load balancing technique to route requests to one of the Azure OpenAI Service instances.
2.  A bearer token is generated using the managed identity associated with the Azure API Management instance which has the **Cognitive Services OpenAI User** role assigned to the Azure OpenAI Service instances.
3.  The original request including headers and body are forwarded to the selected Azure OpenAI Service instance, along with the Authorization header containing the bearer token.

![A simplified flow of a request to Azure OpenAI instances via API Management using managed identity](https://learn.microsoft.com/en-us/samples/azure-samples/azure-openai-apim-load-balancing/azure-openai-service-load-balancing-with-azure-api-management/media/flow.png)



## Policy: azure-openai-apim-load-balancing

```
<policies>
    <inbound>
        <base />
        <cache-lookup-value key="backend-counter" variable-name="backend-counter" />
        <choose>
            <when condition="@(!context.Variables.ContainsKey("backend-counter"))">
                <set-variable name="backend-counter" value="0" />
                <cache-store-value key="backend-counter" value="0" duration="100" />
            </when>
        </choose>
        <choose>
            <when condition="@(Convert.ToInt32(context.Variables["backend-counter"]) == 0)">
                <set-backend-service backend-id="OpenAIUKS" />
                <set-variable name="backend-counter" value="1" />
                <cache-store-value key="backend-counter" value="1" duration="100" />
            </when>
            <otherwise>
                <set-backend-service backend-id="OpenAIWUS" />
                <set-variable name="backend-counter" value="0" />
                <cache-store-value key="backend-counter" value="0" duration="100" />
            </otherwise>
        </choose>
        <authentication-managed-identity resource="https://cognitiveservices.azure.com" client-id="{{MANAGED-IDENTITY-CLIENT-ID}}" output-token-variable-name="msi-access-token" ignore-error="false" />
        <set-header name="Authorization" exists-action="override">
            <value>@("Bearer " + (string)context.Variables["msi-access-token"])</value>
        </set-header>
        <rewrite-uri template="@{return context.Request.OriginalUrl.Path;}" />
    </inbound>
    <backend>
        <retry condition="@(context.Response.StatusCode >= 400)" count="3" interval="5" first-fast-retry="true">
            <cache-lookup-value key="backend-counter" variable-name="backend-counter" />
            <choose>
                <when condition="@(Convert.ToInt32(context.Variables["backend-counter"]) == 0)">
                    <set-backend-service backend-id="OpenAIUKS" />
                    <set-variable name="backend-counter" value="1" />
                    <cache-store-value key="backend-counter" value="1" duration="100" />
                </when>
                <otherwise>
                    <set-backend-service backend-id="OpenAIWUS" />
                    <set-variable name="backend-counter" value="0" />
                    <cache-store-value key="backend-counter" value="0" duration="100" />
                </otherwise>
            </choose>
            <forward-request buffer-request-body="true" />
        </retry>
    </backend>
    <outbound>
        <base />
    </outbound>
    <on-error>
        <base />
    </on-error>
</policies>
```


## 參考
[azure-openai-apim-load-balancing/infra/policies/round-robin-policy.xml at main · Azure-Samples/azure-openai-apim-load-balancing · GitHub](https://github.com/azure-samples/azure-openai-apim-load-balancing/blob/main/infra/policies/round-robin-policy.xml)


