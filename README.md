# payvardoc
# Java API 对外开放接口 1.1

#### 准备资料
用户Gid:  ***
用户私钥：***
#### AES解密

AES密钥 ： ***

AES私钥 ： ***

签名密钥： ***

#### 以下5个接口 ，要以 data,msg,sign 三个参数的方式传递请求

   1.data的值为：每个方法中的请求参数 data,要以 AES加密 。

2. msg的值为：把 data 的加密结果，做 md5 7次，每次都要转成大写字母。
3. sign的值为：是 data={data}&msg={msg}&secret_key={签名密钥}  作为数据源，md5 7次，转成大写字母。





#### 1.同步冷热金额

请求路径
> /api/mhotwallet/gethotdata
>

请求方式

> POST

请求参数data 【要套用上方的传递请求】

| 参数名 | 参数类型 | 必填 | 描述    |
| ------ | -------- | ---- | ------- |
| userId | String   | 是   | 用户Gid |
| mdWu   | String   | 是   | 密文    |

注意：mdWu的计算如下：

1.延值: bdeed5bc4f

2.使用有延值功能 Md5  加密1次

  明文字符串： 用户GID+用户私钥 +延值

{userId}secret_key={Settings.Instance.PrivateKey}{延值}



正确返回结果

```js
{
    "code": 0,
    "msg": "FEF815BE56E367150EA4BDB65A2B9EA2",
    "data": "uVzF8q8SIC0Jm30Gay9BqG9z3/Bw/JzsX8s1Iqmwu7PzaZZjzY4YDhNeG00vopp0io7jXgoqN/wkLwWXEQJYSunRZ1cTiSMKUUnMWf8XdmfcM0Fr50BwCLKRCNsmtnIy/EaoRuZjqXqJaJXTkebakg3N3W9fK+tsJkgdOp61d7jWQQuBFh/Fquvu97LIp7Gg3xWiAbdbo3ewFE9fB2s0Nz98gEYE/Cj5vlObSSXOmAxFncxcBf9VS88IcBQP5Ocr3lZE9yXn5maXEXqNdN5XS3PTWDBtiiBRc4+/wzd5ggpy0R33FvAJ/fiiCtVPh+0Wc/pTb6QvaKHyqMajhiQCDjD7WoU0Xyw4hhRIIOrz43Wvq9ezjQTEQuvnIDGCXl2h2TeRPbLLqiqLMjVnuZ/xbDleQ2awYLU//1n1pvTvnvbLz1IMUzgcge7WFshw9i1FsHF4Stl6bJS5OoxDs53OH60NbeFUKao/gTuCG/bXrQXoH8wl1ASjjjM98gQ68ud1Rrrhjw2ZnB01RrTWEclJjmKA1QTSzSMv3RDaa86wWQ2mJLlFbmUcYZE/pG0q8KXJ3n8NAD1LkE6PIzZ7OuVoX0Zszdi6tEPHKz+yMOa7wuMhwPULOp1q6ichEBqlqHg3E5rcPftla9jsxVTMnY6MqbtoUhBwXZKE2UWWQ4wQbOvBKKAtK2rkVan3GtzgoqBO81hhRNMTykzmpWHGf45dBt0S0wZ1Bitfv5fuACeT4jxkF4Cjcz9c8aBhsV8FY4lYeuXxPokgR0yifMp8LpnYUWX8cL1fNYbmEGuTBdIykHLwJU0spkMTginB2LKoM9qKyr5gCxJEKE56HByLGPtZPPW9AbwW4Qi4Z1VrZsmJhxFA4RSoOyr/ZmN8Rjh0z55h/EPJChdCNcO0PhMZ+Xq3WmxNTo1Lb7AwWTtlngqM3WLc0eDNYqIYJBJuCfo5KiXhVPzkf9Rb0gkTghZEP2rypZ481kp6gUtXOPQK6EaY+stk3dThWR1IGC5cBWz1LVfdQMbfx+Vw2/hXrDDnaKlWOnbFQOYLBuk1cKVto1Clo3KmboBDd94MLLuQX1R1fhaVxn6aDpg+40T7rSSTzZJ7l9t60MrQw9lrLbsOXcv0f0+IGGXiwRFqY9FqcqwKP4zn"
}
```





