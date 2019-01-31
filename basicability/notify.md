# 3.1交易结果通知
**简要描述：** 

- 联动优势->商户
- 联动平台对商户的支付请求数据处理完成后，会将处理的支付结果数据通过服务器异步通知的方式回调商户指定的URL。

**商户回调地址URL：** 
- 支持通过下单接口参数上送，或业务管理平台手工配置。
  
**回调请求方式：**
- HTTP 1.1  POST 方式
- 请求头内容类型 Content-Type: application/json
- 字符集 UTF-8

**平台回调商户请求参数：** 

|字段|名称|长度|必填|示例|说明|
|---|---|---|---|---|---|
|	funCode	|	功能码	|		|	M	|PayResultNotify|	定值：PayResultNotify	|
|	reqDate	|	请求日期	|		|	M	|	20180411|yyyyMMdd	|
|	reqTime	|	请求时间	|		|	M	|	150344|HHmmss	|
|	merId	|	联动商户号	|		|	C	|	41444694|联动商户号,与结算账户相关（一般用不上）	|
|	payType	|	支付类型	|		|	M	|WX|	WX-微信<br>AL-支付宝<br>BK-银行卡<br>YL-银联二维码	|
|	shopNo	|	门店号	|		|	C	|953000000343035	|惠商+平台门店号	|
|	operatorId	|	操作员ID	|		|	C	|	yjy01|惠商+平台门店操作员ID	|
|	deviceType	|	设备类型	|		|	M	|	POS|POS，代表智能POS<br>Other，代表其他	|
|	deviceInfo	|	设备号	|		|	C	|	966000000002053|如设备为“POS”，则设备号(POS SN)<br>如设备为“Other”，则该字段可能为空	|
|	bpid	|	第三方平台BPID	|		|	O	|	|预留	|
|	orderDate	|	订单日期（下单时间）	|		|	M	|20180318	|yyyyMMdd	|
|	partnerOrderId	|	商户订单号	|		|	M	|180509171057718113335	|由终端上送的第三方平台订单号	|
|	amount	|	付款金额	|		|	M	|100	|人民币，且以分为单位	|
|	tradeState	|	交易状态	|	变长16	|	M	|TRADE_SUCCESS	|TRADE_SUCCESS 交易成功<br>TRADE_CANCEL 交易撤销<br>REFUND_SUCCESS 交易退款成功<br>详见交易状态说明	|
|	paySeq	|	银行流水	|	变长32	|	O	|	4200000112223805095997813703|银行流水号	|
|	accountNo	|	银行卡号	|	变长32	|	O	|	|当交易为银行卡支付时返回, 该卡号为脱敏卡号.如:621700*********0034	|
|	**easyPaymentService**	|	**小额双免**	|		|**O**|	|**[Y/N] 小额双免交易**	|
|	**cardType**	|	**银行卡类型**	|		|**O**|	|**00-借记卡 01-贷记卡**	|
|	**refundPartnerOrderId**	|	**退款流水号**	|		|**O**|	|**在api退款时上传**	|
|**nfcMobilePayment**|**NFC移动支付**||O||**[Y/N] NFC移动支付交易**|
|outOrderId|第三方平台订单号|64|O|1811280200000485 同3.2第三方订单查询接口中同名字段||
|promotionDetail|优惠信息|1024|O|[{"promotion_id":"2000000056881516050","name":"微信支付到店红包",<br>"scope":"GLOBAL","type":"COUPON","amount":6,"activity_id":"9244500",<br>"wxpay_contribute":6,"merchant_contribute":0,"other_contribute":0}]||
|bankType|银行类型|16|O|GDB_CREDIT 银行类型编码详情见微信,支付宝官方文档<br>微信参见：https://pay.weixin.qq.com/wiki/doc/api/native.php?chapter=4_2||
|buyerLogonId|买家支付宝账号（脱敏）|64|O|184****5520||
|buyerIdForPromotion|买家支付宝用户Uid|64|O|2088212249696681||
|openId|用户在商户appid下的唯一标识|64|O|oEufkwAlVRcKh9k4S8Oftt8y4jQE||
|subOpenId|用户在子商户appid下的唯一标识|64|O|||
|	sign	|	签名	|	256	|	M	|	|见下方注意事项	|

**注意：** 
- <span style="color:red">商户侧处理联动的返回报文时不要使用硬编码的方式，即采用固定写法获取某些字段的值，而需采用循环遍历的方法获取联动返回的所有字段；</span>
- 通知的频率为：第一次立即通知、第二次 30秒、第三次 300秒、第四次 1800秒（再未收到成功响应时，会持续发通知，但最多只通知四次）；
- <span style="color:red">建议：被动回调通知与主动查询配合使用，以保证订单状态一致性；</span>
- 联动通知接口的请求报文验签方式，待加签串如下（注意asc排序）：

