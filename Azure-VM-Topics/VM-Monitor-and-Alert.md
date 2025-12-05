[[_TOC_]]

說明:
啟用監視服務 Azure Monitor，
會將資料記錄到 Log Analytics Workspace，
可以客製化KQL進行資源監控。


# 監視 > 計量
計量命名空間: 虛擬機器主機
度量: Percentage CPU、Percentage Mem
![image.png](/.attachments/image-f90fa409-6ead-430e-b5fd-9956d22baedd.png)

## 儲存至活頁簿
![image.png](/.attachments/image-68653acd-7ead-40be-8ab5-2df3ad7bff2f.png)

## 建立警示規則
![image.png](/.attachments/image-e071307b-5cb7-491a-9625-2dbb3cd137e6.png)


# 監控 磁碟空間
利用 Azure Monitor > 資料收集規則
![image.png](/.attachments/image-7a0f32ec-50ff-43ba-8e43-6166140fa889.png)

建立
![image.png](/.attachments/image-06cbbdc4-88dc-40f7-9d9c-9a6d9dcf25a3.png)

選擇 資源
![image.png](/.attachments/image-e4ed1d85-96f9-4d60-8a0c-da3db34761c1.png)

選擇 資料來源
![image.png](/.attachments/image-90be950a-18de-413e-b8e5-733f0a40505c.png)

選擇 Log Analytics Workspace
![image.png](/.attachments/image-9b487860-f532-43ce-acb1-88f6ec101ceb.png)

# 建立警示
到 Log Analytics Workspace
![image.png](/.attachments/image-fe7b81f2-58ee-455d-b072-1439ecad05fa.png)

KQL 過濾監控指標
```
// DISK 
InsightsMetrics
| where Namespace == "LogicalDisk"
| where Name has_any ("FreeSpaceMB", "FreeSpacePercentage")
// where Name == "FreeSpaceMB" and (Val/1024 < 50)
| where Name == "FreeSpacePercentage" and (Val < 70)
```
建立警示規則
![image.png](/.attachments/image-b5c7890c-bc21-4d92-be61-2bef0b983aa0.png)


