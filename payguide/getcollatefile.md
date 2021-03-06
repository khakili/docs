# 对账文件下载
**简要描述：**

- 商户->联动优势


**请求URL：** 
`https://mofunapi.umfintech.com/comp-pospre-server/getCollateFile`
  
**请求方式：**
- POST 

**对账文件格式（响应报文中的fileContent字段）：**
``` 
文件第一行  
	文件总条数|文件总金额（文件交易种类为01的总订单金额减去文件交易种类为20的总订单金额）|
文件第二到N行  
	账期|商户订单号|交易种类（01-支付，20-退款）|支付方式（WX-微信，AL-支付宝，BK-银行卡，YL-银联二维码）|U付商户号|恵商商户号|商户简称|终端号|订单金额（单位：分）|卡类型（00-借记卡，01-贷记卡）|手续费（单位：分）|结算金额（单位：分）|门店编号|操作员编号|交易流水号
示例数据：
	3|30
	20181112|liandongbs4186259117|01|WX|41862591|41862591|惠商产品演示商户||10||0|10|||1811120200010583
	20181112|liandongbs4186259119|01|WX|41862591|41862591|惠商产品演示商户||10||0|10|||1811120200011139
	20181112|liandongbs4186259124|01|BK|41862591|41862591|惠商产品演示商户|P2L123456789123|10|00|0|10|950000000000000|0000000000000048|1811120200013492

```




**请求报文参数：**

|	字段	|	名称	|	长度	|	必填	|	说明	|
|----|----|----|----|----|
|	merId	|	集成商编号	|	4	|	M	|	惠商4位集成商编号	|
|	stlDate	|	结算日期	|	8	|	M	|	yyyyMMdd	|
|	sign	|	签名	|	256	|	M	|	参见签名机制	|


 **商户请求报文示例**

```json
context={
"sign":"SEvC7jXqVYL3/3QVwiK764Jg1wOasdfdazAdrMPxTpmLYhtkNNV42yCA/1AnMS4+PGmg2ksn+rzC2E6hKXlg/NpfHfCGTLSpasdasdasmdggYuCc8Q2Mn1uBhxJaJBof/AkrGMMskCHpE+asdfa3S6NlYI8PpC3g658Q=",
"stlDate":"20180809",
"merId":"0012"
}
```


 **返回参数说明** 


|	字段	|	名称	|	长度	|	必填	|	说明	|
|----|----|----|----|----|
|	retCode	|	返回码	|	8	|	M	|	返回码	|
|	retMsg	|	返回语	|	128	|	M	|	返回语	|
|	merId	|	集成商编号	|	4	|	M	|	惠商4位集成商编号，同请求	|
|	stlDate	|	结算日期	|	8	|	M	|	yyyyMMdd，同请求	|
|	fileContent	|	对账文件内容	|	不限	|	M	|	对账文件正文，参照对账文件格式	|
|	sign	|	签名	|	256	|	M	|	参见签名机制	|

 **备注** 
- 更多返回错误代码请看[全局参数说明](单页面地址 : https://www.showdoc.cc/web/#/page/39370587223487 "全局参数说明")