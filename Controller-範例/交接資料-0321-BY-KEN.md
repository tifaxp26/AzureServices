![image.png](/.attachments/image-56471c03-5e1a-4938-b37a-d818d5fc3c2b.png)

簡述: 

1. **APIMetaDataOperationFilters** 下有註解

2. **ExceptionActionFilter**   <- controller 錯誤log處理，這裡文政說需要重製，目前沒有規劃

3. **LogActionFilter** <- log 執行一般log處理，這裡文政說需要重製，目前沒有規劃

   

## **APIMetaDataOperationFilters**

![image.png](/.attachments/image-89237378-20e8-44aa-8191-3abf6fc4d339.png)

1. **APIMetaDataAttr**  (Datacode、開發相關值)

   > 目前用法如下
   > Example: [APIMetaDataAttr(dataCode: "SS-YT-SC-EQ-R-003", publishDate: "2024-01-10", author: "林泓均", owner: "吳萱萱")]

   - **dataCode** 資料唯一碼。

   - **publishDate** 完成日期。(目前這個值有傳出去，但並未有對接)

   - **author** 作者\開發者

   - **owner** 需求端\sa端

     > 未用到的: FunctionID (Action ID) 本來要給瑞益那邊定的，現在未用到，可為空值。
     >
     > 額外規則: 若無owner則owner值訂為author 。

   

2. **APIInOutputMetaDataAttr** (api action或是參數欄位相關)

   > 目前用法如下
   > Example: [APIInOutputMetaDataAttr(apiOperation: nameof(actionName),
   > inputType: typeof(reqModel),
   > outputType: typeof(resModel))]

    - **ApiOperationActionName** 該action名稱 
    - **inputType**: 輸入時的**model**,

   > > 注意: inputType為 payload 結構下的model結果 (例如有tablename的model -> **L5DIPReq**)

   - **outputType**: 輸出時的**model**

   > > 注意: outputType為 payload 結構下的data的結果 -> `"payload": { "count": 1, "total": null, "data": outputType`


3. **APITagAttr** (Swagger.json 拆分名稱的組成有關係)

   > 目前用法如下
   >
   > Example: [APITagAttr(name: "AccountingPerformance", description: "會計績效平台", title: "Walsin.DataPlatform.Accounting-Performance")]

   - Name 名稱

   - Description 描述

   - Title 標題

     > ITagsMetadata 這個介面的使用會讓該值有tag的值

![image.png](/.attachments/image-9efe35e7-04c5-4192-abfb-35ac69581bbb.png)


## DbConnManager
![image.png](/.attachments/image-e944d66f-f495-417a-9648-419463801a06.png)

## ConfigSwaggerExtension
![image.png](/.attachments/image-58cb22a4-f864-43aa-8dba-3b012a0075e4.png)

AddSwaggerGenOption 為資料拆分的控制項，請記住tags類的都是在此處去加的。
  > options.OperationFilter<APIMetaDataOperationFilters>();
  > options.DocumentFilter<AddTagsDocumentFilter>();