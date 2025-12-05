# L5DIP新Drapper元件修改步驟
* 下圖為須修改的範圍
![image.png](/.attachments/image-55f216ff-40c7-4b30-87ab-417a9665255f.png)

* 黃色部分 : 直接刪除
* 綠色部分 : 插入下列語法
```
            var dbConnection = await _dbConnManager.GetDbConnStringAsync(CommonCode.WalsinL5dipDbConnection);
            var darpperHelper = new Core.DataConnector.DapperHelper(dbConnection);

            sqlWhere = ReadDefaultFilter(apiReq, sqlWhere, parameters);
            sql = string.Concat(sql, sqlWhere, sqlOrder);

```
請自行修改`CommonCode.WalsinL5dipDbConnection`成Service對應DB的Connection String
* 紫紅色部分 : 添加新的參數
將`darpperHelper`加入至參數內(第一個參數)
`(darpperHelper, sql, parameters)` and `(darpperHelper, sql, parameters, apiReq.PageInfo)`
