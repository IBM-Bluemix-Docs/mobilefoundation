---

copyright:
  years: 2018, 2019
lastupdated: "2019-06-06"

keywords: security

subcollection:  mobilefoundation
---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:tip: .tip}
{:pre: .pre}

#	安全实用程序
{: #security_utilities}

Mobile Foundation 客户机端 API 提供了一些安全实用程序来帮助保护用户的数据。如果要保护 JSON 对象，那么 JSONStore 等功能非常有用。我们建议不要在 JSONStore 集合中存储二进制 Blob。

应改为在文件系统上存储二进制数据，并将文件路径和其他元数据存储在 JSONStore 集合中。如果要保护图像等文件，可以将其编码为 Base64 字符串，对其加密，然后将输出写入磁盘。要对数据进行解密，可以在 JSONStore 集合中查找元数据。从磁盘读取加密的数据，然后使用存储的元数据对其进行解密。此元数据可包括密钥、加密盐 (Salt)、初始化向量 (IV)、文件类型、文件路径等。

在较高级别，SecurityUtils API 提供了以下 API：

* 密钥生成 - 此密钥生成函数使用基于密码的密钥派生函数 V2 (PBKDF2) 为加密 API 生成高强度 256 位密钥，而不是将密码直接传递给加密函数。它接受一个表示迭代次数的参数。此数字越大，攻击者暴力破解密钥所需时间越长。请使用不低于 10,000 的值。加密盐 (Salt) 必须唯一，确保攻击者难以使用现有散列信息来攻击您的密码。请使用 32 字节的长度。
* 加密 - 使用高级加密标准 (AES) 来对输入加密。此 API 接受通过密钥生成 API 所生成的密钥。它在内部生成一个安全 IV，用于为第一个块密码添加随机性。文本会进行加密。如果要加密图像或其他二进制格式，请使用这些 API 将二进制转换为 Base64 文本。此加密函数会返回具有以下组成部分的对象：
    * ct（密文，也称为加密文本）
    * IV
    * v（版本，允许 API 进行演化，同时仍与先前版本兼容）
* 解密 - 接受加密 API 中的输出作为输入，并将密码或加密文本解密为明文。
* 远程随机字符串 - 通过在 MobileFirst 服务器上与随机生成器联系来获取随机十六进制字符串。缺省值为 20 字节，但可以将此数字改为最高 64 字节。
* 本地随机字符串 - 通过本地生成来获取随机十六进制字符串，不同于需要网络访问的远程随机字符串 API。缺省值为 32 字节，没有最大值。运行时间与字节数成正比。
* 编码 Base64 - 接受字符串并应用 Base64 编码。采用 Base64 编码就其算法本质而言，意味着数据大小会增加至原始大小的约 1.37 倍。
* 解码 Base64 - 接受 Base64 编码的字符串，并应用 Base64 解码。

## 设置
确保导入以下文件以使用 JSONStore 安全实用程序 API。

### 设置 iOS

```objc
#import "WLSecurityUtils.h"
```

### 设置 Android

```java
import com.worklight.wlclient.api.SecurityUtils
```

### 设置 JavaScript
无需任何设置。

## iOS 的示例
### iOS 中的加密和解密

```objc
// User provided password, hardcoded only for simplicity.
NSString* password = @"HelloPassword";

// Random salt with recommended length.
NSString* salt = [WLSecurityUtils generateRandomStringWithBytes:32];

// Recomended number of iterations.
int iterations = 10000;

// Populated with an error if one occurs.
NSError* error = nil;

// Call that generates the key.
NSString* key = [WLSecurityUtils generateKeyWithPassword:password
                                 andSalt:salt
                                 andIterations:iterations
                                 error:&error];

// Text that is encrypted.
NSString* originalString = @"My secret text";
NSDictionary* dict = [WLSecurityUtils encryptText:originalString
                                      withKey:key
                                      error:&error];

// Should return: 'My secret text'.
NSString* decryptedString = [WLSecurityUtils decryptWithKey:key
                                             andDictionary:dict
                                             error:&error];
```
{: codeblock}

### 在 iOS 中编码和解码 Base64

```objc
// Input string.
NSString* originalString = @"Hello world!";

// Encode to base64.
NSData* originalStringData = [originalString dataUsingEncoding:NSUTF8StringEncoding];
NSString* encodedString = [WLSecurityUtils base64StringFromData:originalStringData length:originalString.length];

// Should return: 'Hello world!'.
NSString* decodedString = [[NSString alloc] initWithData:[WLSecurityUtils base64DataFromString:encodedString] encoding:NSUTF8StringEncoding];
```
{: codeblock}

### 在 iOS 中获取远程随机字符串

```objc
[WLSecurityUtils getRandomStringFromServerWithBytes:32
                 timeout:1000
                 completionHandler:^(NSURLResponse *response, NSData *data, NSError *connectionError) {

  // You might want to see the response and the connection error before moving forward.

  // Get the secure random string.
  NSString* secureRandom = [[NSString alloc] initWithData:data encoding:NSUTF8StringEncoding];
}];
```
{: codeblock}

## Android 的示例
### Android 中的加密和解密

```java
String password = "HelloPassword";
String salt = SecurityUtils.getRandomString(32);
int iterations = 10000;

String key = SecurityUtils.generateKey(password, salt, iterations);

String originalText = "Hello World!";

JSONObject encryptedObject = SecurityUtils.encrypt(key, originalText);

// Deciphered text will be the same as the original text.
String decipheredText = SecurityUtils.decrypt(key, encryptedObject);
```
{: codeblock}

### 在 Android 中编码和解码 Base64

```java
import android.util.Base64;

String originalText = "Hello World";
byte[] base64Encoded = Base64.encode(text.getBytes("UTF-8"), Base64.DEFAULT);

String encodedText = new String(base64Encoded, "UTF-8");

byte[] base64Decoded = Base64.decode(text.getBytes("UTF-8"), Base64.DEFAULT);

// Decoded text will be the same as the original text.
String decodedText = new String(base64Decoded, "UTF-8");
```
{: codeblock}

### 在 Android 中获取远程随机字符串

```java
Context context; // This is the current Activity's context.
int byteLength = 32;

// Listener calls the callback functions after it gets the response from the server.
WLRequestListener listener = new WLRequestListener(){
  @Override
  public void onSuccess(WLResponse wlResponse) {
    // Implement the success handler.
  }

  @Override
                    public void onFailure(WLFailResponse wlFailResponse) {
                        // Implement the failure handler.
    }
};

SecurityUtils.getRandomStringFromServer(byteLength, context, listener);
```
{: codeblock}

### 在 Android 中获取本地随机字符串

```java
int byteLength = 32;
String randomString = SecurityUtils.getRandomString(byteLength);
```
{: codeblock}

## JavaScript 的示例
### JavaScript 中的加密和解密

```javascript
// Keep the key in a variable so that it can be passed to the encrypt and decrypt API.
var key;

// Generate a key.
WL.SecurityUtils.keygen({
  password: 'HelloPassword',
  salt: Math.random().toString(),
  iterations: 10000
})

.then(function (res) {

  // Update the key variable.
  key = res;

  // Encrypt text.
  return WL.SecurityUtils.encrypt({
    key: key,
    text: 'My secret text'
  });
})

.then(function (res) {

  // Append the key to the result object from encrypt.
  res.key = key;

  // Decrypt.
  return WL.SecurityUtils.decrypt(res);
})

.then(function (res) {

  // Remove the key from memory.
  key = null;

  //res => 'My secret text'
})

.fail(function (err) {
  // Handle failure in any of the previously called APIs.
});
```
{: codeblock}

### 在 JavaScript 中编码和解码 Base64

```javascript
WL.SecurityUtils.base64Encode('Hello World!')
.then(function (res) {
  return WL.SecurityUtils.base64Decode(res);
})
.then(function (res) {
  //res => 'Hello World!'
})
.fail(function (err) {
  // Handle failure.
});
```
{: codeblock}

### 在 JavaScript 中获取远程随机字符串

```javascript
WL.SecurityUtils.remoteRandomString(32)
.then(function (res) {
  // res => deba58e9601d24380dce7dda85534c43f0b52c342ceb860390e15a638baecc7b
})
.fail(function (err) {
  // Handle failure.
});
```
{: codeblock}

### 在 JavaScript 中获取本地随机字符串

```javascript
WL.SecurityUtils.localRandomString(32)
.then(function (res) {
  // res => 40617812588cf3ddc1d1ad0320a907a7b62ec0abee0cc8c0dc2de0e24392843c
})
.fail(function (err) {
  // Handle failure.
});
```
{: codeblock}