amount=1&deviceInfo=P2L01000000F9000289&deviceType=POS&funCode=PayResultNotify&merId=41193697&operatorId=301xxx6&orderDate=20180329&partnerOrderId=88800Dxxx192486&paySeq=1755105xxx956105&payType=WX&reqDate=20180329&reqTime=172149&shopNo=966000000122268&tradeState=TRADE_SUCCESS

**对于有些非必传字段，如果为空，不参与加签，比如：对于没有门店编号的商户shopNo为空，那么待加签串为：amount=1&deviceInfo=P2L01000000F9000289&deviceType=POS&funCode=PayResultNotify&merId=41193697&operatorId=301xxx6&orderDate=20180329&partnerOrderId=88800Dxxx192486&paySeq=1755105xxx956105&payType=WX&reqDate=20180329&reqTime=172149&tradeState=TRADE_SUCCESS**

**交易状态说明：** 

|序号|枚举名称|说明|备注|
|---|---|---|---|
|	1	|	WAIT_BUYER_PAY	|	交易创建，等待买家付款。	|	不进行通知	|
|	2	|	TRADE_SUCCESS	|	交易成功	|	 通知	|
|	3	|	TRADE_CLOSED	|	交易关闭，在指定时间段内未支付时关闭的交易	|	不进行通知	|
|	4	|	TRADE_CANCEL	|	交易撤销	|	通知	|
|	5	|	TRADE_FAIL	|	交易失败	|	不进行通知	|
|	6	|	REFUND_SUCCESS	|	退费成功	|	通知	|
|	7	|	REFUND_FAIL	|	退费失败	|	通知	||






**平台请求报文示例：**
```json
{
	"amount": "1",
	"bpid": "",
	"deviceInfo": "",
	"deviceType": "Other",
	"funCode": "PayResultNotify",
	"merId": "414545847",
	"operatorId": "301xxx6",
	"orderDate": "20180313",
	"partnerOrderId": "88800Dxxx192486",
	"paySeq": "1755105xxx956105",
	"payType": "AL",
	"reqDate": "20180313",
	"reqTime": "110343",
	"shopNo": "966000000122268",
	"sign": "QNuBBKhs2TE67L2oslDVJwNZwuztcbdrA/CMebCkJo+y3XAztLnfdfdsfdsfsdfdsyu9QABrjnwaSiEPIh0PinSKrOOJBLIYOf3B6Qt3/iPo3ukMTJiYCXnkqsdfsDFSFSobMfAzdxtHTuUVLzyZtgKW44+T5UirBgu003d",
	"tradeState": "TRADE_SUCCESS"
}
```


 **商户给平台返回参数**
 
|字段|名称|长度|必填|说明	|
|---|---|---|---|---|
|	funCode	|	交易功能码	|	16	|	M	|	同请求数据	|
|	reqDate	|	请求日期	|	8	|	M	|	同请求数据	|
|	reqTime	|	请求时间	|	6	|	M	|	同请求数据	|
|	retCode	|	返回状态码	|	4	|	M	|	该返回码和联动通知商户的交易是否成功无关，只要商户方成功接收到支付结果通知且验签成功，商户均应返回0000	|
|	retMsg	|	返回信息	|	128	|	O	|	返回信息，如非空，为错误原因<br>签名失败、参数格式校验错误	|
|	partnerOrderId	|	商户订单号	|	32	|	M	|	同请求数据	|
|	orderDate	|	订单日期	|	8	|	M	|	同请求数据	|
|	sign	|	签名	|	256	|	M	|	参见下方注意事项	|

**商户响应报文示例：**
```json
{
	"funCode": "PayResultNotify",
	"orderDate": "20180329",
	"partnerOrderId": "88800Dxxx192486",
	"reqDate": "20180329",
	"reqTime": "172123",
	"retCode": "0000",
	"retMsg": "",
	"sign": "LY4GQ3efL8nn9TPr1o4vcj0XHeb66h4/9jDLmb3fc6tYPN+r2jQYr9mAanmXFf9fRg3VotzGr3KL5TrVfwJ9FQ/IJYk46wEpQhTMfTg4KWwiXX9GPPMqHZnFQ19lLdGA2DBu13kcrrBjqtIBTFYNWDcJ5AyJENBRH2d4D8bKGqA="
}
```


**注意：** 
-  <span style="color:red">响应联动通知接口报文的加签方式，待加签串为（注意asc排序）：</span>
	
`PayResultNotify|20180316|88800D****192486 |20180316|163323|0000`

因为retMsg为空所以示例中没有参与加签


