## 用户授权（登陆按钮）

平台方在其网站上放置登陆按钮，用户点击后跳转回风迷平台进行登陆授权。

### 获取 `authorization_code`

通过在浏览器中访问下面的地址，来引导用户授权，并获得 authorization_code

```
https://www.fengmi.tv/oauth2/authorize
```

#### 参数

| 参数名称          | 参数说明                                     | 必须   |
| ------------- | ---------------------------------------- | ---- |
| app_id        | 应用的唯一标识，对应于AppID                         | 是    |
| redirect_uri  | 用户授权完成后的回调地址，应用需要通过此回调地址获得用户的授权结果。此地址必须与在申请时所填域名一致。 | 是    |
| state         | 可选参数，用来维护请求和回调状态的附加字符串，在授权完成回调时会附加此参数，应用可以根据此字符串来判断上下文关系。 |      |
| scope         | 申请权限的范围，如果不填，则使用缺省的 scope。目前固定值`carousel`。 |      |
| response_type | 此值可以为 code 或者 token 。在本流程中，此值为 `code`    |      |

例如:
```
https://www.fengmi.tv/oauth2/authorize?
  app_id=0b5405e19c58e4cc21fc11a4d50aae64&
  redirect_uri=https://www.example.com/back&
  response_type=code&
  scope=carousel&
  state=142
```

> 请注意：此请求必须为浏览器GET请求，所有携带参数如果有特殊字符，均应urlencode

#### 返回结果

- 当用户拒绝授权时，浏览器会重定向到 redirect_uri，并附加错误信息
```
https://www.example.com/back?error=access_denied
```
- 当用户同意授权时，浏览器会重定向到 redirect_uri，并附加 autorization_code
```
https://www.example.com/back?code=9b73a4248&state=142
```

### 获取 `access_token`

平台方服务在通过回跳链接获取code后，然后服务端通过下面的接口，通过`code`换取`access_token`

```
https://www.fengmi.tv/oauth2/token
```

| 参数名称      | 参数说明                            | 必须   |
| --------- | ------------------------------- | ---- |
| app_id    | 应用的唯一标识，对应于 AppID               | 是    |
| code      | 上一步中获得的 authorization_code | 是    |
| signature | 签文，请查看 [签名] 章节                  | 是    |

例如：
```
https://www.fengmi.tv/oauth2/token?
  app_id=0b5405e19c58e4cc21fc11a4d50aae64&
  signature=edfc4e395ef93375&
  code=9b73a4248
```

#### 返回结果

```
{
  "access_token":"a14afef0f66fcffce3e0fcd2e34f6ff4",
  "expires_in":7200,
  "refresh_token":"5d633d136b6d56a41829b73a424803ec",
  "openid":"1221"
}
```


> 请注意，此处的`expires_in`尚未完全确定，请根据返回的时间进行计算

### 刷新 `access_token`

在 OAuth 2.0 中，access_token 不再长期有效。在授权获取 access_token 时会一并返回其有效期，也就是返回值中的 expires_in 参数。

在 access_token 使用过程中，如果服务器返回106错误：“access_token_has_expired ”，此时，说明 access_token 已经过期，除了通过再次引导用户进行授权来获取 access_token 外，还可以通过 refresh_token 的方式来换取新的 access_token 和 refresh_token。

通过 refresh_token 换取 access_token 的处理过程如下：

```
https://www.fengmi.tv/oauth2/token/refresh
```

| 参数名称          | 参数说明              | 必须   |
| ------------- | ----------------- | ---- |
| app_id        | 应用的唯一标识，对应于 AppID | 是    |
| refresh_token | 必选参数，刷新令牌         | 是    |
| signature     | 签文，请查看 [签名] 章节    | 是    |

注意：此请求必须是 HTTP POST 方式，refresh_token 刷新`access_token`时，旧的`access_token`就会失效; `refresh_token` 会在用户授权超过一年，或又重新授权时无法使用。

例如：

```
https://www.fengmi.tv/oauth2/token/refresh?
  client_id=0b5405e19c58e4cc21fc11a4d50aae64&
  signature=edfc4e395ef93375&
  refresh_token=5d633d136b6d56a41829b73a424803ec
```

```
{
  "access_token":"a14afef0f66fcffce3e0fcd2e34f6ff4",
  "expires_in":7200,
  "refresh_token":"5d633d136b6d56a41829b73a424803ec",
  "openid":"1221"
}
```

### 链接

- [针对近期“博全球眼球OAuth漏洞”的分析与防范建议](http://blog.knownsec.com/2014/05/oauth_vulnerability_analysis/)
- [隐蔽重定向漏洞](https://zh.wikipedia.org/wiki/%E9%9A%B1%E8%94%BD%E9%87%8D%E5%AE%9A%E5%90%91%E6%BC%8F%E6%B4%9E)