| 参数名 | 参数类型 | 作用 |
| ------ | -------- | ---- |
| code | Int | 0是成功状态 |
| data | string | AES算法加密明文的结果 |
| msg | String   | data内容做md5 7次的结果 |

data 序列化对象 说明  

对象：列表  

| 参数名 | 参数类型 | 描述 |
| ------ | -------- | ---- |
| CoinTypeId | long | 币的ID |
| Addr | string | 热地址 |
| CoinType | string | 币种名称 |
| HotSum | BigDecimal | 热金额 |
| CoolSum | BigDecimal | 冷金额 |
| MdWu | string | 币种+热地址+用户GID+热金额[8位小数]+冷金额[8位小数] + 用户私钥 +延值  md5 1次 |



错误返回结果

```js
{
  "code": -1,
  "msg": "数据格式不对。",
  "data": null
}

{
  "code": -1,
  "msg": "没有开通的币种。",
  "data": null
}
```

#### 2.同步冷地址

请求路径

> /api/mcool/getaddrs

请求方式

> POST

请求参数【要套用上方的传递请求】

| 参数名       | 参数类型 | 必填 | 描述       |
| ------------ | -------- | ---- | ---------- |
| userId       | String   | 是   | 用户Gid    |
| coinTypeName | String   | 是   | 币种名称   |
| count        | int      | 是   | 采集的条数 |
| mdWu         | string   | 是   | 密文       |

注意：mdWu的计算如下：

1.延值: bdeed5bc4f

2.使用有延值功能 Md5  加密1次

  明文字符串：用户ID+币种+数量+用户私钥+延值 

{userId}{coinTypeName}{count}secret_key={Settings.Instance.PrivateKey}{延值}



正确返回结果

```json
{
    "code": 0,
    "msg": "878CA60461EC62D07D72845E5BFF06BF",
    "data": "kRuIRxYIVmArKMoBueiUKrPHAf2tTRXW1Y5Nc8NBY73JJNcF4o9lO6RACd4o6AN+TxgCTuD02VDuLI5pRZOUvT4ZXwTvo0W8iznyLYCPTK1GNuStCGWz1YtGbpE5yIJJF6nhrCvkRkXsLm5MPkVf8IWgwz6R+TmRUtCQFbkznOHTooh1K4NBxBdCi8+iAe53P29osN2pOjvWPTHmoqJiyNo5rBZNsn2gJxdNmcVYSBL5hUCcj6sc5+q8yV1kBTR9dnYvX7xiTV1TGQ/SCCT5ONVr+a9Fqtzbhl8GE3VzZ8sZRHeAPVtMe6Oq9XbfSrGHiUWGlshxtqrZBaUraZxJgsW0zPE3Z7zXP6EN7DYjP8qdflKDHmBiMV4WWbOqS4hRVU6EeAck08azMPjKoDBSxtri25Iq7nGePanpaEI6Guc+cnOYhvceCTygQJVMWJ3uTgtsJrcVg5Ojt/ACNN5zvz8T7Cer6rDgLWw6r/jxSQKDX5YE+nMNp39zxd5WQdTUsz0UhA1f5LjSBcTIBlXui9L4ZnShH7cJX1+pOIzm9J4MK8F0ElzLDhOl7hkv1ZDgYTpKo3fneAkZhKa43T31IyBYAmz0STRqXQ+hy7vknc98IWGSyzQ5n6tCylrClc6ms9cZEjdX3RvuhQzc13W+CJh5xrVOlrcKiHVTlgcPLO/xrVDpi68xDY9IdgOzh9cFq4s85q1MspBce3emYAPSaWzhE5ArCjzphoze7EIPjLtkYA3SjY4ALs2BKTAvqjtC"
}
```



| 参数名 | 参数类型 | 作用                    |
| ------ | -------- | ----------------------- |
| code   | Int      | 0是成功状态             |
| data   | string   | AES算法加密明文的结果   |
| msg    | String   | data内容做md5 7次的结果 |

data 序列化对象 说明  

对象：列表  

| 参数名   | 参数类型 | 描述                                       |
| -------- | -------- | ------------------------------------------ |
| Addr     | string   | 冷地址                                     |
| CoinType | string   | 币种名称                                   |
| MdWu     | string   | 币种+冷地址+用户ID+用户私钥 +延值  md5 1次 |



