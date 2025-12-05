[[_TOC_]]

AKS 私人叢集 外部服務是無法讀到 API 服務，
建議使用 ARM 部署方式較為方便

       

# (建議)Service Connection type選 ARM 方式
選 aks 訂閱帳戶
選 ask 資源群組
選 ask 名稱
勾選 Use Cluster admin credentials
選 Namespace
設定 Mainfests
設定 Containers 可不填  因為寫在 Manifest\service\各模組.yaml
ImagePullSecrets 可不填

![image.png](/.attachments/image-ef99b682-7e80-43f4-b5cf-be8b37ca2cb4.png)
![image.png](/.attachments/image-8e9a06e8-b2fe-41b1-9bd5-d0eb6639df86.png)

# Service Connection type選 AKS Service Connection 方式
![image.png](/.attachments/image-fb4c69df-1520-4553-9024-fd117822971d.png)

# Pipelines / Release 部署設定
選 Hosted Agent
![image.png](/.attachments/image-03d19cdf-abb2-4211-9a41-e2c75da584d9.png)
![image.png](/.attachments/image-c4778331-c100-4ec2-985a-f9d122df34ec.png)


