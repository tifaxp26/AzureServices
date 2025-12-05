# Controller命名請根據(依順序優先)
1. 資料來源(資料的最源頭)
1. 專案名稱
1. 來源DB名稱(MES, BIDATA, SAP...) *不建議,因為DB中的表格會很多

## 收到開發API需求後請先確認是否已經建立Controller了沒有請先建立新Controller，Service, IService, Model, Example都請跟著Controller命名, 
`建議除了請User提供Product也請提供Controller名稱，把Controller當成是一個小分類`
既往不咎，因為動到Controller會影響網址所以舊的就先不動，新API請根據上面順序命名，其餘命名原則依據先前定義的開發規範。
## 通常專案名稱只會用在APITagAttr的部分