错误返回结果

```json
{
  "code": -1,
  "msg": "数据格式不对。",
  "data": null
}
{
  "code": -1,
  "msg": "mdwu没有匹配成功。",
  "data": null
}
{
  "code": -1,
  "msg": "没有开通的币种。",
  "data": null
}
```





#### 3.发起用户提币申请

请求路径

> /api/mwithdrawlist/uploadwithdraw

请求方式

> POST

请求参数【要套用上方的传递请求】

| 参数名        | 参数类型   | 必填 | 描述            |
| ------------- | ---------- | ---- | --------------- |
| coinType      | String     | 是   | 币种名称        |
| toAddress     | String     | 是   | 收币地址        |
| amountDecimal | BigDecimal | 是   | 金额            |
| userGid       | string     | 是   | 用户gid         |
| orderId       | String     | 是   | 交易所提现表 id |
| mdwu          | String     | 是   | 密文            |

注意：mdWu的计算如下：

1.延值: bdeed5bc4f

2.使用有延值功能 Md5  加密1次

  明文字符串：用户GID+币种名称+收币地址+金额【8位小数】+orderId+ 用户私钥 +延值

{userGid}{coinType}{toAddress}{ amountDecimal.ToString("0.########")}{orderId}{Settings.Instance.PrivateKey}{延值}



正确返回结果

```json
{
    "code":0,
    "msg":"44DFA36A55EBA566965F9073E2C37846",
    "data":"O3K/cnKnWpn0fODZm3oVzQ=="
}
```



| 参数名 | 参数类型 | 作用                    |
| ------ | -------- | ----------------------- |
| code   | Int      | 0是成功状态             |
| data   | string   | AES算法加密明文的结果   |
| msg    | String   | data内容做md5 7次的结果 |

data 序列化对象 说明  

对象：int  1

小技巧：只要 long.Parse(result) > 0 就是成功的。



错误返回结果

```json
{
  "code": -1,
  "msg": "数据格式不对。",
  "data": null
}
{
  "code": -1,
  "msg": "mdwu不对。",
  "data": null
}
{"code":-1,
 "msg":"提现数据已存在。",
 "data":null
}
```
#### 4.请求用户提币结果

请求路径

> /api/mwithdrawlist/getwithdrawdata

请求方式

> POST

请求参数【要套用上方的传递请求】

| 参数名        | 参数类型   | 必填 | 描述            |
| ------------- | ---------- | ---- | --------------- |
| toAddress     | String     | 是   | 收币地址        |
| amountDecimal | BigDecimal | 是   | 金额            |
| userGid       | string     | 是   | 用户gid         |
| orderId       | String     | 是   | 交易所提现表 id |
| mdwu          | String     | 是   | 密文            |

注意：mdWu的计算如下：

1.延值: bdeed5bc4f

2.使用有延值功能 Md5  加密1次

  明文字符串：用户GID+收币地址+金额【8位小数】+orderId+ 用户私钥 +延值

{userGid}{toAddress}{ amountDecimal.ToString("0.########")}{orderId}{Settings.Instance.PrivateKey}{延值}



正确返回结果

```json
{
    "code": 0,
    "msg": "E00811CDA193B3F2D0015D1255BA0BFA",
    "data": "U33h/Hq19K0O/5ODl1lxq8ScthdKWXzCcKvPqyGZFrotMmeckYBjvT5uT0LVcwwxdkuHQBVodQBRKCKbLcwdaGRNQQYjB2I06beGUT8pPdYRjgzq1UK/XrHipmL+CNdJMDRe967THpnCp9gVSxhQ7y/soxZnhUPo1tSQar1t73UCdLSPmKkMsYArQz9vH2DmnDJRZufwY+Vwno2zBD3DZBnjMKmK+teP6WxaDNlhz96zTW2DF20caqgAedWLopQO2GtnP1LbZEjrYAfJEZEoV/1j2yG5bO9l5Gqosh44MC9hnfK7Pkh3EobnKAR/YwCivP4BfzZ+irFSqIL1Xrp7CA=="
}
```



| 参数名 | 参数类型 | 作用                    |
| ------ | -------- | ----------------------- |
| code   | Int      | 0是成功状态             |
| data   | string   | AES算法加密明文的结果   |
| msg    | String   | data内容做md5 7次的结果 |

