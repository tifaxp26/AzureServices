[[_TOC_]]

# 使用 Terraform
---------

## Step 1 安裝工具
需要先安裝：
- Terraform
- Azure CLI
- VS Code（可選）

## Step 2 登入 Azure
`az login`

## Step 3 建立 Terraform 檔案
建立一個檔案：
main.tf

內容如下：

```
provider "azurerm" {
  features {}
}

resource "azurerm_resource_group" "rg" {
  name     = "rg-terraform-demo"
  location = "East Asia"
}
```


## Step 4 初始化 Terraform
`terraform init`
會下載 Azure Provider。
![image.png](/.attachments/image-43167a32-7956-4737-a41c-bbd0eea52af4.png)


## Step 5 預覽部署
`terraform plan`

你會看到類似：
+ create azurerm_resource_group.rg
![image.png](/.attachments/image-7b690dbb-e8cc-455f-b0cf-af827bc64272.png)

## Step 6 建立資源
`terraform apply`
輸入： yes

完成後就會建立：
Resource Group
rg-terraform-demo
![image.png](/.attachments/image-1798e4e9-23da-478e-95dc-d5a111b455b5.png)


可以到 Microsoft Azure Portal 查看。
![image.png](/.attachments/image-82a15521-e72e-4fd2-96fa-c83019ed378b.png)


---

## 繼續實現 VNET, SUBET

2️⃣ Virtual Network
resource "azurerm_virtual_network" "vnet" {
  name                = "vnet-demo"
  location            = azurerm_resource_group.rg.location
  resource_group_name = azurerm_resource_group.rg.name
  address_space       = ["10.0.0.0/16"]
}
3️⃣ Subnet
resource "azurerm_subnet" "subnet1" {
  name                 = "subnet-demo"
  resource_group_name  = azurerm_resource_group.rg.name
  virtual_network_name = azurerm_virtual_network.vnet.name
  address_prefixes     = ["10.0.1.0/24"]
}

erraform 通常會建立：


```
Terraform  
   │  
   ├─ Resource Group  
   ├─ VNet  
   ├─ Subnet  
   ├─ AKS Cluster  
   ├─ KeyVault  
   ├─ Managed Identity  
   └─ Azure SQL MI
```


---

# Terraform State 

## 說明
======================
不要放在本機

當你使用 **Terraform** 時，每次 `apply` 都會產生：
terraform.tfstate

這個檔案其實是：
Terraform 的資源資料庫

裡面記錄：
* 已建立的資源 ID    
* Azure 資源名稱
* 設定內容    
* 依賴關係
    
## 企業標準做法（Remote State）
======================

Terraform 官方推薦：
把 state 放在遠端。
在 Azure 就是：
**Azure Storage**
架構變成：


```
Terraform CLI  
      │  
      │  
Azure Storage (state)  
      │  
      │  
Azure Infrastructure
```


State 放在：

Storage Account  
   └── Blob Container  
         └── terraform.tfstate

## 最大好處（企業為什麼一定這樣做）
================

使用 **Azure Storage Remote State** 會得到：

| 功能 | 說明 |
| --- | --- |
| State Lock | 避免同時 apply |
| 團隊共用 | 所有人用同一份 state |
| 安全 | RBAC 控制 |
| CI/CD | Pipeline 可讀 |
| Backup | Azure 自動備份 |


## Terraform Remote State 設定（實務教學）
===============================

Step1 建立 Storage
----------------
先建立：
Storage Account  
Container

例如：
Storage Account  
tfstatestorage123  
  
Container  
tfstate

* * *

Step2 Terraform backend 設定
--------------------------

在 `main.tf` 加入：


```
terraform {  
  backend "azurerm" {  
    resource_group_name  = "rg-terraform"  
    storage_account_name = "tfstatestorage123"  
    container_name       = "tfstate"  
    key                  = "prod.terraform.tfstate"  
  }  
}
```


這代表：
State file  
↓  
Azure Blob Storage

* * *

Step3 初始化 backend
-----------------
重新初始化：
terraform init

Terraform 會問： Do you want to migrate state?
輸入：yes

Terraform 會：
本機 tfstate  
↓  
搬到 Azure Storage


## State Lock（最酷的地方）
=================

當有人執行：
terraform apply

Azure Blob 會鎖定：
terraform.tfstate

其他人執行：
terraform apply

會看到：
Error acquiring the state lock
這可以 **避免 infrastructure 搞壞**。


# 企業 Terraform 架構（推薦）
===================

很多公司會這樣設計：


```
Terraform Project  
│  
├── main.tf  
├── variables.tf  
├── outputs.tf  
├── backend.tf  
│  
└── modules  
     ├── vnet  
     ├── aks  
     ├── sqlmi  
     └── keyvault
```


Remote State：

```
Azure Storage  
   │  
   ├── dev.tfstate  
   ├── test.tfstate  
   └── prod.tfstate
```


企業 Terraform Project 目錄結構（常見版本）

```
terraform-azure-project/  
│  
├── backend.tf  
├── provider.tf  
├── main.tf  
├── variables.tf  
├── outputs.tf  
├── terraform.tfvars  
│  
├── modules/  
│ │  
│ ├── vnet/  
│ │ ├── main.tf  
│ │ ├── variables.tf  
│ │ └── outputs.tf  
│ │  
│ ├── aks/  
│ │ ├── main.tf  
│ │ ├── variables.tf  
│ │ └── outputs.tf  
│ │  
│ ├── sqlmi/  
│ │ ├── main.tf  
│ │ ├── variables.tf  
│ │ └── outputs.tf  
│ │  
│ └── keyvault/  
│ ├── main.tf  
│ ├── variables.tf  
│ └── outputs.tf  
│  
└── environments/  
│  
├── dev.tfvars  
├── test.tfvars  
└── prod.tfvars
```

# Example Code
[IaCProject.zip](/.attachments/IaCProject-63ad5d79-8a14-4882-8993-4e543ae0d088.zip)




