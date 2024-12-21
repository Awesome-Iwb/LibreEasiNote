# Login

## 0. 行为

```javascript
config.qrCodeUrl = 'http://id.seewo.com';

if (qrCodeApi.includes(req.path)) {
    return proxy(config_1.default.qrCodeUrl + req.originalUrl)(req, res, next);
}
```

1. 请求qrcode数据来输出qrcode来让用户扫码登录
2. 轮询判断qrcode是否有效
3. ...

## 1. 请求qrcode数据

```
GET https://id.seewo.com/scan/qrcode?oriSys=EasiNote5&t=1484722930223.3638 HTTP/1.1
```

实际上是把请求给转发到了 [http://id.seewo.com](http://id.seewo.com)。这个域名也用于 EasiAgent 的单点登录。反正这个域名就是跟 seewo 账号的认证有关。

### URL 请求参数：

| parameter | type     | description    | default           |
| --------- | -------- | -------------- | ----------------- |
| `oriSys`  | `string` | 指示登录客户端类型      | `EasiNote5`       |
| `type`    | `string` |                | `text`            |
| `random`  | `number` | 传入随机数，0-1之间的浮点 | `0.1145141919810` |

### 响应体：

```json
{
    "data": "https://id.seewo.com/scan/middle?uid=barcode_[GUID]&oriSys=EasiNote5",
    "qrkey": "barcode_[GUID]"
}
```

| parameter | type     | description | default          |
| --------- | -------- | ----------- | ---------------- |
| `data`    | `string` | qrcode的数据   |                  |
| `qrkey`   | `string` |             | `barcode_[GUID]` |

## 2. 判断qrCode是否有效

```
GET https://id.seewo.com/scan/pcCheckQrcode HTTP/1.1
```

### URL 请求参数：

| parameter         | type      | description                                      | default           |
| ----------------- | --------- | ------------------------------------------------ | ----------------- |
| `oriSys`          | `string`  | 指示登录客户端类型                                        | `"EasiNote5"`     |
| `system`          | `string`  | 指示类型，这里由于MacOS版本使用的是electron，所以直接当EasiNote网页端对待。 | `"en5web"`        |
| `type`            | `string`  |                                                  | `long`            |
| `withCredentials` | `boolean` | 这里和cookie有关                                      | `true`            |
| `random`          | `number`  | 传入随机数，0-1之间的浮点                                   | `0.1145141919810` |
| `t`               | `number`  | 传入现在的时间戳，毫秒                                      | `1734708453615`   |

此处使用 Windows 端 EasiNote 时，发现这个请求没有任何 URL params. 估计是把这些信息给写到请求体的 Cookie 里面了，有 `x-auth-app=EasiNote5` 这个东西。等待继续挖掘。

### Cookie

|name|type|description|default|
|-|-|-|-|
| `qrkey` | `string` | 当前的qrKey Cookie ||

### 响应体：

```json
{
    "statusCode": 200,
    "message": "二维码仍有效"
}
```

|parameter|type|description|default|
|-|-|-|-|
| `statusCode` | `number` || `200` |
| `message` | `string` |||

## 3. 账号密码登录

```
POST https://edu.seewo.com/api/v1/auth/login HTTP/1.1
```

### POST 数据：

```json
{
    "username": "1145141919810",
    "password": "5b91145d421bf243239ddsdfd82913ad2e",
    "captcha": null,
    "phoneCountryCode": ""
}
```

| parameter          | type     | description                            | default |
| ------------------ | -------- | -------------------------------------- | ------- |
| `username`         | `string` | 这里传入字符串类型的手机号，注意只支持 +86 手机号登录希沃白板。     |         |
| `password`         | `string` | 密码使用MD5摘要算法处理。                         |         |
| `captcha`          | `?`      | 目前还不知道这个captcha使用在哪里，有大佬知道的可以给我说一下，谢谢。 | `null`  |
| `phoneCountryCode` | `string` | 手机号国家码，目前不用传入。                         | `""`    |

目前真的不知道这个 captcha 会在什么情况下出现，即使是开了代理也不会出现人机验证。手机号国家码，目前不用传入（因为 EasiNote 只支持国内手机号）。

### 响应体：

```json
{
    "data": {
        "isRegister": 0,
        "token": "d68b3f5f0bde4e869794bba39966f27e-0446",
        "user": {}
    },
    "error_code": 0,
    "message": "ok"
}
```

| parameter         | type          | description                               | default |
| ----------------- | ------------- | ----------------------------------------- | ------- |
| `data.isRegister` | `number`      | 一般都是0（希沃白板客户端无法直接注册，除非是网页端，网页端的情况等待完善...） | `0`     |
| `data.token`      | `string`      |                                           |         |
| `data.user`       | `[SeewoUser]` | 给的是当前登录上的这个用户的信息。详细请查看[这里](./user.md)     |         |
| `data.error_code` | `number`      |                                           | `0`     |
| `data.message`    | `string       |                                           | `"ok"`  |

## 4. 获取登录日志

```
POST https://edu.seewo.com/api/v1/user/loginLog HTTP/1.1
```

POST 数据都是 JSON 格式的。如果没有写 POST 数据的话，默认就是一个空的 JSON 对象。

### 响应体：

```json
{
	"data":{
		"loginTimes":81
	},
	"error_code":0,
	"message":"ok"
}
```

返回的 `loginTimes` 就是累计登录的次数。其实 data 里面还可能会返回一个 `guide` 对象，不过目前我没有遇到，就先把逆向出来的 EasiNote 的代码给 po 出来吧：

```csharp
namespace Cvte.EasiNote
{
	public class GuideInfo
	{
		[DataMember(Name = "id")]
		public int Id { get; set; }

		[DataMember(Name = "name")]
		public string Name { get; set; }

		[DataMember(Name = "type")]
		public int Type { get; set; }

		[DataMember(Name = "url")]
		public string Url { get; set; }
	}
}
```