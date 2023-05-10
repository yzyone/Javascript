# [CryptoJS-中文文档](https://www.cnblogs.com/huiguo/p/16601076.html)

原始文档：https://code.google.com/archive/p/crypto-js/

## 介绍

CryptoJS是一个JavaScript的加解密的工具包。它支持多种的算法：`MD5、SHA1、SHA2、SHA3、RIPEMD-160` 哈希散列，进行 `AES、DES、Rabbit、RC4、Triple DES` 加解密。

## 散列算法

### MD5

`MD5`是一种广泛使用的散列函数。它被用于各种安全应用，也通常用于校验文件的完整性。但`MD5`不耐碰撞攻击，因此不适用于`SSL`证书或数字签名。

```bash
var hash = CryptoJS.MD5("Message");
```

### SHA-1

`SHA` 散列函数由美国国家安全局 (NSA) 设计。`SHA-1` 是现有 `SHA` 散列函数中最成熟的，它用于各种安全应用程序和协议。但随着新攻击的发现或改进，`SHA-1` 的抗攻击能力一直在减弱。

```bash
var hash = CryptoJS.SHA1("Message");
```

### SHA-2

`SHA-224、SHA-256、SHA-384`，和`SHA-512`合称为`SHA-2`。
`SHA-256`是`SHA-2`集合中的四个变体之一。虽然它提供了更好的安全性，但是它的应用不如SHA-1广泛。

```bash
var hash = CryptoJS.SHA256("Message");
```

`SHA-512`在很大程度上与`SHA-256`相同，但在64位计算机上`SHA-512`比`SHA-256`更快(因为它们在内部使用64位算术)；在8位，16位和32位计算机上，`SHA-256`比`SHA-512`更快。

```bash
var hash = CryptoJS.SHA512("Message");
```

`CryptoJS`还支持`SHA-224`和`SHA-384`，这两个版本大致相同，分别是`SHA-256`和`SHA-512`的删减版本。

### SHA-3

`SHA-3`是第三代安全散列算法(Secure Hash Algorithm 3)

```bash
var hash = CryptoJS.SHA3("Message");
```

`SHA-3`可以配置输出散列长度为224，256，384或512位，默认为512位。

```bash
var hash = CryptoJS.SHA3("Message", { outputLength: 512 });
var hash = CryptoJS.SHA3("Message", { outputLength: 384 });
var hash = CryptoJS.SHA3("Message", { outputLength: 256 });
var hash = CryptoJS.SHA3("Message", { outputLength: 224 });
```

### RIPEMD-160

```bash
var hash = CryptoJS.RIPEMD160("Message");
```

## 散列输入

散列算法接受输入字符串或`WordArray`实例。`WordArray`对象表示一个32位“单词数组”。当你传入一个字符串时，它会自动转换为编码为`UTF-8`的`WordArray`。

## 散列输出

返回的散列不是字符串，它是一个`WordArray`对象。当您在字符串上下文中使用`WordArray`对象时，它会自动转换为十六进制字符串。

```bash
var hash = CryptoJS.SHA256("Message");

typeof hash
> "object";

hash
> "2f77668a9dfbf8d5848b9eeb4a7145ca94c6ed9236e4a773f6dcafa5132b2f91";
```

使用`toString`方法并传入编码格式，可以将`WordArray`对象转换为其他编码格式

```python
var hash = CryptoJS.SHA256("Message")

hash.toString(CryptoJS.enc.Base64)
> "L3dmip37+NWEi57rSnFFypTG7ZI25Kdz9tyvpRMrL5E=";

hash.toString(CryptoJS.enc.Hex)
> "2f77668a9dfbf8d5848b9eeb4a7145ca94c6ed9236e4a773f6dcafa5132b2f91";
```

### 渐进式散列

```csharp
var sha256 = CryptoJS.algo.SHA256.create();
sha256.update("Message Part 1");
sha256.update("Message Part 2");
sha256.update("Message Part 3");

var hash = sha256.finalize();
```

### HMAC

`HMAC`是一种使用加密散列函数进行消息认证的机制，可以与任何迭代密码散列函数结合使用。

