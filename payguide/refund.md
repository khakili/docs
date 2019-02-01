# 商户退款请求
**简要描述：** 

- 商户->联动优势
- 商户可对支付成功的订单发起退款申请，支持部分退款。
- 商户退款的金额须小于或等于可退款金额，支持对90天以内的成功订单发起退款。

**请求URL：** 

`https://mofunapi.umfintech.com/in-pay-client/online/refund`

**请求方式：**
- POST 

**请求参数：** 

|	字段	|	名称	|	长度	|	必填	|	说明	|
|----|----|----|----|----|
|	subMerId	|	商户号	|	8	|	M	|	商户号(联动平台分配)	|
|	payType	|	交易类型可选微信、支付宝、银联二维码	|	2	|	M	|	AL:支付宝<br>WX:微信<br>YL:银联二维码	|
|	proxyId	|	集成商ID	|	4	|	M	|	集成商唯一标识（同魔方登录账号）	|
|	amount	|	订单金额	|	13	|	M	|	如果是人民币，则以分为单位	|
|	partnerOrderId	|	商户订单号	|	32	|	M	|	商户的支付订单号	|
|	refundPartnerOrderId	|	退款订单号	|	32	|	M	|	退款订单号	|
|	orderDate	|	原订单日期	|	8	|	<span style="color:red">O	</span>|	yyyyMMdd	|
|	apiVersion	|	版本	|	3	|	M	|	定值:1.0	|
|	reqDate	|	请求日期	|	8	|	M	|	yyyyMMdd	|
|	reqTime	|	请求时间	|	6	|	M	|	HHmmss	|
|	sign	|	签名	|	256	|	M	|	参见签名机制	|
|	shopNo	|	门店编号	|		|	O	|	门店编号	|
|	operator	|	操作人	|		|	O	|	操作人	|
|	notifyUrl	|	通知地址	|	256	|	O	| 	结果通知地址. 必须以 http:// 或 https:// 开始, 支持大小写字母,数字,'/','&','%','?','=' . 暂不支持通知地址中包含其他字符,包含url编码后的结果 如%3C %3E等	|

 **商户请求报文示例**

```json
context={
	"amount": "1",
	"tradeNo": "1801534000007443",
	"orderId": "180116194213097N7NL00056778",
	"partnerOrderId": "0101180116193952366",
	"applicationSign": "D9:FE:77:58:30:3A:EC:A3:51:35:5F:D9:BC:CC:33:C8:5C:34:9C:F3",
	"channel": "Z2POS",
	"reqTime": "105252",
	"rpid": "P570116000018047",
	"version": "2.0",
	"applicationAppId": "02df974b143e458bb2644615e7438547",
	"posSN": "N7NL00056778",
	"subMerId": "30000101",
	"payType": "AL",
	"reqDate": "20180313",
	"clientIp": "10.10.51.127",
	"packageName": "com.umpay.payplugindemo",
	"funCode": "ZFBREVSV2",
	"orderDate": "20180115",
	"proxyId": "0025"
}
```

 **返回参数说明** 

|	字段	|	名称	|	长度	|	必填	|	说明	|
|----|----|----|----|----|
|	retCode	|	返回码	|	8	|	M	|	详见全局返回码	|
|	Memo	|	返回信息	|	128	|	M	|		|
|	sign	|	签名	|	256	|	M	|	参见签名机制	|
|	partnerOrderId	|	商户订单号	|	32	|	O	|	支付订单号	|
|	amount	|	退款金额	|	13	|	O	|	退款金额	|
|	orderDate	|	原订单日期	|		|	O	|	yyyyMMdd 	|

 **备注** 
- 更多返回错误代码请看[全局参数说明](单页面地址 : https://www.showdoc.cc/web/#/page/39370587223487 "全局参数说明")

**常见问题：**

【问题】：退款受理成功是否算是退款成功？
【答案】：退款接口返回“0000”即表示退款受理成功。
可以调用退款查询接口查询退款状态，实际以用户收到退款金额为准。

【问题】：退款成功后，钱什么时候到账？
【答案】：余额即时到账，银行卡3-5个工作日。

【问题】：退款受理成功，还可以再次发起退款吗？
【答案】：可以；

【问题】：什么情况算是退款失败？
【答案】：非“0000”的其他返回码算是退款失败。

【问题】：退款失败会发通知吗？
【答案】：不会。

【问题】：退款查询失败，还可以再次发起退款吗？
【答案】：可以。