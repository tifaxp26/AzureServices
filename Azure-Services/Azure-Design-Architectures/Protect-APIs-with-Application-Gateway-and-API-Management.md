
With more workloads adhering to the API-first approach for their design, and the growing number and severity of threats to web applications over the internet, it's critical to have a security strategy to protect APIs. One step toward API security is protecting the network traffic by using the Gateway Routing pattern. You use the gateway to restrict traffic source locations and traffic quality in addition to supporting flexible routing rules. This article describes how to use Azure Application Gateway and Azure API Management to protect API access.

# Architecture
This article doesn't address the application's underlying services, like App Service Environment, Azure SQL Managed Instance, and Azure Kubernetes Services. Those parts of the diagram only showcase what you can do as a broader solution. This article specifically discusses the shaded areas, API Management and Application Gateway.

Diagram showing how Application Gateway and API Management protect APIs.


<IMG  src="https://learn.microsoft.com/en-us/azure/architecture/web-apps/api-management/_images/protect-apis.png"  alt="Diagram showing how Application Gateway and API Management protect APIs."/>