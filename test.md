**简要描述：** 
- 商户->联动优势
- 商户通过接口下单成功后，使用返回的预下单信息在微信公众号、微信小程序或支付宝服务窗中调起支付插件，消费者完成支付。调起支付插件方法参考如下：
微信：https://pay.weixin.qq.com/wiki/doc/api/jsapi.php?chapter=7_7
支付宝：https://docs.open.alipay.com/common/105591
- 请通过主动调用**2.6订单状态同步**接口，配合**3.1交易结果通知**（异步回调）来保证订单状态一致性
- 交易时序图如下
![](https://www.showdoc.cc/server/api/common/visitfile/sign/0167ee2c2179ebac08540e90ff213f4d?showdoc=.jpg)

**请求URL：** 
`https://mofunapi.umfintech.com/in-pay-client/api/unifiedcode`

  
**请求方式：**
- POST


**请求数据：** 

|	字段	|	名称	|	长度	|	必填	|说明|	示例	|
| ------------ | ------------ |
|	partnerOrderId	|	商户订单号	|	32	|	M	|	商户的支付订单号|180509171323918113335	|
|	subMerId	|	商户号	|	8	|	M	|	商户号(联动平台分配)|30200102	|
|	amount	|	订单金额	|	10	|	M	|	单位:分|	100|
|	payType	|	支付类型	|	2	|	M	|WX： 微信支付 AL： 支付宝支付|	WX	|
|	openId	|	微信或支付宝用户标识	|	28	|	M	|微信上传用户openid；</br>支付宝上传用户user_id；	|	微信openid：oUMnsvyNYu6pjDvpJzcXe3PvGXPY</br>支付宝user_id：2088522626852000|
|	appId	|	APPID	|	18	|	C	|微信及支付宝的AppId，如获取OpenID</br>所使用的AppID非下单商户主体资质，则</br>该字段无需上传	|	wx102e43f81031d8d6|
|	proxyId	|	集成商ID	|	4	|	M	|	集成商唯一标识（同魔方登录账号）	|0097|
|	apiVersion	|	版本	|	3	|	M	|传递:1.0	|1.0		|
|	sign	|	签名	|	256	|	M	|参见签名机制	|	|
|	shopNo	|	门店编号	|		|	O	|	门店编号	|
|	operator	|	操作人	|		|	O	|	操作人	|
|	goodsInfo	|	商品信息	|	80	|	O	|可上送商品描述、商户订单号等信息，</br>用户付款成功后会在微信账单页面展示|例：家政培训费（订单号2018080812635453）|
|	sub_appid	|	子商户appid	|	18	|	O	|二级商户的appId	|	wx102e43f81031d8d6|
|	extData	|	扩展信息	|	128	|	O	|	保留扩展字段，以 `I`（竖线）分隔	| 扩展信息1`I`扩展信息2|
|	reduceAmt	|	抵扣金额	|	10	|	O	|	商家优惠金额	|0|
|	expairTime	|	订单有效时间(秒)	|	4	|	O	|当传递小于300秒或大于1800秒或</br>不传递时系统默认为300秒。订单有</br>效时间从调起用户密码键盘开始算起</br>，超时之后,用户无法继续支付。	|600	|
|	notifyUrl	|	通知地址	|	256	|	O	|	结果通知地址. 必须以 http:// 或 https:// 开始,支持大小写字母,数字,'/','&',</br>'%','?','='. 暂不支持通知地</br>址中包含其他字符,包含url编码后的结果 如%3C %3E等	| |

		


 **商户请求报文示例**
``` 
context={
	"subMerId": "30000102",
	"amount": "9",
	"payType": "WX",
	"apiVersion": "1.0",
	"partnerOrderId": "HSAPI1520425994612",
	"openId": "122010000151",
	"appId": "111",
	"sign": "FI8kw10QzZICpj2xRjsYcmU+HI7oNFm4KaNSPeT4YbmG7izCV4m9jZJQ1gxkny0bt5xY8MZXXtzFeRR5KEyzp2YFYMC0AFjvsd/5HGlE6JxrVKNg/LhIba7aR7WMrX4FtEcmBm4ILMosgVhf665KgGtdHBuCd5qRfAs217iPWd0=",
	"proxyId": "0049"
}
```
 **返回参数说明** 


|	字段	|	名称	|	长度	|	必填	|	说明|	示例|
| ------------ | ------------ |
|	retCode	|	返回码	|	8	|	M	|0000支付成功	|	0000|
|	subMerId	|	商户号	|	8	|	M	|同原请求	|	30200102|
|	resDate	|	响应日期	|		|	M	|格式：yyyyMMdd	|20180223	|
|	resTime	|	响应时间	|		|	M	|格式：HHmmss	|	172344|
|	payType	|	支付类型	|		|	M	|WX： 微信刷卡支付<br>AL: Alipay支付宝条码支付	|	WX|
|	openId	|	用户授权标识	|		|	M	|		|122010000151|
|	appId	|	APPID	|		|	M	|	微信及支付宝的AppId|	2c91aa1843444d09a5a7ca811955bbe2|
|	Memo	|	返回码描述	|		|	M	|	|	交易成功|
|	prepayId	|	预支付ID	|		|	M	|调起支付插件需要	|	prepay_id=wx091722014693444a557c8f280689718193|
|	hostLsNo	|	交易流水号	|		|	M	|	|	8229172300153405|
|	timestamp	|	时间戳	|		|	C	|调起微信支付插件需要，</br>支付宝可不填	|	1525857061537|
|	partnerOrderId	|	商户订单号	|		|	M	|同原请求	|180509171323918113335|
|	nonceStr	|	随机数	|		|	C	|调起微信支付插件需要，</br>支付宝可不填	|	1525857061537|
|	signType	|	签名算法	|		|	C	|调起微信支付插件需要，</br>支付宝可不填|		MD5|
|	paySign	|	H5签名	|		|	C	|调起微信支付插件需要，</br>支付宝可不填	|	73DBF7459A11BCCED6D8F45EA660A416|
|	sign	|	签名	|	256	|	M	|参见签名机制	||



 **备注** 


- 更多返回错误代码请看[全局参数说明](单页面地址 : https://www.showdoc.cc/web/#/page/39370587223487 "全局参数说明")