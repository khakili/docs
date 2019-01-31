# 3.2第三方订单查询
**简要描述：** 

- 联动优势->商户
- 联动平台会请求商户指定的URL地址，获取订单的支付金额，配合3.1交易结果通知接口，打通商户的订单系统。

  
**请求方式：**
- POST 

** 请求参数**

|编号| 标签| 名称| 限制| 长度 |描述 |
|---|---|---|---|---|---|
|1. | funCode |交易功能码 |M| 16| ORDERQUERY |
|2. | reqDate |请求日期 |M |8| 请求日期 |
|3. | reqTime |请求时间 |M |6| 请求时间 |
|4. |  rpid   |消息流水 |M |16| 消息流水 |
|5. |  shopNo |门店号   |C| 32| 惠商+平台门店号 |
|6. | operatorId| 操作员ID |C| 32| 惠商+平台门店操作员ID |
|7. | deviceType| 设备类型 |M |32 |POS，代表智能POS<br> Other，代表其他 |
|8. | proxyId| 集成商Id |M |4 |集成商4位标识 |
|9.  |deviceInfo |设备号 |O| 32 |如设备为“POS”，则设备号(POS SN) <br>如设备为“Other”，则该字段可为空 |
|10.  |bpid| 第三方平台BPID |O | 预留 ||
|11. | outOrderId| 第三方平台订单号 |M |32 |第三方平台订单号|
| 12.|  userId |第三方平台会员ID| O|  |预留|
| 13.|  mobileId |手机号| O|   |预留|
| **14.**|  **subMerId** |**魔方商户号**| **M**|** 8 **|**魔方商户号**|
|15.|  sign| 签名| M ||||

**平台请求报文示例：**
```json
{
	"deviceType": "POS",
	"reqTime": "163224",
	"rpid": "03163224045859292191",
	"deviceInfo": "P2L01000000F7000098",
	"subMerId": "99960001",
	"reqDate": "20180705",
	"outOrderId": "2018070517232800460",
	"funCode": "QueryOrder",
	"operatorId": "30000085",
	"shopNo": "966000000000015",
	"proxyId": "0001",
	"sign": "ezOGQs4CtMKdJxmvl46CMW1XAencf6UCAz3QuVVadQuS0xm4XJpnMKXAXHxrAJG8ZoakxooZHz5mqSk1binzck1FXFzWlEK4nHl5RgJMCosxgkbEW5ymIaPQaP+DiJ5iF44Sey+LSsZdX3/xGSXdoTsKTp8RcM+7Pv6jfQ1s4X0="
}
```

**响应参数**

|编号| 标签| 名称| 限制| 长度 |描述 |
|---|---|---|---|---|---|
|1.  |funCode       |    交易功能码     | M |16   |同请求数据                                          |
|2.  |reqDate       |    请求日期      | M | 8   | 同请求数据                                         |
|3.  |reqTime       |    请求时间      | M | 6   | 同请求数据                                         |
|4.  |rpid          |    交易序列号     |M  |16   |同请求数据                                          |
|5.  |retCode       |    返回码       |  M|  8  |  0000 成功/其他为失败.                               |
|6.  |retMsg        |    返回信息      | M | 128 |  返回信息，如非空，为错误原因 签名失败、参数格式校验错误                 |
|7.  |bpid          |    第三方平台BPID |O  |  预留   |                                             |
|8.  |outOrderId    |    订单号       |  M|  32 |  同请求数据                                        |
|**9.**  |**partnerOrderId**    |    **支付流水**       |  **M**|  **64 **| ** 支付流水号**                                     |
|10.  |orderDate     |    订单日期      | O | 8   |  同请求数据                                        |
|11. | orderInfo    |    订单内容      | O | 128 |  订单数据                                         |
|12. | amount       |    应付金额      | M | 13  |  订单总金额，只能为整数，单位为分                             |
|13. | payType      |    支付渠道      | O | 8   |  银行卡：BK <br>支付宝：AL<br> 微信：WX|
|14.  |sign |签名| M           ||||

**商户响应报文示例：**
```json
{
	"amount": "1",
	"funCode": "QueryOrder",
	"orderDate": "20180705",
	"outOrderId": "2018070517232800460",
	"partnerOrderId": "2018070517232800460",
	"reqDate": "20180705",
	"reqTime": "163224",
	"retCode": "0000",
	"retMsg": "查询成功",
	"rpid": "03163224045859292191",
	"sign": "otXJ8j7wgB0aXTA1Q8Z+nOhCQBj+WTwMu/QUshM9HD9dKugdeW5KBc9/OXCqzD8WQgZqiCXQRYto2hGjxGPh6NNDJM8iYbI1wcLirStaxVeaFs1t/HpRU/UcwIoHxKOR7WFFHXH4vcW5RHV5b7mtS53FUHWN5R8FhEjy4fuqTvo="
}
```

**返回码**

|序号	|返回码	  | 描述            |
|---|---|---|
|1	| 0000	    |允许支付         |
|2	| ORDER_STATE_PAID	    |已支付的订单          |
|3	| ORDER_STATE_ERROR	    |订单状态异常            |
|4	| ORDER_NOT_EXIST	  |订单不存在      |

- 更多返回错误代码请看[全局参数说明](单页面地址 : https://www.showdoc.cc/web/#/page/39370587223487 "全局参数说明")

**注意事项**
- 若不支持或不需要重复支付，响应中outOrderId可与partnerOrderId相同，若需要支持组合支付或多次支付，则需要上送不同的支付流水号，并存在partnerOrderId字段中。 
