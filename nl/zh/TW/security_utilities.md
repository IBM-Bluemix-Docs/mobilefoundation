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

#	安全公用程式
{: #security_utilities}

Mobile Foundation 用戶端 API 提供一些安全公用程式，以協助保護使用者的資料。如果您要保護 JSON 物件，優先推薦 JSONStore 這類特性。建議您不要在 JSONStore 集合中儲存二進位 Blob。

而是將二進位資料儲存在檔案系統上，並將檔案路徑和其他 meta 資料儲存在 JSONStore 集合內。如果您要保護影像這類檔案，可以將它們編碼為 base64 字串，並進行加密，然後將輸出寫入磁碟。若要解密資料，您可以查閱 JSONStore 集合中的 meta 資料。讀取磁碟中的已加密資料，然後使用所儲存的 meta 資料予以解密。此 meta 資料可以包括索引鍵、salt、起始設定向量 (IV)、檔案類型、檔案路徑及其他項目。

在高層次，SecurityUtils API 提供下列 API：

* 金鑰產生 - 此金鑰產生功能使用「密碼型金鑰衍生函數 2 (PBKDF2)」來產生加密 API 的高保護性 256 位元金鑰，而不是將密碼直接傳遞至加密功能。它採用參數作為反覆運算次數。數字越高，攻擊者強制入侵您的金鑰所需的時間就越多。請使用至少 10,000 的值。salt 必須是唯一的，如此可協助確保攻擊者難以使用現有的雜湊資訊來攻擊您的密碼。請使用 32 個位元組的長度。
* 加密 - 使用「進階加密標準 (AES)」來加密輸入。此 API 採用使用金鑰產生 API 所產生的金鑰。它會在內部產生安全 IV，以用來將隨機化新增至第一個區塊密碼。文字已加密。如果您要加密影像或其他二進位格式，則請使用這些 API 將二進位轉換成 base64 文字。此加密功能會傳回具有下列組件的物件：
    * ct（密碼文字，也稱為已加密文字）
    * IV
    * v（版本，容許 API 進化，同時仍與舊版相容）
* 解密 - 採用加密 API 的輸出作為輸入，並將密碼或已加密文字解密為純文字。
* 遠端隨機字串 - 聯絡 MobileFirst Server 上的隨機產生器，以取得隨機十六進位字串。預設值為 20 個位元組，但您最多可以將數目變更為 64 個位元組。
* 本端隨機字串 - 在本端產生隨機十六進位字串，以取得隨機十六進位字串，這與需要網路存取權的遠端隨機字串 API 不同。預設值為 32 個位元組，且沒有最大值。作業時間與位元組數目成比例。
* 編碼 base64 - 採用字串，並套用 base64 編碼。基於演算法本質的 base64 編碼表示資料大小會增加為原始大小的大約 1.37 倍。
* 解碼 base64 - 採用 base64 已編碼字串，並套用 base64 解碼。

## 設定
請確定您匯入下列檔案，以使用 JSONStore 安全公用程式 API。

### 設定 iOS

```objc
#import "WLSecurityUtils.h"
```

### 設定 Android

```java
import com.worklight.wlclient.api.SecurityUtils
```

### 設定 JavaScript
不需要任何設定。

## 適用於 iOS 的範例
### 在 iOS 中加密及解密

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

### 在 iOS 中編碼及解碼 base64

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

### 在 iOS 中取得遠端隨機

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

## 適用於 Android 的範例
### 在 Android 中加密及解密

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

### 在 Android 中編碼及解碼 base64

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

### 在 Android 中取得遠端隨機

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

### 在 Android 中取得本端隨機

```java
int byteLength = 32;
String randomString = SecurityUtils.getRandomString(byteLength);
```
{: codeblock}

## 適用於 JavaScript 的範例
### 在 JavaScript 中加密及解密

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

### 在 JavaScript 中編碼及解碼 base64

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

### 在 JavaScript 中取得遠端隨機

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

### 在 JavaScript 中取得本端隨機

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
