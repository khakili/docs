# 签名验证（java）
## 签名函数

java建议使用版本： signUtil.20170115.v2.1.jar

```java
public static String signAndRelativePath(String dataString, String charset, String keyPath) throws SignEncException
```

- 输入参数：
dataString：待签名字符串;charset为字符集；
keyPath：证书内容(通过此方法通过相对路径获取证书内容private static byte[] getFileContent(String filePath) throws SignEncException )

- 输出参数：
String：为签名串

- 抛出异常：
SignEncException：为Exception的子类，表示签名失败，调用方法需要捕获或抛出

- 备注： 如果证书在应用的根目录下要用   /证书名。jar可以反编译，有注释。jar里带源码。

## 签名包signUtilv2.1.jar使用说明

Jar包中包含三个文件：

		 com.umpay.SignEnc.class
		 com.umpay.SignEncException.class
		 com.umpay.SignVerProp.properties

其中com.umpay.SignEncException.class为签名验签的异常处理类，开发接口不需要关注。

com.umpay.SignEnc.class类为签名验签的处理类。

com.umpay.SignVerProp.properties为签名验签的配置文件，分别配置签名验签的商户私钥和平台公钥。mer.prikey.path属性配置为惠商+平台的私钥，目前配置为testMer.key.p8文件；plat.cert.path属性配置为惠商+平台的公钥，目前配置为testMer.cert.crt文件。两个配置均为绝对路径，需要商户根据实际情况修改路径。修改方法：把用rar工具把jar文件打开，把com.umpay.SignVerProp.properties文件拷贝出来，用文本工具修改并保持，最好通过rar工具用拖拽的方式把新的配置文件拽进jar文件中。