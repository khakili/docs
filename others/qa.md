# 4.1接入常见问题说明
1. 请求URL地址错误，每个接口都配备了两个URL地址，一个开发环境和一个生产环境，请注意识别请求地址是否正确（目前支持<span style="color:red">生产环境</span>测试）

2. 联调时，平台提供联调灰度环境，即由平台提供灰度环境匹配的商户证书，正式商用该证书无效，请使用商户自申请的私钥证书做签名（目前该过程还是人工受理，具体申请流程请联系相关业务人员）
```java
//获取加签结果，testMer.key.p8是联调商户证书，正式商用需要修改成商户自有私钥证书
String sign = SignEnc.sign(befSign, "UTF-8", "src/main/resources/crt/testMer.key.p8");
```
