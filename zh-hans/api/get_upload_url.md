## 获取上传URL

> 视频上传到影音云，需要每个用户对于URL；为实现不同用户的上传区分，所以上传URL，需要：通用URL + 唯一识别码

### 请求 URL `GET`

```
/oauth2/video/upload/url
```

### 私有参数

```
无
```

> 通用参数必须，请参见：[通用参数](must.md)

#### 返回 格式 `JSON`

| 字段名 | 变量名 | 类型 | 是否必须 | 示例 |
| ---- | ------ | -------- | ---- | --------- |
| 状态码 | `code` | `int` | 是 | `200` |
| 状态信息 | `msg` | `string` | 是 | `success` |
| 数据集 | `data` | `array` | 是 | 见下表 |

数据集

| 字段名 | 变量名 | 类型 | 是否必须 | 示例 |
| ----- | -------- | -------- | ---- | ----------------- |
| 是否支持分块上传 | `block` | `int` | 是 | `1` 分块上传，`0` 不支持分块 |
| 上传url | `url` | `string` | 是 | `无` |

状态码对应信息

```
200: 信息处理成功
500: 未知错误: unknown error, please try again;
```

成功示例：

```json
{
    "code":200,
    "msg":"success",
    "data":{
        "block":1,
        "url":"http://103.15.202.164:8888/upload?key=344A33F03D3DD02ED6CB985735F2D9AF20F4FF4B1D106B82A4C3685F436DB5D9B664D30EC1D697E258F421AC3C6F7A167B1FFC566CF27D76D81320C84C6DD0405C558FEDE69D20AF7BAD0A7D69CA0048F15F3FB5F37E0AA58B3DE24034BEB3CAA640D4BC915FBABFDDA0C2FDA57CA6DA250DD96D8A9FDA139DD65742AF27F8572C50FA436E2697879D494DE663C740DF93E6D9D59D8F3791CDB4044ED19AA581A548823D79F6A26BE5BB4EB54445159217C79CD1DB587F8725E2DF61195A316E025E1215EA888B0200C3B9888EB78000D5D1CDB426ACAE7B2651D96A5AA07A471384F47EAD5BCE11D9C8C438D23E9AB3DE7E36CFB17349B85053F106D10FD719060B67843EF8251BC5CA27790E3EF36A1018B5A182E7357CF88B83DEE10073C8D3FF5CA1A4E105060E33A49F26C5D59CFF819681B2199B05223E68E5518E44F6"
    }
}
```

### 分块上传

*请注意，上传后的视频并不会自动进行转码，需平台方在上传完成后，将得到的`id`字段以`fileid`提交视频信息，方开始转码。*

正常情况，上一步获取到的url,可以支持分块上传，在使用分块上传时，请遵守通用分块上传协议。

下面是分块上传时不同步骤的返回信息：

上传中:
```json
{"OK":0,"debug":0,"data":{"code":101,"message":"Failed to open input stream."}}
```

上传成功:
```json
{"OK":1,"debug":1,"data":{"id":"70c013870ad8093eb821bace48738350"}}
```

上传失败:
```json
{"OK":0,"debug":1,"data":{"code":-1,"message":""}}
```

> 具体分块协议，可从 [风迷官网](http://www.fengmi.tv/) 用户上传分析。

#### 分块协议简析

分块时，请使用“multipart/form-data”这种表单上传文件，其中每一个分块会发出一次请求，表单中有两个字段，分别是“chunk”和“chunks”，其中“chunk”是当前正在处理的文件分块的序号（从0开始计数），而“chunks”则是文件的分块总数。

服务器将会在收到所有文件块后进行组装。

#### 参考链接
- [plupload 上传组件，后台用java实现](http://jeyke.iteye.com/blog/1672934)