```bash
var hash = CryptoJS.HmacMD5("Message", "Secret Passphrase");
var hash = CryptoJS.HmacSHA1("Message", "Secret Passphrase");
var hash = CryptoJS.HmacSHA256("Message", "Secret Passphrase");
var hash = CryptoJS.HmacSHA512("Message", "Secret Passphrase");
```

### 渐进式HMAC散列

```csharp
var hmac = CryptoJS.algo.HMAC.create(CryptoJS.algo.SHA256, "Secret Passphrase");
hmac.update("Message Part 1");
hmac.update("Message Part 2");
hmac.update("Message Part 3");

var hash = hmac.finalize();
```

### PBKDF2

`PBKDF2`是一个用来对用户口令(password)进行加密的函数。在密码学的许多应用中，用户安全性最终取决于用户口令，由于用户口令通常不能直接用作密钥，因此需要进行一些处理。

它的基本原理是通过一个伪随机函数(例如`HMAC`函数),把明文和一个盐值作为输入参数,，生成一个散列值，然后将这个散列值作为一个加密key，应用到后续的加密过程中，以此类推，将这个过程重复很多次，从而增加了密码破解的难度，这个过程也被称为是密码加强。如果重复的次数足够大,破解的成本就会变得很高。

- `Salt` 表示盐值，一个随机数
- `iterations` 操作次数

```csharp
var salt = CryptoJS.lib.WordArray.random(128 / 8);
var key128Bits = CryptoJS.PBKDF2("Secret Passphrase", salt, {
  keySize: 128 / 32
});
var key256Bits = CryptoJS.PBKDF2("Secret Passphrase", salt, {
  keySize: 256 / 32
});
var key512Bits = CryptoJS.PBKDF2("Secret Passphrase", salt, {
  keySize: 512 / 32
});
var key512Bits1000Iterations = CryptoJS.PBKDF2("Secret Passphrase", salt, {
  keySize: 512 / 32,
  iterations: 1000
});
```

## 加密

### 加密算法

加密函数的参数是：(明文字符串, 密钥字符串，可选参数对象)，返回密文字符串。
加密函数是：`Cryptojs.AES.encrypt`，`Cryptojs.DES.encrypt``，Cryptojs.Rabbit.encrypt`，`Cryptojs.RC4.encrypt`，`Cryptojs.TripleDES.encrypt`

解密函数的参数是：(密文字符串, 密钥字符串，可选参数对象)，返回的结果必须用`.toString(CryptoJS.enc.Utf8)`转为明文。
解密函数是:`CryptoJS.AES.decrypt`，`CryptoJS.DES.decrypt`，`CryptoJS.Rabbit.decrypt`，`CryptoJS.RC4.decrypt`，`CryptoJS.TripleDES.decrypt`

其中可选参数对象常用属性：

- `mode`：加密模式 【CBC ECB CFB OFB CTRGladman(CTR)】
- `paddig`：填充方式 【 NoPadding ZeroPadding Pkcs7(Pkcs5) Iso10126 Iso97971 AnsiX923】
- `vi`： 偏移向量
- `formatter`：自定义格式

### AES

`AES`密码学中的高级加密标准（Advanced Encryption Standard，AES），又称Rijndael加密法，是美国联邦政府采用的一种区块加密标准。这个标准用来替代原先的DES（Data Encryption Standard），已经被多方分析且广为全世界所使用。

```csharp
var encrypted = CryptoJS.AES.encrypt("Message", "Secret Passphrase");

var decrypted = CryptoJS.AES.decrypt(encrypted, "Secret Passphrase");
```

`CryptoJS`支持`AES-128、AES-192`和`AES-256`。密钥的长度决定了`AES加密`的轮数。

### DES,三重DES

`DES`是以前比较重要的加密算法，但由于密钥长度太短，安全性不够。

```csharp
var encrypted = CryptoJS.DES.encrypt("Message", "Secret Passphrase");

var decrypted = CryptoJS.DES.decrypt(encrypted, "Secret Passphrase");
```

`三重DES`是为了增加DES的强度，将DES重复3次所得到的一种密码算法，具有足够的安全性。

```csharp
var encrypted = CryptoJS.TripleDES.encrypt("Message", "Secret Passphrase");

var decrypted = CryptoJS.TripleDES.decrypt(encrypted, "Secret Passphrase");
```

