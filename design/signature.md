# 数字签名
## 说明
数据传输过程中的数据真实性和完整性，我们需要对数据进行数字签名，在接收签名数据之后进行签名校验。
## 签名机制
### 请求数据-加签
1.请求参数按照ASCII码表的顺序进行排列
- 加签首先将证书转成二进制字节数组，然后根据证书获取私钥，再设置RSA的加密方式，最后用base64Encode即可得到签名串（注意：字符编码集为UTF-8 ）。

2.所有参数名和参数值（除了sign）按照上面的排序用“&amp;”连接起来，格式是：

`p1=v1&p2=v2`
-  <span style="color:red">注意：空值不参与加签，见举例2</span>

3.签名时将字符转化成字节流时应指定UTF-8的字符集。
-  <span style="color:red">各参数的参数值的前后不允许出现空白符，否则将影响签名的正确性。</span>

4.JAVA签名方式，详见1.3小节。

**举例1:**  
调用某接口需要以下参数，接口定义的参数名顺序如下：

|序号|字段|值（举例）|
|------| ------------ |----|
|1|subMerId|subMerId=99960001|
|2|payType|payType=AL|  
|3|proxyId|proxyId=0025|   
|4|amount|amount=1234|  
|5|partnerOrderId|partnerOrderId=HSAPI619585101312876||

按照asc排序后：

|序号|  字段 |值（举例）|
|----|----|----|
|1|amount|amount=1234|  
|2|partnerOrderId|partnerOrderId=HSAPI619585101312876|
|3|payType|payType=AL|  
|4|proxyId|proxyId=0025|   
|5|subMerId|subMerId=99960001||

那么待签名的数据就是：
amount=1234&amp;partnerOrderId=HSAPI619585101312876&amp;payType=AL&amp;proxyId=0025&amp;subMerId=99960001

使用私钥签名得到：
"ugDbqdUu0aan7bncsaxIlcf+3NzbngGtZJkPuVTg4JyZ1vpEhd+Ln5fWOFGDbgtQ2rEQYkrYjWi5uvKauN+qBs2NkoWT/bkUeR3lmdQcRnhCMde9qF45meLjN2qfdJlkvxUye9JmdQX1AepJltYf3ss6orR8z5O3ViPJHVqiByo="

发送给后台的报文就是：
```json
context = {
	"amount": "1234",
	"sign": "ugDbqdUu0aan7bncsaxIlcf+3NzbngGtZJkPuVTg4JyZ1vpEhd+Ln5fWOFGDbgtQ2rEQYkrYjWi5uvKauN+qBs2NkoWT/bkUeR3lmdQcRnhCMde9qF45meLjN2qfdJlkvxUye9JmdQX1AepJltYf3ss6orR8z5O3ViPJHVqiByo=",
	"partnerOrderId": "HSAPI619585101312876",
	"payType": "AL",
	"proxyId": "0025",
	"subMerId": "99960001"
}
```

**举例2:**  
调用某接口需要以下参数，接口定义的参数名顺序如下：

|序号|  字段 |值（举例）|
|----|----|----|
|1|subMerId|subMerId=99960001|
|2|payType|payType=AL|  
|3|proxyId|proxyId=0025|   
|4|amount|amount=1234|  
|5|partnerOrderId|partnerOrderId=HSAPI619585101312876|
|6|shopId|shopId="" <span style="color:red">（空值）</span>||

去掉空值，按照asc排序后：

|序号|字段|值（举例）|
|----|----|----|
|1|amount                    |  amount=1234|  
|2|partnerOrderId|partnerOrderId=HSAPI619585101312876|
|3|payType                   |  payType=AL|  
|4|proxyId                   |proxyId=0025|   
|5|subMerId                  | subMerId=99960001   ||

那么待签名的数据就是：
amount=1234&amp;partnerOrderId=HSAPI619585101312876&amp;payType=AL&amp;proxyId=0025&amp;subMerId=99960001

使用私钥签名得到：
"ugDbqdUu0aan7bncsaxIlcf+3NzbngGtZJkPuVTg4JyZ1vpEhd+Ln5fWOFGDbgtQ2rEQYkrYjWi5uvKauN+qBs2NkoWT/bkUeR3lmdQcRnhCMde9qF45meLjN2qfdJlkvxUye9JmdQX1AepJltYf3ss6orR8z5O3ViPJHVqiByo="

