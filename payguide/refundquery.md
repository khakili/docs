# 退款订单查询
**简要描述：**
- 商户->联动优势

**请求URL：** 
`https://mofunapi.umfintech.com/in-pay-client/refund/query`

**请求方式：**
- POST 

**请求参数：** 

|	字段	|	名称	|	长度	|	必填	|	说明	|
|----|----|----|----|----|
|	subMerId	|	商户号	|	8	|	M	|	商户号(联动平台分配)	|
|	proxyId	|	集成商ID	|		|	M	|	集成商唯一标识	|
|   reqDate     |请求日期  |8 | M    |20180101 |
|   reqTime |请求时间   |6 |M   |132018  |
|	payType	|	支付类型	|		|	O	|	WX：微信刷卡支付<br>AL：支付宝条码支付<br>YL：银联二维码支付	|
|	partnerOrderId	|	商户订单号	|	32	|	M	|	原来请求订单号	|
|	refundPartnerOrderId	|	退款订单号	|	32	|	M	|		|
|	orderDate	|	原订单日期	|		|	<span style="color:red">O</span>	|	yyyyMMdd	|
|	refundDate	|	发起退费日期	|		|	<span style="color:red">O</span>	|	yyyyMMdd	|
|	version	|	版本	|		|	M	|	定值:1.0	|
|	sign	|	签名	|	256	|	M	|	参见签名机制	|
|	shopNo	|	门店编号	|		|	O	|	门店编号	|
|	operator	|	操作人	|		|	O	|	操作人	|

 **商户请求报文示例**

```json
context="{\"applicationAppId\":\"\",\"applicationSign\":\"\",\"funCode\":\"THQUERY\",\"operator\":\"\",\"orderDate\":\"20180913\",\"packageName\":\"\",\"partnerOrderId\":\"1809131019330032\",\"payType\":\"\",\"posSN\":\"\",\"proxyId\":\"***\",\"refundDate\":\"20180913\",\"refundPartnerOrderId\":\"20180913101318\",\"reqDate\":\"20181015\",\"reqTime\":\"175001\",\"shopNo\":\"\",\"sign\":\"aMz2kOtP6knlEkfMqo4NPG1IC44S7rZRxs9t1m3axLW+5W4YvwiQKd043hDppwLtS0BESHuIgNgPUqWsTebiYTDyoirJNgjPpX9P1Ua2joXfa/aqjqL82G5EKHFvOfxFlDg7NOjE8/dtpW4eiqoJaxnkaRhcH+wFlbfUKQBcYY8=\",\"subMerId\":\"30029272\"}"
```

 **返回参数说明** 
 
|	字段	|	名称	|	长度	|	必填	|	说明	|
|----|----|----|----|----|
|	retCode	|	返回码	|	8	|	M	|	返回码	|
|	retMsg	|	返回信息	|	128	|	M	|	返回信息	|
|	resDate	|	平台日期	|	8	|	C	|	平台日期	|
|	resTime	|	平台时间	|	6	|	C	|	平台时间HHmmss、	|
|	amount	|	订单金额	|	10	|	C	|	订单金额 (打印小票使用)	|
|	status	|	退款状态	|	1	|	C	|	<span style="color:red">1、成功，2失败，其他当受理中	</span>|
|	partnerOrderId	|	商户订单号	|	32	|	C	|	原来请求订单号	|
|	refundPartnerOrderId	|	退款订单号	|	32	|	C	|	原来请求订单号	|
|	subMerId	|	商户号	|	8	|	C	|	商户号(联动平台分配)	|
|	proxyId	|	集成商ID	|	4	|	C	|	集成商唯一标识	|
|	orderDate	|	原订单日期	|	8	|	<span style="color:red">O</span>	|	yyyyMMdd	|
|	refundDate	|	发起退费日期	|	8	|	<span style="color:red">O</span>	|	yyyyMMdd	|
|	sign	|	签名	|	256	|	M	|	参见签名机制	|


 **备注** 
- 响应报文中，retCode=0000,请求退款查询成功，retCode!=0000,请求退款查询失败。
- <span style="color:red">查询成功后，获取退款订单状态（status），根据status判定这笔退款订单成功或者失败。</span>
- 更多返回错误代码请看[全局参数说明](单页面地址 : https://www.showdoc.cc/web/#/page/39370587223487 "全局参数说明")