# RFC_SAP 使用

[[_TOC_]]

API 連結：`http://10.190.253.201/api/SAPRFC`

test API 連結：`http://10.190.253.201/api/testSAPRFC`

[站台檔案](https://dev.azure.com/WalsinTeam/DataPlatform/_git/Walsin.DataPlatform.SAP)

### 目前有的數據

| 次序 | Function Name | Table Name(輸出) | 需求窗口 | 內容簡介
| --- | ------------- | ----------------- | -------- | --------------------------------------------- | 
| 1 | [`Z_AR_CUST_OPEN_AR_01`](#z_ar_cust_open_ar_01) | ZSDT070 | 陳新弘 | 得到客戶的未結應收明細項目 | 
| 2 | [`Z_FI_RREADGLPCTDATA_02`](#z_fi_rreadglpctdata_02) | TRS003 | 邱瑞益 | 新莊BI專案-損益表讀取 |
| 3 | [`Z_PM_RREPAIRS_EXPENSE_01`](#z_pm_rrepairs_expense_01) | OUT_ZPMR002 | 邱瑞益 | 
| 4 | [`Z_MM_ZIM00014`](#z_mm_zim00014) | PT_OUT | 邱瑞益 | 
| 5 | [`Z_MM_MVT_DATA_01`](#z_mm_mvt_data_01) | OUT_ZMMR115 | 邱瑞益/伏育磊 | 
| 6 | [`Z_MM_RUPLOAD_PAYMENT_CON_01`](#z_mm_rupload_payment_con_01) | T_OUT | 陳新弘 | 
| 7 | [`Z_FI_RREADGLPCADATA_18`](#z_fi_rreadglpcadata_18) | OUT_ZMMR116 | 陳新弘 | 
| 8 | [`Z_SD_PAYMENT_TERM`](#z_sd_payment_term) | ITAB | 陳新弘 | 
| 9 | [`Z_CREDIT_EXPOSURE_1`](#z_credit_exposure_1) | ZKNK | 陳新弘 | 
| 10 | [`Z_SD_SALESPERSON_01`](#z_sd_salesperson_01) | OUT_DATA | 陳新弘 | 業務員清單 |
| 11 | [`Z_FI_RREADGLPCADATA_20`](#z_fi_rreadglpcadata_20) | TRS001 | 陳新弘 | 
| 12 | [`RFC_READ_TABLE`](#rfc_read_table) | DATA | 陳文政/陳新弘 | 
| 13 | [`Z_AR_ACC_CLOSEITEM_01`](#z_ar_acc_closeitem_01) | OUT_ZARS005 | 陳新弘 | 
| 14 | [`Z_FI_RREADGLPCTDATA_16`](#z_fi_rreadglpctdata_16) | TRS001 | 陳新弘 | 
| 15 | [`Z_FI_RREADGLPCADATA_13`](#z_fi_rreadglpcadata_13) | PT_OUT | 伏育磊 | 24/10/29 調整文件
| 16 | [`Z_SD_RZRSD0001_01`](#z_sd_rzrsd0001_01) | PT_OUT | 陳新弘 |24/10/22 調整寫法
| 17 | [`Z_MM_RZMMJ_01`](#z_mm_rzmmj_01) | PT_OUT | 陳新弘 |
| 18 | [`Z_PO_ZPO00011_01`](#z_po_zpo00011_01) | OUT_ZPOR011 | 陳新弘 |
| 19 | [`Z_PO_ZPO00012D_1`](#z_po_zpo00012d_1) | Data | 陳新弘 |
| 20 | [`Z_PP_RPRODORD_03`](#z_pp_rprodord_03) | IT_SUM | 伏育磊 |
| 21 | [`Z_FI_RREADGLPCADATA_22`](#z_fi_rreadglpcadata_22) | OUT_ZFIS026 | 伏育磊 |
| 22 | [`Z_PP_RREADAFPODATA_01`](#z_pp_rreadafpodata_01) | OUT_ZPPR200 | 伏育磊 |
| 23 | [`Z_SD_ZSDR0031_01`](#z_sd_zsdr0031_01) | DATA | 伏育磊 |
| 24 | [`Z_SD_ZSDR0054_1_01`](#z_sd_zsdr0054_1_01) | DATA | 伏育磊 |
| 25 | [`Z_FI_RGETSALESDATA_01`](#z_fi_rgetsalesdata_01) | OUT_ZSDR027 | 伏育磊 |
| 26 | [`Z_MM_ZIM5_01`](#z_mm_zim5_01) | OUT_ZMMR055 | 伏育磊 |
| 27 | [`Z_MM_ZIM7_01`](#z_mm_zim7_01) | DATA | 伏育磊 |
| 28 | [`Z_FI_RREADGLPCTDATA_15`](#z_fi_rreadglpctdata_15) | TRS001 | 陳新弘 |
| 29 | [`Z_PM_RREPAIRS_EXPENSE_TCSS`](#z_pm_rrepairs_expense_tcss) | OUT_ZPMR002 | 邱瑞益 |
| 30 | [`Z_SD_ZSD00032`](#z_sd_zsd00032) | ZSD00032 | 陳新弘 |
| 31 | [`Z_MM_ZIMR0039_2`](#z_mm_zimr0039_2) | DATA | 邱瑞益 |
| 32 | [`Z_FI_APDATA_01`](#z_fi_apdata_01) | I_DATA | 陳新弘 |
| 33 | [`Z_FI_RREADGLPCADATA_21`](#z_fi_rreadglpcadata_21) | OUT_ZMMR200 | 陳新弘 |
| 34 | [`Z_ZIMB_YSS1`](#z_zimb_yss1) | OUT_TXT | 吳振華 |
| 35 | [`Z_SD_RB6REPORT_YS`](#z_sd_rb6report_ys) | OUT_ZSDR022 | 陳新弘 |
| 36 | [`ZMMF1009`](#zmmf1009) | T_POSL | 鄭意儒 |
| 37 | [`Z_SD_RGETSDORDERS_03`](#Z_SD_RGETSDORDERS_03) | TRS001| 陳新弘 |
| 38 | [`Z_MM_ZIMR0043`](#Z_MM_ZIMR0043) | IT_DATA | 鄭意儒 |
| 39 | [`Z_IM_GOODSMOVEMENTS_01`](#Z_IM_GOODSMOVEMENTS_01) | I_DATA | 鄭意儒 |
| 40 | [`Z_FI_RREADNIDATA_01`](#Z_FI_RREADNIDATA_01) | TRS001 | 鄭意儒 |
| 41(BUG) | [`Z_COR_IO_BUDGET_VALUE`](#Z_COR_IO_BUDGET_VALUE) | TBPGE | 陳新弘 |
| 42(BUG)| [`ZRFC_ZIMR0041`](#ZRFC_ZIMR0041) | OUTDATA | 鄭意儒 |
| 43 | [`Z_IM_ZIM00015`](#Z_IM_ZIM00015) | IT_DATA | 鄭意儒 |
| 44(BUG) | [`Z_MM_ZIM00015`](#Z_MM_ZIM00015) |   | 鄭意儒 |
| 45(BUG) | [`Z_MM_ZIMR0007`](#Z_MM_ZIMR0007) |ITAB| 鄭意儒 |
| 46 | [`ZMM_ZPO00025B_FM`](#ZMM_ZPO00025B_FM) | T_ANLA | 陳新弘 |
| 47 | [`BBP_RFC_READ_TABLE`](#BBP_RFC_READ_TABLE) | DATA | 陳新弘 | 124/12/18調整
| 48 | [`Z_IM_MATERIALCHANGE_01`](#Z_IM_MATERIALCHANGE_01) | DATA | 陳新弘 |
| 49 | [`Z_FI_ACC_DOCUMENT_POST_05`](#Z_FI_ACC_DOCUMENT_POST_05) | DATA | 傅昱翔 |
| 50 | [`Z_FI_RREADSET_02`](#Z_FI_RREADSET_02) | STR | 陳新弘 | 24/10/04新增
| 51 | [`Z_FI_ACC_GETOPENITEMS`](#Z_FI_ACC_GETOPENITEMS) | IT_BSIS | 陳新弘 | 24/10/07新增
| 52 | [`Z_IM_MATERIAL_WEIGHT_01`](#Z_IM_MATERIAL_WEIGHT_01) | IT_DATA | 陳新弘 | 25/03/18新增
| 53 | [`ZFM_AA_FIXED_ASSET_READ`](#ZFM_AA_FIXED_ASSET_READ) | OT_ASSET| 陳新弘 | 25/04/10新增
| 54 | [`Z_MM_INVENTORY_DATA_01`](#Z_MM_INVENTORY_DATA_01) | IT_DATA | 王彥樺 | 25/11/28新增

## 用法: 替代PAYLOAD裡面資料
```JSON
{
  "Header": {
    "dept": "string",
    "UserNo": "string"
  },
  "PayLoad": {

  }
}
```


## 用法: 目前擁有的模型方法

#### Z_AR_CUST_OPEN_AR_01

```JSON
"funcName" : "Z_AR_CUST_OPEN_AR_01","tableName" : "ZSDT070"
```

#### Z_FI_RREADGLPCTDATA_02
```JSON
"funcName" : "Z_FI_RREADGLPCTDATA_02", "tableName" : "TRS003","tableParameter":{"PA_GJAHR":"2022","PA_ACCONT":"ZIS0150000"}  
```

#### Z_PM_RREPAIRS_EXPENSE_01
``` json
"funcName" : "Z_PM_RREPAIRS_EXPENSE_01", "tableName" : "OUT_ZPMR002","tableParameter":{"ILOW_I":20181004,"IHIGH_I":20181004}  
```
#### Z_MM_ZIM00014
``` json
"funcName" : "Z_MM_ZIM00014", "tableName" : "PT_OUT","tableParameter":{"PI_YEAR":"2022","PI_MONTH":"4","PR_MATNR":"1*"}
```

#### Z_MM_MVT_DATA_01
``` json
"funcName" : "Z_MM_MVT_DATA_01", "tableName" : "OUT_ZMMR115"
,"tableParameter":{
"ILOW_I":20220601,"IHIGH_I":20220630,"WERKS":"YSS1"
,"LGORT": "","BWART_ILOW_I":"933","BWART_IHIGH_I":"934","MATNR":""}
```

#### Z_MM_RUPLOAD_PAYMENT_CON_01
``` json
"funcName" : "Z_MM_RUPLOAD_PAYMENT_CON_01","tableName" : "T_OUT" 
```

#### Z_FI_RREADGLPCADATA_18
``` json
"funcName" : "Z_FI_RREADGLPCADATA_18","tableName" : "OUT_ZMMR116","tableParameter":{"PA_GJAHR":"2022","PA_POPER":"009"}
```

#### Z_SD_PAYMENT_TERM
``` json
"funcName" : "Z_SD_PAYMENT_TERM","tableName" : "ITAB"
```

#### Z_CREDIT_EXPOSURE_1
``` json
"funcName" : "Z_CREDIT_EXPOSURE_1","tableName" : "ZKNKK"
```

#### Z_SD_SALESPERSON_01
``` json
"funcName" : "Z_SD_SALESPERSON_01","tableName" : "OUT_DATA"
```

#### Z_FI_RREADGLPCADATA_20
``` json
"funcName" : "Z_FI_RREADGLPCADATA_20","tableName" :"TRS001",
"tableParameter":{
"PA_ACCONT":"NETNR",
"PA_ACCONT1":"NETARS",
"PA_ACCONT2":"AP",
"RKOKRS":"WL01",
"RBUKRS":"WL01",
"RRYEAR":"2022",
"RMONAT":"009"
}
```

#### RFC_READ_TABLE
``` json
"funcName" : "RFC_READ_TABLE",
"tableName" : "DATA",
"tableParameter":{
    "QUERY_TABLE":"CE1WL01","OPTIONS":"PERIO = 2022001" ,
    "FieldModel" : [
        {
            "FieldName": "PERIO"
        },
        {
            "FieldName": "PRCTR"
        },
        {
            "FieldName": "VVEW1"
        }
    ]
}
```

#### Z_AR_ACC_CLOSEITEM_01
``` json
"funcName": "Z_MM_ZIMR0007",
"tableName": "ITAB",
"tableParameter": {
	"WERKS": "20221225",
	"JOBNAME": "",
	"FIRST": "",
	"D_RQIDENT": ""
}
```

#### Z_FI_RREADGLPCTDATA_16
``` json
"funcName" : "Z_FI_RREADGLPCTDATA_16","tableName" :"TRS001","tableParameter":{
"PA_ACCONT":"NETNR",
"PA_GJAHR":"2022",
"PA_MONAT":"009",
"PA_PROFIT":"PC_C01_CA",
"PA_RVERSP":"WL7"
}
```

#### Z_FI_RREADGLPCADATA_13
``` json
"funcName" : "Z_FI_RREADGLPCADATA_13","tableName" :"PT_OUT1",
"tableParameter":{
            "PI_BUKRS": "WSWI",
            "PI_RYEAR": "2024",
            "PI_POPER1": "009",
            "PI_POPER2": "009",
            "PI_RVERS": "000",
            "PI_KOKRS": "WSCH",
            "PI_KSGRU": "EXP-WSCH",
            "PI_KAGRU": "",
            "PR_KSTAR_OP": "",
            "PR_KSTAR_LOW": "",
            "PR_KSTAR_HIGH": "",
            "PR_KOSTL_OP": "",
            "PR_KOSTL_LOW": "",
            "PR_KOSTL_HIGH": ""
}
```

#### Z_SD_RZRSD0001_01
``` json
"funcName": "Z_SD_RZRSD0001_01",
"tableName": "PT_OUT",
"tableParameter": {
	"PR_WERKS": [
		{
			"SIGN": "I",
			"OPTION": "EQ",
			"LOW": "SCP1",
			"HIGH": ""
		}
	],
	"PR_MATNR": [
		{
			"SIGN": "I",
			"OPTION": "CP",
			"LOW": "5*",
			"HIGH": ""
		}
	]
}
```

#### Z_MM_RZMMJ_01
``` json
"funcName" : "Z_MM_RZMMJ_01","tableName" : "PT_OUT","tableParameter":[{"SIGN":"I","OPTION":"EQ","LOW":"SCDM","HIGH":""},{"SIGN":"I","OPTION":"EQ","LOW":"SCT2","HIGH":""}]
```

#### Z_PO_ZPO00011_01
``` json
"funcName" : "Z_PO_ZPO00011_01","tableName" : "OUT_ZPOR011",
"tableParameter":{"ILOW_I": 20220101,"IHIGH_I": 20220130}
```

#### Z_PO_ZPO00012D_1
``` json
"funcName" : "Z_PO_ZPO00012D_1","tableName" : "Data",
"tableParameter":{"RAD_1":"X","CHECK1":"X","RBUKRS": "WL01","RWERKS": "YSS1","RAEDAT_ILOW_I":"20220101","RAEDAT_IHIGH_I":"20220531"}
```

#### Z_PP_RPRODORD_03
``` json
"funcName" : "Z_PP_RPRODORD_03","tableName" :"IT_SUM",
"tableParameter":{
"P_CONVDAT ": "06/22/2010","P_RATETYP": "M","P_CURR":"RMB",
"RBUDAT_LOW":"20100201","RBUDAT_HIGH":"20100228",
"RBWART_LOW":"101","RBWART_HIGH":"102",
"RMATNR1":"599*","RWERKS":"SCP1"}
```

#### Z_FI_RREADGLPCADATA_22
``` json
"funcName" : "Z_FI_RREADGLPCADATA_22","tableName" :"OUT_ZFIS026",
"tableParameter":{
"PA_GJAHR":"2022",
"PA_PERFR":"001",
"PA_PERTO":"002",
}
```

#### Z_PP_RREADAFPODATA_01
``` json
"funcName" : "Z_PP_RREADAFPODATA_01","tableName" : "OUT_ZPPR200",
"tableParameter":{
"SWERKS":"SCG1",
"SDGLTP_ILOW_I":"20220101",
"SDGLTP_IHIGH_I":"20220115",
}
```

#### Z_SD_ZSDR0031_01
``` json
"funcName" : "Z_SD_ZSDR0031_01","tableName" : "DATA",
"tableParameter":{
"RAUDAT_LOW":"20220301",
"RAUDAT_HIGH":"20220331"
}
```

#### Z_SD_ZSDR0054_1_01
``` json
"funcName" : "Z_SD_ZSDR0054_1_01","tableName" : "DATA",
"tableParameter":{
"RWERKS":"SCP1",
"RDIV":"電力",
"RDATE1_ILOW_I":"20220901",
"RDATE1_IHIGH_I":"20220930",
"RDATE2_ILOW_I":"20220301",
"RDATE2_IHIGH_I":"20220930",
"RPODATE_ILOW_I":"20220301",
"RPODATE_IHIGH_I":"20220930",
}
```

#### Z_FI_RGETSALESDATA_01
``` json
"funcName" : "Z_FI_RGETSALESDATA_01","tableName" : "OUT_ZSDR027",
"tableParameter":{
"PA_DATE_F":"20220101",
"PA_DATE_T":"20220131",
"PA_SALES":"TOTALSALE1"
}
```

#### Z_MM_ZIM5_01
``` json
"funcName" : "Z_MM_ZIM5_01","tableName" : "OUT_ZMMR055",
"tableParameter":{
"list_PA_WERKS":["SCP1","SCG1"],
"list_PA_MATNR": ["A0601081300000000S", "A0601100600000000S", "A0601081600000000S", "A0601082400000000S", "A0601082600000000S", "A0601082700000000S", "A0601082900000000S", "A0601080300000000S", "A0601080400000000S", "A0601082200000000S", "A0601090100000000S", "A0601090300000000S", "A0601081000000000S", "A0605010400000000S", "A0605012000000000S", "A0605010700000000S", "A0605013600000000S", "A0605020200000000S", "A0605020300000000S", "A0605030700000000S", "A0605031800000000S", "A0605011000000000S", "A0605020600000000S", "A0605012100000000S", "A0605020400000000S", "A0605020900000000S"]
}
```

#### Z_MM_ZIM7_01
``` json
"funcName" : "Z_MM_ZIM7_01","tableName" : "DATA",
"tableParameter":{
"list_RWERKS":["SCP1","SCG1"],
"list_RMATNR": ["A0601081300000000S", "A0601100600000000S", "A0601081600000000S", "A0601082400000000S", "A0601082600000000S", "A0601082700000000S", "A0601082900000000S", "A0601080300000000S", "A0601080400000000S", "A0601082200000000S", "A0601090100000000S", "A0601090300000000S", "A0601081000000000S", "A0605010400000000S", "A0605012000000000S", "A0605010700000000S", "A0605013600000000S", "A0605020200000000S", "A0605020300000000S", "A0605030700000000S", "A0605031800000000S", "A0605011000000000S", "A0605020600000000S", "A0605012100000000S", "A0605020400000000S", "A0605020900000000S"]
}
```

#### Z_FI_RREADGLPCTDATA_15
``` json
"funcName" : "Z_FI_RREADGLPCTDATA_15","tableName" : "TRS001",
"tableParameter":{
"PA_GJAHR":"2023",
"PA_MONATB":"006",
"PA_MONATE":"006",
"PA_PROFIT":"PC8_ALL_F",
"PA_RVERSP":"WL7",
"PA_ACCONT":"S41000F",
"PA_RVERSA":"0"
}
```

#### Z_PM_RREPAIRS_EXPENSE_TCSS
``` json
"funcName" : "Z_PM_RREPAIRS_EXPENSE_TCSS","tableName" : "OUT_ZPMR002",
"tableParameter":{
"ILOW_I":"20220601",
"IHIGH_I":"20220630",
}
```

#### Z_SD_ZSD00032
``` json
"funcName" : "Z_SD_ZSD00032","tableName" : "ZSD00032",
"tableParameter":{
"P_DATE":"20220601",
"P_VKORG_LOW":"0001",
"P_VKORG_HIGH":"0002",
"P_ERDAT_LOW":"20220101",
"P_ERDAT_HIGH":"20220131",
"P_SPART_LOW":"06",
"P_SPART_HIGH":"08"
}
```

#### Z_MM_ZIMR0039_2
``` json
"funcName" : "Z_MM_ZIMR0039_2","tableName" : "DATA",
"tableParameter":{
"GJAHR":"2022",
"MONAT":"08",
"MODE":"2"
}
```

#### Z_FI_APDATA_01
``` json
"funcName" : "Z_FI_APDATA_01","tableName" : "I_DATA",
"tableParameter":{
"P_BURKS":"WL01",
"P_GJAHR":"2023",
"P_HKONT":"0011210040",
"P_MONAT":"06"
}
```

#### Z_FI_RREADGLPCADATA_21
``` json
"funcName" : "Z_FI_RREADGLPCADATA_21","tableName" : "OUT_ZMMR200",
"tableParameter":{
"SRYEAR":"2023",
"SPOPER":"006",
"SBWART":"8A",
"SRBUKRS":"WL01",
"SRPRCTR_ILOW_I":"PE1000",
"SRPRCTR_IHIGH_I":"PE1300",
"SRACCT_ILOW_I":"12240000",
"SRACCT_IHIGH_I":"12240000"
}
```

#### Z_ZIMB_YSS1
``` json
"funcName" : "Z_ZIMB_YSS1","tableName" : "OUT_TXT",
"tableParameter":{
"WERKS":"TCSS",
"LGORT_ILOW_I":"CGC5",
"LGORT_IHIGH_I":"CGC6"}
```

#### Z_SD_RB6REPORT_YS
``` json
"funcName" : "Z_SD_RB6REPORT_YS","tableName" : "OUT_ZSDR022",
"tableParameter":{
"RVKORG":"0001",
"RERDAT_ILOW_I":"20220901",
"RERDAT_IHIGH_I":"20230930",
"RSPART":"11",
"list_RWERKS":["YSS1"]}
```

#### ZMMF1009

``` json
##1
	"funcName": "ZMMF1009",
	"tableName": "T_POSL",
	"tableParameter": {
		"List_T_WERKS": [
			{
				"SIGN": "",
				"OPTION": "",
				"LOW": "H204",
				"HIGH": ""
			},
			{
				"SIGN": "",
				"OPTION": "",
				"LOW": "HL04",
				"HIGH": ""
			}
		]
	}
##2
"funcName": "ZMMF1009",
		"tableName": "T_POSL",
		"tableParameter": {
			"List_T_WERKS": [
				{
					"SIGN": "I",
					"OPTION": "BT",
					"LOW": "WYW2",
					"HIGH": "YSS1"
				}
			],
			"List_T_MATNR": [
				{
					"SIGN": "I",
					"OPTION": "CP",
					"LOW": "1F011*",
					"HIGH": ""
				}
			],
			"List_T_BEDAT": [
				{
					"SIGN": "I",
					"OPTION": "BT",
					"LOW": "20190801",
					"HIGH": "20190802"
				}
			]
		}
```

#### Z_SD_RGETSDORDERS_03
``` json
"funcName" : "Z_SD_RGETSDORDERS_03","tableName" : "TRS001",
"tableParameter":{
"RVKORG":"0001",
"RSPART_ILOW_I":"06",
"RSPART_IHIGH_I":"08",
"RERDAT_ILOW_I":"20231205",
"RERDAT_IHIGH_I":"20231206"
}
```

#### Z_MM_ZIMR0043
``` json
"funcName" : "Z_MM_ZIMR0043","tableName" : "IT_DATA",
"tableParameter":{
"P_BUDAT":"12/18/2023"
}
```

#### Z_IM_GOODSMOVEMENTS_01
``` json
"funcName" : "Z_IM_GOODSMOVEMENTS_01","tableName" : "I_DATA",
"tableParameter":{
"P_BUDAT":"12/18/2020"}
```

#### Z_FI_RREADNIDATA_01
``` json
"funcName" : "Z_FI_RREADNIDATA_01","tableName" :"TRS001",
"tableParameter":{
"SVKBUR":"010",
"SVRGAR":"B",
"SPERIB":"2019001",
"SPERIE":"2019001",
"SKOKRS":"WL01", 
"SBUKRS":"WL01",
"SPRCTR":"PE1000",
"SWERKS":"YSS1",
"RARTNR":"0301040NI099_1"
}
```

#### Z_COR_IO_BUDGET_VALUE
``` json
"funcName" : "Z_COR_IO_BUDGET_VALUE", "tableName" : "TBPGE","tableParameter":{
"List_RAUFNR":["603794","603888"]
}  
```

#### ZRFC_ZIMR0041
``` json
"funcName" : "ZRFC_ZIMR0041", 
"tableName" : "OUTDATA",
"tableParameter":{
	"P_DATE":"2022-12-25",
	"P_MATNR":{"SIGN":"I","OPTION":"BT","LOW":"Y*","HIGH":""},
	"P_WERKS":{"SIGN":"I","OPTION":"BT","LOW":"YSS1","HIGH":"YSS2"}
}  
```

#### Z_IM_ZIM00015
``` json
"funcName" : "Z_IM_ZIM00015", 
"tableName" : "IT_DATA",
"tableParameter":{
	"S_BUDAT":"20221225"
}  
```

#### Z_MM_ZIM00015
``` json
"funcName" : "Z_MM_ZIM00015", 
"tableName" : "ITAB"
```

#### Z_MM_ZIMR0007
``` json
"funcName" : "Z_MM_ZIMR0007", 
"tableName" : "ITAB",
"tableParameter":{
	"WERKS":"20221225",
	"JOBNAME":"",
	"FIRST":"",
	"D_RQIDENT",""
}  
```

#### ZMM_ZPO00025B_FM
``` json
"funcName" : "ZMM_ZPO00025B_FM", 
"tableName" : "T_ANLA",
"tableParameter":{
	"T_ANLN1":[
                {"ANLN1":"5303062"},
                {"ANLN1":"5303061"}
	]
}  
```

#### BBP_RFC_READ_TABLE
``` json
"funcName" : "BBP_RFC_READ_TABLE",
"tableName" : "DATA",
"tableParameter":{
    "QUERY_TABLE":"GLPCA",
    "DELIMITER": "@",
    "ROWCOUNT": "10",
    "OPTIONS":
    [
        {"TEXT": "RYEAR = '2022' AND POPER = '006' AND"},
        {"TEXT": "RPRCTR IN ('PC1100','PC1200','PD1210','PD1220') AND"},
        {"TEXT": "RACCT IN ('0061200000','0061130010','0061130020')"}
    ],
    "FIELDS" : [
        {"FIELDNAME": "RYEAR"},
        {"FIELDNAME": "KOSTL"},
        {"FIELDNAME": "LIFNR"}
    ]
}
```

#### Z_IM_MATERIALCHANGE_01
``` json
// 輸出tableName：R_WERKS, OUT_DATA, IN_DATA, WH_MATERIALS_DATA, FINISHED_GOODS_DATA, WASTE_DATA, ASSETS_DATA
"funcName" : "Z_IM_MATERIALCHANGE_01",
"tableName" : "OUT_DATA",
"tableParameter":{
    "B_BUDAT": "20240101", 
    "E_BUDAT": "20240701"
}
```

#### Z_FI_ACC_DOCUMENT_POST_05
``` json
"funcName" : "Z_FI_ACC_DOCUMENT_POST_05",
"tableName" : "RETURN",
"tableParameter":{
    "P_BLDAT": "08/05/2024",
    "P_BLART": "SA",
    "P_GJAHR": "2024",
    "P_MONAT": "08",
    "P_BUDAT": "08/05/2024",
    "P_BUKRS": "WL01",
    "P_WAERS": "TWD",
    "P_KURSF": "",
    "P_WWERT": "",
    "P_BKTXT": "TEST",
    "ZACCOUNTGL": [
        {
            "BUZEI": "000001",
            "BSCHL": "40",
            "HKONT": "71410073",
            "SGTXT": "TEST1",
            "KOSTL": "",
            "EBELN": "",
            "EBELP": "00000",
            "AUFNR": "",
            "PRCTR": "PA1300",
            "WRBTR": "1.00",
            "WAERS": "",
            "VALUT": "",
            "ZUONR": "123"
        }
    ]
}
```

#### Z_FI_RREADSET_02
``` json
"funcName" : "Z_FI_RREADSET_02",
"tableName" : "STR",
"tableParameter":{
    "SETID": "PC9_ALL_F"
}
```

#### Z_FI_ACC_GETOPENITEMS
``` json
"funcName" : "Z_FI_ACC_GETOPENITEMS",
"tableName" : "IT_BSIS",
"tableParameter":{
    "I_BUKRS": "WL01",
    "I_HKONT": "0011039950",
    "I_KEYDATE": "10/07/2024"
}
```

#### Z_IM_MATERIAL_WEIGHT_01
``` json
"funcName" : "Z_IM_MATERIAL_WEIGHT_01",
"tableName" : "IT_DATA",
"R_MATNR":{
    "P_EXECDATE": "2025/03/01",
    "R_MATNR": [
        {
            "S": "I",
            "OPT": "EQ",
            "LOW": "030126SUS000",
            "HIGH": "030126SUS000"
        }
    ],
    "R_MATNR": [
        {
            "S": "I",
            "OPT": "EQ",
            "LOW": "030126SUS000",
            "HIGH": "030126SUS000"
        }
    ]
}
```

#### ZFM_AA_FIXED_ASSET_READ
``` json
"funcName": "ZFM_AA_FIXED_ASSET_READ",
"tableName": "OT_ASSET",
"tableParameter": {
	"I_BUKRS": "WLGR",
	"I_ANLKL": null,
	"I_DEAKTFLAG": null,
	"I_KOSTL": null,
	"I_ANLN1": null,
	"I_ANLN2": null,
	"I_TXT50": null,
	"I_INVNR": null,
	"I_SPRAS": null
}
```

#### Z_MM_INVENTORY_DATA_01
``` json
"funcName": "Z_MM_INVENTORY_DATA_01",
"tableName": "IT_DATA",
"tableParameter": {
    "RWERKS": [
        { "LOW": "SCP1" },
        { "LOW": "SCG1" }
    ],
    //"RLGORT": null,
    "RMATNR": [
        { "LOW": "5220727401007000" },
        { "LOW": "G121527103088000" }
    ],
    "RMTART": [
        { "LOW": "FERT" },
        { "LOW": "HALB" }
    ]
}
```

## 例子
``` json
{
  "Header": {
    "dept": "string",
    "UserNo": "string"
  },
  "PayLoad": {
	"funcName" : "Z_AR_CUST_OPEN_AR_01",
	"tableName" : "ZSDT070",
	"tableParameter":{}
	-- "pageFunction":{}
	-- "Rules":{}
  }
}
```


## 參數方法
> 請放在 `[Payload]` 其他參數方法 (ex:funcName,tableName)後方

##### 1. 環境: 測試環境 or 正式環境
> 當**未**使用時, **預設**為 正式環境
>
> **測試環境**: Test , **正式環境**: Official
``` json
"modeName":"Official"
```
## 額外方法
> 請放在 `Payload` 裡面並至於其他參數模型方法最後方

### 分頁 pageFunction
##### 1. 分頁: 半數
> **使用時機**: 當 DataFactory 無法讀取過大的資料的時候,需拆解
>
> 取得`半數`資料集,`pageNumber`可控制頁數 (參數為1,2)
``` json
"pageFunction":{"paging":true,"funcName":"Half","pageNumber":1}
```

##### 2. 分頁: 四分之一
> **使用時機**: 當 DataFactory 無法讀取過大的資料的時候,需拆解
>
> 取得`四分之一`資料集,`pageNumber`可控制頁數(參數為1,2,3,4)
``` json
"pageFunction":{"paging":true,"funcName":"Quarter","pageNumber":1}
```

##### 3. 分頁: 8分之一
> **使用時機**: 當 DataFactory 無法讀取過大的資料的時候,需拆解
>
> 取得`8分之一`資料集,`pageNumber`可控制頁數(參數為 1~8)
``` json
"pageFunction":{"paging":true,"funcName":"Eighth","pageNumber":1}
```

##### 4. 分頁: 10分之一
> **使用時機**: 當 DataFactory 無法讀取過大的資料的時候,需拆解
>
> 取得`10分之一`資料集,`pageNumber`可控制頁數(參數為 1~10)
``` json
"pageFunction":{"paging":true,"funcName":"Tenth","pageNumber":1}
```

### 規則 Rules

##### 1. 規則: Blob啟用table名稱
> **使用時機**: 當 Blob需要資料夾分開 table 名稱 之時

``` json
"Rules":{"BlobHasTableName":true}
```

