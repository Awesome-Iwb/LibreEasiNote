# QrCode Login

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
http://127.0.0.1:28501/scan/qrcode?oriSys=EasiNote5&type=text&random=0.35865064997742113
```

实际上是把请求给转发到了 http://id.seewo.com。这个域名也用于 EasiAgent 的单点登录。反正这个域名就是跟 seewo 账号的认证有关。

### URL 请求参数：

|parameter|type|description|default|
|-|-|-|-|
| `oriSys` | `string` | 指示登录客户端类型 | `EasiNote5` |
| `type` | `string` | | `text` |
| `random` | `number` | 传入随机数，0-1之间的浮点 | `0.1145141919810` |

### 响应体：

```json
{
    "data": "https://id.seewo.com/scan/middle?uid=barcode_[GUID]&oriSys=EasiNote5",
    "qrkey": "barcode_[GUID]"
}
```

|parameter|type|description|default|
|-|-|-|-|
| `data` | `string` | qrcode的数据 ||
| `qrkey` | `string` || `barcode_[GUID]` |

## 2. 判断qrCode是否有效

```
http://127.0.0.1:28501/scan/pcCheckQrcode?type=long&t=1734707528106&system=en5web&withCredentials=true&random=0.3705031403250001
```

### URL 请求参数：

|parameter|type|description|default|
|-|-|-|-|
| `oriSys` | `string` | 指示登录客户端类型 | `EasiNote5` |
| `system` | `string` | 指示类型，这里由于MacOS版本使用的是electron，所以直接当EasiNote网页端对待 | `en5web` |
| `type` | `string` || `long` |
| `withCredentials` | `boolean` | 这里和cookie有关 | `true` |
| `random` | `number` | 传入随机数，0-1之间的浮点 | `0.1145141919810` |
| `t` | `number` | 传入现在的时间戳，毫秒 | `1734708453615` |

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