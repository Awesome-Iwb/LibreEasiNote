# User

## 0. -0 用户信息结构体

类型： `[SeewoUser]`

```json
{
    "user": {
        "accountId": "a4f776e163b24545b67fa7123af54d28",
        "accountType": 1,
        "address": "",
        "appCode": "EasiNoteIOS",
        "cityId": "500100",
        "createTime": 1640405145000,
        "dingdingUid": "",
        "email": "",
        "gender": 0,
        "joinUnitTime": 1640405145000,
        "nickName": "dubi906w",
        "phone": "1145141919810",
        "photoUrl": "https://cstore-en-public-tx.seewo.com/easinote5_public/dont_shijian_me",
        "provinceId": "500000",
        "realName": "",
        "riskLevel": 0.0,
        "uid": "zgjtpmjsgoxqjomkunkunjinitaimei114514",
        "unitId": "001",
        "username": "1145141919810",
        "version": 1701328816560,
        "wechatUid": ""
    }
}
```

| parameter           | type     | description                                                                      | default            |
| ------------------- | -------- | -------------------------------------------------------------------------------- | ------------------ |
| `user.accountId`    | `string` |                                                                                  |                    |
| `user.accountType`  | `number` | 来自 EasiNote 电脑端的逆向：0为未知，1为个人用户，2为组织用户，3为都是。                                      | `1`                |
| `user.address`      | `string` | 未知                                                                               | `""`               |
| `user.appCode`      | `string` | 未知，一般会返回“EasiNoteIOS”                                                            | `"EasiNoteIOS"`    |
| `user.cityId`       | `string` | 地区代码，此处请参考：[县及县以上行政区划代码](https://www.gov.cn/test/2011-08/22/content_1930111.htm) | `"500100"` - 重庆市辖区 |
| `user.createTime`   | `number` | 账户创建时间                                                                           |                    |
| `user.dingdingUid`  | `string` | 如果你绑定了钉钉账户，则这里会有钉钉账户的UID。                                                        |                    |
| `user.email`        | `string` | 如果你绑定了电子邮箱地址，则这里会有邮箱地址返回。                                                        |                    |
| `user.gender`       | `number` | 来自 EasiNote 电脑端的逆向：0为未知，1♂，2为♀。                                                  | `0`                |
| `user.joinUnitTime` | `number` | 未知                                                                               |                    |
| `user.nickname`     | `string` | 昵称                                                                               |                    |
| `user.phone`        | `string` | 电话号码                                                                             |                    |
| `user.photoUrl`     | `string` | 头像地址（不要视奸我）                                                                      |                    |
| `user.provinceId`   | `string` | 省份ID，此处请参考：[县及县以上行政区划代码](https://www.gov.cn/test/2011-08/22/content_1930111.htm) | `"500000"` - 重庆市   |
| `user.realName`     | `string` | 真名                                                                               |                    |
| `user.riskLevel`    | `number` | 未知（风险等级？）                                                                        |                    |
| `user.uid`          | `string` | 用户ID，这个在部分情况下会用到。                                                                |                    |
| `user.unitId`       | `string` | 未知                                                                               | `"001"`            |
| `user.username`     | `string` | 用户名，和手机号一样。                                                                      |                    |
| `user.version`      | `number` | 似乎是一个时间戳。暂时未知                                                                    |                    |
| `user.wechatUid`    | `string` | 如果你绑定了微信账户，这里会返回你的微信UID。                                                         |                    |
