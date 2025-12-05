--使用者 今日使用狀況
SELECT [ActionName],[OcpApimSubscriptionKey], COUNT(*) AS total_usage_count
FROM [AZDPS_APDATA_LOG]
WHERE LoggedDate = CAST(GETDATE() AS DATE)
GROUP BY [OcpApimSubscriptionKey],[ActionName]
ORDER BY [OcpApimSubscriptionKey]

--使用者 今日使用狀況 v_view
SELECT [ActionName]
      ,[OcpApimSubscriptionKey]
      ,[total_usage_count]
  FROM [dbo].[V_APDATA_LOG_USER_USAGE_GETDATE]

--使用者 使用狀況總和
SELECT [OcpApimSubscriptionKey], SUM(total_usage_count) AS totalusagecount
FROM [dbo].[V_APDATA_LOG_USER_USAGE_GETDATE]
GROUP BY [OcpApimSubscriptionKey]
ORDER BY totalusagecount DESC

--使用者 每天 的 API 使用情況
SELECT [LoggedDate],[OcpApimSubscriptionKey], COUNT(*) AS total_usage_count
FROM [AZDPS_APDATA_LOG]
GROUP BY [OcpApimSubscriptionKey],[LoggedDate]
ORDER BY total_usage_count desc

--一整周使用者的 API 使用情況
SELECT 
    [OcpApimSubscriptionKey],
    DATEPART(WEEK, [LoggedDate]) AS WeekNumber,
    COUNT(*) AS TotalUsageCount
FROM [AZDPS_APDATA_LOG]
GROUP BY 
    [OcpApimSubscriptionKey],
    DATEPART(WEEK, [LoggedDate])
ORDER BY 
    WeekNumber DESC, TotalUsageCount DESC, [OcpApimSubscriptionKey];
	
--前六小時錯誤排查
SELECT [Id]
      ,[DP_CREATE_DATE]
      ,[ActionName]
      ,[ControllerName]
      ,[Message]
      ,[OcpApimSubscriptionKey]
  FROM [dbo].[V_APDATA_ERROR_LOG_SIX_HOURS_AGO]
  
--(原始)前六小時錯誤排查
SELECT [Id] , [CreatedOn], [Message]
FROM [AZDPS_APDATA_LOG]
WHERE CreatedOn >=  DATEADD(HOUR, -6, DATEADD(HOUR, 8, GETDATE()))
  AND [Level] = 'Error'
  AND [Message] != 'ErrorCode: E903 | ErrorMessage: 查無資料' 
ORDER BY LoggedDate;

--前六小時錯誤排查 id一欄
SELECT STRING_AGG(CONVERT(NVARCHAR(MAX), [Id]), ', ') AS Ids
FROM [dbo].[V_APDATA_ERROR_LOG_SIX_HOURS_AGO];