### Rabbit

`Rabbit` 是一种高速流密码，于 2003 年在 FSE 研讨会上首次提出。 `Rabbit` 使用一个 128 位密钥和一个 64 位初始化向量。 该加密算法的核心组件是一个位流生成器，该流生成器每次迭代都会加密 128 个消息位。

```csharp
var encrypted = CryptoJS.Rabbit.encrypt("Message", "Secret Passphrase");

var decrypted = CryptoJS.Rabbit.decrypt(encrypted, "Secret Passphrase");
```

### RC4, RC4Drop

`RC4`算法是`Ron Rivest`为`RSA公司`在1987年设计的一种流密码，作为`RSA`的商业机密直到1994年才被匿名公布于`Internet`。
`RC4`被用于为网络浏览器和服务器间通信而制定的`SSL/TLS`（安全套接字协议/传输层安全协议）标准中，以及作为IEEE 801.11无线局域网标准一部分的`WEP`(Wired Equivalent Privacy)协议和新的WiFi受保护访问协议(WAP)中。从这些应用来看，`RC4`构成了当今网络通信的非常重要的部分，因此这个算法非常重要。

```csharp
var encrypted = CryptoJS.RC4.encrypt("Message", "Secret Passphrase");

var decrypted = CryptoJS.RC4.decrypt(encrypted, "Secret Passphrase");
```

后来研究者发现，密钥流初始的若干字节有偏移问题，泄露了密钥信息，被很多用户弃用。当然，我们可以通过丢弃密钥流的初始若干字节来防御这种攻击，这种改进的算法也被称为`RC4-drop`。
`RC4-drop`可以用`drop`属性设置丢弃字节值。

```csharp
var encrypted = CryptoJS.RC4Drop.encrypt("Message", "Secret Passphrase");

var encrypted = CryptoJS.RC4Drop.encrypt("Message", "Secret Passphrase", {
  drop: 3072 / 4
});

var decrypted = CryptoJS.RC4Drop.decrypt(encrypted, "Secret Passphrase", {
  drop: 3072 / 4
});
```

### 设置密钥（key）和偏移量（iv）

```csharp
var key = CryptoJS.enc.Hex.parse("000102030405060708090a0b0c0d0e0f");

var iv = CryptoJS.enc.Hex.parse("101112131415161718191a1b1c1d1e1f");

var encrypted = CryptoJS.AES.encrypt("Message", key, { iv: iv });
```

### 加密模式和填充方式

```swift
var encrypted = CryptoJS.AES.encrypt("Message", "Secret Passphrase", {
  mode: CryptoJS.mode.CFB,
  padding: CryptoJS.pad.AnsiX923
});
```

`CryptoJS`支持以下加密模式：

- CBC (the default)
- CFB
- CTR
- OFB
- ECB

`CryptoJS`支持以下填充方式：

- Pkcs7 (the default)
- Iso97971
- AnsiX923
- Iso10126
- ZeroPadding
- NoPadding

### 加密输入

对于明文消息，加密算法接受输入字符串或`CryptoJS.lib.WordArray`实例。

对于密钥key，当您输入一个字符串时，它将用于生成密钥和IV。您可以输入实际密钥的`WordArray`对象和实际的`IV`。

对于密文，密码算法接受输入字符串或`CryptoJS.lib.CipherParams`的实例`。CipherParams`对象表示一组参数，如`IV、salt`和原始密文本身。当您输入字符串时，它会根据可配置的格式策略自动转换为`CipherParams`对象。

### 加密输出

解密后得到的明文是一个`WordArray`对象。详见散列的输出。

加密后得到的密文不是字符串，它是一个`CipherParams`对象。通过`CipherParams`对象，您可以拿到加密期间使用的所有参数。当您在字符串上下文中使用`CipherParams`对象时，它会根据格式策略自动转换为字符串。默认是`openssl`兼容的格式。

