# 商户撤销交易

    
**简要描述：** 

- 商户->联动优势
- 针对交易当天的成功订单可以进行撤销，过了账期则需要走退费接口。**仅支持微信支付宝刷卡交易（即用户被扫）的撤销，其他支付成功订单如需实现相同功能请调用2.3退款请求接口**

**请求URL：** 

`https://mofunapi.umfintech.com/in-pay-client/online/repeal`
  
**请求方式：**
- POST 

**请求参数：** 

|字段|名称|长度|必填|说明|
|----|----|----|----|----|
|subMerId      |	商户号	            |	8	|	M	|	商户号(联动平台分配)	|
|payType       |	交易类型可选微信或支付宝	|	2	|	M	|	AL:支付宝撤销</br>WX:微信撤销</br>YL:银联二维码撤销	|
|proxyId       |	集成商ID	            |	4	|	M	|	集成商唯一标识（同魔方平台登录账号）	|
|amount        |	订单金额	            |	13	|	M	|	如果是人民币，则以分为单位	|
|partnerOrderId|	商户订单号	        |	32	|	M	|	商户的支付订单号	|
|orderDate     |	原订单日期	        |	8	|	M	|	yyyyMMdd 	|
|apiVersion    |	版本	                |	3	|	M	|	定值:1.0	|
|reqDate       |	请求日期	            |	8	|	M	|	yyyyMMdd	|
|reqTime       |	请求时间	            |	6	|	M	|	HHmmss	|
|sign          |	签名	|	256	|	M	|	参见签名机制	|
|shopNo        |	门店编号	|		|	O	|	门店编号	|
|operator      |	操作人	|		|	O	|	操作人	|
|notifyUrl     |	通知地址	|	256	|	O	| 	结果通知地址. 必须以 http:// 或 https:// 开始, 支持大小写字母,数字,'/','&','%','?','=' . 暂不支持通知地址中包含其他字符,包含url编码后的结果 如%3C %3E等	|

 **商户请求报文示例**

```json
context={
	"subMerId": "30000102",
	"amount": "1",
	"payType": "WX",
	"apiVersion": "1.0",
	"reqDate": "20180313",
	"partnerOrderId": "1509517040403",
	"sign": "Ys1Pfdri0tXSWA7f5XvcLlez9urz5d8GtvOMZygPxSC8T0gfWiJ/RrB6jt0YYCflhF6H7efhwIOBH4S5fQZRIHeHaQ8s33dk+vYJYH42nVzm6FMe6fSX7uWyZd6AkiXELu7mdrx0JUbknFPR+oAl98hbWkcCLZuBoTWJWtfF5MY=",
	"reqTime": "105103",
	"orderDate": "20171101",
	"proxyId": "0049"
}
```

 **返回参数说明** 

|字段|名称|长度|必填|说明|
|----|----|----|----|----|
|	retCode	|	返回码	|	8	|	M	|	详见全局返回码	|
|	Memo	|	返回信息	|	128	|	M	|		|
|	sign	|	签名	|	256	|	M	|	参见签名机制	|
|	partnerOrderId	|	商户订单号	|	32	|	O	|	支付订单号	|
|	amount	|	退款金额	|	13	|	O	|	退款金额以分为单位	|
|	orderDate	|	订单日期	|	8	|	O	|	原订单日期	|


 **备注** 
- 更多返回错误代码请看[全局参数说明](单页面地址 : https://www.showdoc.cc/web/#/page/39370587223487 "全局参数说明")