data 序列化对象 说明  

对象：单个Model 



| 参数名      | 参数类型   | 描述                                |
| ----------- | ---------- | ----------------------------------- |
| coinName    | string     | 冷币名称                            |
| blockHash   | string     | 币种txid                            |
| freeDecimal | BigDecimal | 手续费                              |
| userGid     | String     | 用户Gid                             |
| states      | int        | 0:处理中， 1：成功 2：失败          |
| orderId     | String     | 交易所的id                          |
| mdwu        | String     | 用户GID+txid+orderId 用户私钥 +延值 |





错误返回结果

```json
{
  "code": -1,
  "msg": "数据格式不对。",
  "data": null
}
{
  "code": -1,
  "msg": "mdwu不对。",
  "data": null
}
{"code":-1,
 "msg":"数据不存在。",
 "data":null
}
```





#### 5.用户充值

请求路径

> /api/mrechangelist/getuserrechange

请求方式

> POST

请求参数【要套用上方的传递请求】

| 参数名  | 参数类型 | 必填 | 描述    |
| ------- | -------- | ---- | ------- |
| userGid | String   | 是   | 用户Gid |
| mdwu    | String   | 是   | 密文    |



注意：mdWu的计算如下：

1.延值: bdeed5bc4f

2.使用有延值功能 Md5  加密1次

  明文字符串：用户GID+用户私钥 +延值

{userGid}{Settings.Instance.PrivateKey}{延值}



正确返回结果

```json
{
    "code": 0,
    "msg": "FAF8DDC6ED35E6DBBA49049B12BA1068",
    "data": "L48BgfYEP2zbEHl+gJSND19FR3rDEGiaO8tGhy87c8CH9PCYH8Liaz6OPyi0gmXpgz1v+O40D4kTMYGzhrTkSiPOQGdt6HQsLmR2Wc2yCweatKkZSgfnk5BUB9YE8Xbn9PtQOgdJFeLEp81aTEFjKzyJ07EtGVN9o3pSM0KmZ8GEcUBvxdCSNuxv3tA/2MvWcNfZb3SktPRiGiLiI65nN1Sqvh7+k1+UY+O+gmmqtoGibDjHZCBOAQ3A1tzyycuZADB07CExTzZVI4VUc5RR5JTmmN6MvknRnI0SOEuLv2fb28HUG09JJI8GNjic7W4ctpE8GNGFRfSXdnhAkGovuXK3ytplvSTtZZi1QcuOvKjr9W4rlJEYrGkFqUYfgLHtYlNpoMFtdRuy6LLMIYkBcnpMxs6qe7dDZVtv9SXjJoTrLRa/T5i0fUc+uM2O/waR0aTnq5QXHGM2hh+1Whve5VHioRg4yJcBFR8T+jtsvdS4W50T8ynKtiiSIpvBV866"
}
```





| 参数名 | 参数类型 | 作用                    |
| ------ | -------- | ----------------------- |
| code   | Int      | 0是成功状态             |
| data   | string   | AES算法加密明文的结果   |
| msg    | String   | data内容做md5 7次的结果 |

data 序列化对象 说明  

对象：列表



| 参数名        | 参数类型   | 描述                                                     |
| ------------- | ---------- | -------------------------------------------------------- |
| coinType      | String     | 冷币名称                                                 |
| transTime     | Date       | 交易时间                                                 |
| fromAddr      | String     | 打币地址                                                 |
| toAddr        | String     | 目标地址                                                 |
| amountDecimal | BigDecimal | 金额                                                     |
| feeDecimal    | BigDecimal | 手续费                                                   |
| txid          | String     | txid                                                     |
| walletDataId  | long       | sqlserver ID                                             |
| corfirm       | int        | 确认数                                                   |
| mdwu          | String     | 用户GID+币种名称+收币地址+金额【8位小数】+用户私钥 +延值 |



错误返回结果

```json
{
  "code": -1,
  "msg": "数据格式不对。",
  "data": null
}
{
  "code": -1,
  "msg": "mdwu没有匹配成功。",
  "data": null
}
{"code":-1,
 "msg":"数据不存在。",
 "data":null
}
```