```shell
var encrypted = CryptoJS.AES.encrypt("Message", "Secret Passphrase");

encrypted.key
> "74eb593087a982e2a6f5dded54ecd96d1fd0f3d44a58728cdcd40c55227522223 ";

encrypted.iv
> "7781157e2629b094f0e3dd48c4d786115";

encrypted.salt
> "7a25f9132ec6a8b34";

encrypted.ciphertext
> "73e54154a15d1beeb509d9e12f1e462a0";

encrypted
> "U2FsdGVkX1+iX5Ey7GqLND5UFUoV0b7rUJ2eEvHkYqA=";
```

您可以定义自己的格式，以便与其他加密实现兼容。`format`有两个对象方法`stringify`和`parse`，在`CipherParams`对象和密文字符串之间进行转换。
下面是编写`JSON`格式化的方法:

```javascript
var JsonFormatter = {
  stringify: function(cipherParams) {
    // 创建ciphertext对象
    var jsonObj = { ct: cipherParams.ciphertext.toString(CryptoJS.enc.Base64) };

    //可选添加 iv 和 salt
    if (cipherParams.iv) {
      jsonObj.iv = cipherParams.iv.toString();
    }

    if (cipherParams.salt) {
      jsonObj.s = cipherParams.salt.toString();
    }

    // 转换成json字符串
    return JSON.stringify(jsonObj);
  },
  parse: function(jsonStr) {
    // 转换成json格式
    var jsonObj = JSON.parse(jsonStr);

    // extract ciphertext from json object, and create cipher params object
    var cipherParams = CryptoJS.lib.CipherParams.create({
      ciphertext: CryptoJS.enc.Base64.parse(jsonObj.ct)
    });

    // 可选提取 iv 和 salt

    if (jsonObj.iv) {
      cipherParams.iv = CryptoJS.enc.Hex.parse(jsonObj.iv);
    }

    if (jsonObj.s) {
      cipherParams.salt = CryptoJS.enc.Hex.parse(jsonObj.s);
    }

    return cipherParams;
  }
};

var encrypted = CryptoJS.AES.encrypt("Message", "Secret Passphrase", {
  format: JsonFormatter
});

encrypted
> {
    ct: "tZ4MsEnfbcDOwqau68aOrQ==",
    iv: "8a8c8fd8fe33743d3638737ea4a00698",
    s: "ba06373c8f57179c"
  };

var decrypted = CryptoJS.AES.decrypt(encrypted, "Secret Passphrase", {
  format: JsonFormatter
});

decrypted.toString(CryptoJS.enc.Utf8)
> "Message";
```

### 渐进式加密

```java
var key = CryptoJS.enc.Hex.parse("000102030405060708090a0b0c0d0e0f");
var iv = CryptoJS.enc.Hex.parse("101112131415161718191a1b1c1d1e1f");

// encrypt加密
var aesEncryptor = CryptoJS.algo.AES.createEncryptor(key, { iv: iv });

var ciphertextPart1 = aesEncryptor.process("Message Part 1");
var ciphertextPart2 = aesEncryptor.process("Message Part 2");
var ciphertextPart3 = aesEncryptor.process("Message Part 3");
var ciphertextPart4 = aesEncryptor.finalize();

// decrypt解密
var aesDecryptor = CryptoJS.algo.AES.createDecryptor(key, { iv: iv });

var plaintextPart1 = aesDecryptor.process(ciphertextPart1);
var plaintextPart2 = aesDecryptor.process(ciphertextPart2);
var plaintextPart3 = aesDecryptor.process(ciphertextPart3);
var plaintextPart4 = aesDecryptor.process(ciphertextPart4);
var plaintextPart5 = aesDecryptor.finalize();
```

### 可行性操作

使用OpenSSL加密拿到openSSLEncrypted：

```python
openssl enc -aes-256-cbc -in infile -out outfile -pass pass:"Secret Passphrase" -e -base64
```

使用CryptoJS解密：

```csharp
var decrypted = CryptoJS.AES.decrypt(openSSLEncrypted, "Secret Passphrase");
```

### 编码器

CryptoJS可以将Base64、Latin1或Hex等编码格式转换为WordArray对象，反之亦然。