发送给后台的报文就是：
```json
context = {
	"amount": "1234",
	"sign": "ugDbqdUu0aan7bncsaxIlcf+3NzbngGtZJkPuVTg4JyZ1vpEhd+Ln5fWOFGDbgtQ2rEQYkrYjWi5uvKauN+qBs2NkoWT/bkUeR3lmdQcRnhCMde9qF45meLjN2qfdJlkvxUye9JmdQX1AepJltYf3ss6orR8z5O3ViPJHVqiByo=",
	"partnerOrderId": "HSAPI619585101312876",
	"payType": "AL",
	"proxyId": "0025",
	"subMerId": "99960001"
	"shopId": ""
}
```

### 响应数据-验签
1.响应参数按照ASCII码表的顺序进行排列
- <span style="color:red">注意：请使用程序对参数进行自动排列，不要人工对参数排序后写死在代码中，以防平台返回参数发生变更验签失败的情况发生</span>

2.所有参数值（除了sign）按照上面的排序用“|”连接起来，格式是：

`v1|v2`
-  <span style="color:red">注意：空值不参与验签，见举例2</span>

3.签名时将字符转化成字节流时应指定UTF-8的字符集。
- <span style="color:red">各响应参数的参数值的前后不允许出现空白符，否则将影响签名的正确性</span>

4.JAVA签名方式，详见1.3小节。

**举例1：**
收到的响应报文：
```json
{
	"sign" = "XhPuNulwDfXtfR8Gm03g+w9iJ9Exio7HveF23p77qGK2VT4aSi/X1e0QyixFkzEmZkmqPLjncjI/S40D+tx74/gHn9ExhWUt096k/JS6VLO5yFO/am2oJLL8AdhOuwrh/FqTkKZyaSyo0oDnUvqdmP0Z7dBAf6g2CQSndYpvgs8=",
	"retCode" = "0000",
	"Memo" = "退款成功"
}

```
接口定义的参数名顺序如下：

| 序号  |   字段|值（举例）|
|----|----|----|
| 1  |  Memo |  退款成功   |
| 2  |  retCode | 0000 |


按照asc排序

| 序号  |   字段|值（举例）|
|----|----|----|
| 1  |  retCode | 0000 |
| 2  |  Memo |  退款成功   |

那么待验签的数据就是：

`0000|退款成功`

使用公钥对待验签串进行验签，判断结果是否和
"sign" = "XhPuNulwDfXtf...QSndYpvgs8="的值一致


**举例2：**
收到的响应报文(包含空值)：
```json
{
	"sign" = "XhPuNulwDfXtfR8Gm03g+w9iJ9Exio7HveF23p77qGK2VT4aSi/X1e0QyixFkzEmZkmqPLjncjI/S40D+tx74/gHn9ExhWUt096k/JS6VLO5yFO/am2oJLL8AdhOuwrh/FqTkKZyaSyo0oDnUvqdmP0Z7dBAf6g2CQSndYpvgs8=",
	"retCode" = "0000",
	"amount" = "",
	"Memo" = "退款成功"
}

```
接口定义的参数名顺序如下：

| 序号  |   字段|值（举例）|
| ---- | ---- | ---- |
| 1  |  Memo |  退款成功   |
| 2  |  retCode | 0000 |
| 3  |  amount | "" <span style="color:red">（空值）</span> |


去掉空值，并按照asc排序

| 序号  |   字段|值（举例）|
| ---- | ---- | ---- |
| 1  |  retCode | 0000 |
| 2  |  Memo |  退款成功   |

那么待验签的数据就是：

`0000|退款成功`

使用公钥对待验签串进行验签，判断结果是否和
"sign" = "XhPuNulwDfXtf...QSndYpvgs8="的值一致

联动优势提供了java、PHP等编程语言的示例代码（[点击下载][点击下载]）下载密码（1xzx）


[点击下载]: https://pan.baidu.com/s/1nOhp6phwvNuoqF0WfRrBPQ "点击下载"