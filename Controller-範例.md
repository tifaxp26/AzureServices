# 範例
---
以 **AccountingPerformanceController** 為例
> AccountingPerformance -> 會計績效平台 
![image.png](/.attachments/image-135ef619-9f7c-4fb4-9652-d5c71eb4521f.png)
(圖供參考,詳細情況請見下說明)
---
## 下列四項為**必要**
1. Controller的 **Summary** .
2. Controller下所有**Method**的共同屬性.
3. **Method** 的摘要.
>> summary : 通常為title
>> remarks : 你的敘述描述
>> para : 換行
4. **Method**上需要附加的屬性 **(註4)**.
---
# (註4)目前需要附加的自定義屬性有: (APIMetaDataAttr)與(APIInOutputMetaDataAttr)
--> 更正 (APITagAttr)(APIMetaDataAttr)與(APIInOutputMetaDataAttr)
![image.png](/.attachments/image-755cda7e-986f-42e0-8c6b-8d0296f95869.png)
<br>


## (在dataCode出來前暫停使用,暫時註解掉)**APIMetaDataAttr** - 為**API**中繼資料,其下包含 
>(string **dataCode**, string **author**, string **publishDate**, string? **owner** = null)
1. **dataCode**: 此Method的資料碼(唯一值)
2. **publishDate**: 開發完成的日期
3. **author**: 開發者**姓名**
4. **owner**: 需求/窗口提出者(user找的對象)**姓名**,若無則留空,**預設**為author
---
## **APIInOutputMetaDataAttr** - (範例請參閱Fincontroller)其為**輸入輸出**中繼資料,
下包含 
> (string **apiOperation**, Type **inputType**, Type **outputType**)
1. **apiOperation**: 此Method的名稱(以**nameof**方式放入)
2. **inputType**: 輸入時的**model**, 
>>注意: inputType為 payload 結構下的model結果 (例如有tablename的model -> **L5DIPReq**)
3. **outputType**: 輸出時的**model**
>>注意: outputType為 payload 結構下的data的結果 -> `"payload": {
 "count": 1,
 "total": null,
 "data": outputType
`


