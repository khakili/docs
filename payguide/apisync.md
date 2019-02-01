# 订单状态同步
  
**简要描述：** 

- 商户->联动优势
- 平台对商户的支付请求数据处理完成后，响应为非明确状态。如：[全局错误码中](https://www.showdoc.cc/web/#/6234326490827?page_id=39370587223487 "全局参数说明中")“01000000”、“02000000”、“03000000”三种返回码的情况下，需要调用同步接口，请求银行查询最终结果。

**请求URL：** 
`https://mofunapi.umfintech.com/in-pay-client/api/apisync`



**请求方式：**
- POST 

**请求参数：** 

|	字段	|	名称	|	长度	|	必填	|	说明	|
|----|----|----|----|----|
|	subMerId	|	商户号	|	8	|	M	|	商户号(联动平台分配)	|
|	payType	|	支付类型	|	2	|	O	|	WX：微信刷卡支付<br>AL：支付宝条码支付<br>YL：银联二维码支付	|
|	proxyId	|	集成商ID	|	4	|	M	|	集成商唯一标识	|
|	partnerOrderId	|	商户订单号	|	32	|	M	|	原来请求订单号	|
|	orderDate	|	原订单日期	|	8	|	<span style="color:red">O</span>	|	yyyyMMdd	|
|	apiVersion	|	版本	|	3	|	M	|	定值:1.0	|
|	sign	|	签名	|	256	|	M	|	参见签名机制	|
|	ymfFlag	|	是否一码付（是否是YmfPay）	|		|	O	|	1:是	|
|	shopNo	|	门店编号	|		|	O	|	门店编号	|
|	operator	|	操作人	|		|	O	|	操作人	|

 **YmfPay商户请求报文示例**

```json
context={
	"subMerId": "30000102",
	"payType": "WX",
	"apiVersion": "1.0",
	"partnerOrderId": "1509518653211",
	"sign": "fgJpXQpFKPsLxlNJvFK3MPc5x+GHEQru1Q+69m65358e8WTge9Djv9qT6wkJOijPOESOZNaWM1mDCePA7WaeWwdR9CjjLTzf9gVKmFNcSehTbUl2JW8WSg09dPqkfbZq9SFrg6vGC5HHf/Z9YJF82gtVlzIt4SzwxGx//EzTyPM=",
	"orderDate": "20171101",
	"proxyId": "0049",
	"ymfFlag": "1"
}
```
 **非YmfPay商户请求报文示例**
```json
context={
	"subMerId": "30000102",
	"payType": "WX",
	"apiVersion": "1.0",
	"partnerOrderId": "1509518653211",
	"sign": "p563KDqGXjOxHDoN7uWAoqd6S3Jxzdq9tj+8cg56BYPzZu/eSIMKWuVwHsrtEKYmZ40dvOtfzuJNmEqFoRUzgCllld+4eSVQwJXLeukJlwO/WOpUyBZnkNvZ8A1HqijKKwPrepKvWkYKBsGNhGfMnksnmaCmD8h1+o+hQkd3DZA=",
	"orderDate": "20171101",
	"proxyId": "0049"
}
```

 **返回参数说明** 
 
|	字段	|	名称	|	长度	|	必填	|	说明	|
|----|----|----|----|----|
|	retCode	|	返回码	|	8	|	M	|	返回码	|
|	Memo	|	返回信息	|	128	|	M	|	返回信息	|
|	paySeq	|	支付流水号	|		|	C	|	支付流水号，成功返回	|
|	payDay	|	支付日期	|		|	C	|	支付日期	|
|	amount	|	订单金额	|		|	C	|	订单金额 (打印小票使用)	|
|	coupAmount	|	优惠金额	|		|	C	|	优惠金额 单位:分	|
|	payAmount	|	实际支付金额	|		|	C	|	实际支付金额：单位:分<br>payAmount = amount - coupAmount	|
|	tradeNo	|	平台处理流水 	|		|	C	|	平台处理流水	|
|	platDate	|	平台日期	|		|	C	|	平台日期	|
|	platTime	|	平台时间	|		|	C	|	平台时间HHmmss、	|
|	partnerOrderId	|	商户订单号	|	32	|	M	|	订单号	|
|	**payType**	|	支付类型	|		|	C	|	支付类型：<br>BK-银行卡<br>WX-微信<br>AL-支付宝<br>YL-银联二维码	|
|	**easyPaymentService**	|	小额双免标识	|	|	C	|	0-非小额双免，1-小额双免	|
|	**cardType**	|	银行卡类型	|		|	C	|	00-借记卡 ，01-贷记卡	|
|	sign	|	签名	|	256	|	M	|	参见签名机制	|


 **备注** 
- 除了成功和支付中（需要再次发起查询）的返回码需要做对应的逻辑处理，其他返回码均不需要做处理
- 更多返回错误代码请看[全局参数说明](单页面地址 : https://www.showdoc.cc/web/#/page/39370587223487 "全局参数说明")

**常见问题：**

【问题】：如何区分“退款成功状态”还是“撤销成功状态”？
【答案】：目前在退款和撤销请求中我们只有“退款/撤销申请成功状态”，实际以用户收到退款金额为准。
在同步接口中，如果收到返回码“88881220”才明确表示“退款受理成功状态”；
在同步接口中，如果收到返回码“88881137”才明确表示“撤销成功状态”;

【问题】：你们文档上写了，今天的订单不能调用“退款接口”，应该调用“撤销接口”，那为什么会返回0000？还是说都可以调用“退款接口”？
【答案】：早期为了让商户不需要关注退款和撤销，简化开发逻辑，所以我们只提供一个退款接口（包含退款和撤销）。但是对于有对账需求的商户和未来可能需要进行对账的商户，强烈建议区分退款和撤销接口。

【问题】：为什么我们一笔退款退了好几次，每次都返回成功？
【答案】：退款目前返回的成功仅表示：“退款申请成功状态”，实际以用户收到退款金额为准；多次请求后台都返回成功，是因为在惠商+后台对于同一笔订单号做了幂等处理。

【问题】：同步接口商户侧的超时时间应该如何设置？
【答案】：同步接口建议商户的超时时间>1分钟

【问题】：退款和撤销接口没有拿到明确状态或者退款失败，接着发起同步请求，同步请求返回的结果0000的时候是“退款成功”和“撤销成功” 还是“退款和撤销申请成功”，具体结果还要再等待通知 ？
【答案】：这种情况下同步返回0000时，表示后台状态为：“支付成功”，也就是退款失败

【问题】：  现在 0000返回码表示是支付成功，还有3个 支付中的码01000000、02000000、03000000，其他状态我都无法通过这个接口确定（订单待支付、订单支付失败、订单已关单、订单退款成功等）,应该怎么区分。
【答案】：退款、撤销成功的返回码我们会马上加上。支付中和待支付都是是这三个01000000、02000000、03000000返回码、非支付中返回码就是订单关闭状态