```java
// Base64字符串 > WordArray对象
var words = CryptoJS.enc.Base64.parse("SGVsbG8sIFdvcmxkIQ==");

// WordArray对象 > Base64字符串
var base64 = CryptoJS.enc.Base64.stringify(words);

// Latin1字符串 > WordArray对象
var words = CryptoJS.enc.Latin1.parse("Hello, World!");

// WordArray对象 > Latin1
var latin1 = CryptoJS.enc.Latin1.stringify(words);

// 16进制 > WordArray对象
var words = CryptoJS.enc.Hex.parse("48656c6c6f2c20576f726c6421");

// WordArray对象 > 16进制
var hex = CryptoJS.enc.Hex.stringify(words);

// utf8 > WordArray对象
var words = CryptoJS.enc.Utf8.parse("𔭢");

// WordArray对象 > utf8
var utf8 = CryptoJS.enc.Utf8.stringify(words);

// utf16 > WordArray对象
var words = CryptoJS.enc.Utf16.parse("Hello, World!");

// WordArray对象 > utf16
var utf16 = CryptoJS.enc.Utf16.stringify(words);

// utf16le > WordArray对象
var words = CryptoJS.enc.Utf16LE.parse("Hello, World!");

// WordArray对象 > utf16le
var utf16 = CryptoJS.enc.Utf16LE.stringify(words);
```

## 使用方法

### 1、安装

```x86asm
npm install crypto-js --save-dev
yarn add crypto-js --dev
// 或者下载js文件https://cdnjs.cloudflare.com/ajax/libs/crypto-js/3.1.9-1/crypto-js.js
```

### 2、在node中使用

**ES6方式：**

```javascript
import sha256 from 'crypto-js/sha256';
import hmacSHA512 from 'crypto-js/hmac-sha512';
import Base64 from 'crypto-js/enc-base64';
const message, nonce, path, privateKey; 
const hashDigest = sha256(nonce + message);
const hmacDigest = Base64.stringify(hmacSHA512(path + hashDigest, privateKey));
```

**非ES6方式：**

```javascript
var AES = require("crypto-js/aes");
var SHA256 = require("crypto-js/sha256");
...
console.log(SHA256("Message"));
```

可以引入整个`CryptoJS`，这样可以使用所有加解密的方法

```javascript
var CryptoJS = require("crypto-js");
console.log(CryptoJS.HmacSHA1("Message", "Key"));
```

### 3、在浏览器客户端使用

**RequireJS方式：**
按需导入CryptoJS子模块方法

```lua
require.config({
    packages: [
        {
            name: 'crypto-js',
            location: 'path-to/bower_components/crypto-js',
            main: 'index'
        }
    ]
});
require(["crypto-js/aes", "crypto-js/sha256"], function (AES, SHA256) {
    console.log(SHA256("Message"));
});
```

或者导入整个`CryptoJS`模块，这样可以使用所有加解密的方法

```lua
require.config({
    paths: {
        'crypto-js': 'path-to/bower_components/crypto-js/crypto-js'
    }
});
require(["crypto-js"], function (CryptoJS) {
    console.log(CryptoJS.HmacSHA1("Message", "Key"));
});
```

**非RequireJS方式：**

```xml
<script type="text/javascript" src="path-to/bower_components/crypto-js/crypto-js.js"></script>
<script type="text/javascript">
    var encrypted = CryptoJS.AES(...);
    var encrypted = CryptoJS.SHA256(...);
</script>
```

### 4、AES加密例子

**纯文本加密**

```javascript
var CryptoJS = require("crypto-js");
// Encrypt
var ciphertext = CryptoJS.AES.encrypt('my message', 'secret key 123').toString();
// Decrypt
var bytes  = CryptoJS.AES.decrypt(ciphertext, 'secret key 123');
var originalText = bytes.toString(CryptoJS.enc.Utf8);
console.log(originalText); // 输出'my message'
```

**对象加密**

```javascript
var CryptoJS = require("crypto-js");
var data = [{id: 1}, {id: 2}]
// Encrypt
var ciphertext = CryptoJS.AES.encrypt(JSON.stringify(data), 'secret key 123').toString();
// Decrypt
var bytes  = CryptoJS.AES.decrypt(ciphertext, 'secret key 123');
var decryptedData = JSON.parse(bytes.toString(CryptoJS.enc.Utf8));
console.log(decryptedData); //输出 [{id: 1}, {id: 2}]
```

分类: [前端库](https://www.cnblogs.com/huiguo/category/2205499.html)