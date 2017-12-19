## 获取视频信息

此接口，用于在获取`access_token`后，获取用户资料，以及用户`openid`。

### 请求 URL `GET`

```
/oauth2/user/info
```

### 请求 参数 格式

```
无
```

> 通用参数必须，请参见：[通用参数](must.md)

#### 返回 格式 `json`

| 字段名 | 变量名 | 类型 | 是否必须 | 示例 |
| ---- | ------ | -------- | ---- | --------- |
| 状态码 | `code` | `int` | 是 | `200` |
| 状态信息 | `msg` | `string` | 是 | `success` |
| 数据集 | `data` | `array` | 是 | 见下表 |

数据集

| 字段名 | 变量名 | 类型 | 是否必须 | 示例 |
| ----- | -------- | -------- | ---- | ----------------- |
| 用户标识符 | `openid` | `string` | 是 | `123423sdetewr` |
| 用户昵称 | `nickname` | `string` | 否 | `dingdayu` |
| 用户头像 | `avatar` | `string` | 是 | `` |

`status` 对应视频状态

| `status` | 对应状态 |
| ----- | -------- |
| `deleted` | 视频已删除 |
| `disabled` | 被屏蔽（审核不通过） |
| `transcoding` | 视频转码中 |
| `error` | 视频转码失败 |
| `success` | 视频转码成功，即正常视频 |
| `published` | 视频发布到轮播 |

状态码对应信息

```
200: 信息处理成功，返回正常data

340: 请求参数为空: fileid is empty
400: 文件id错误或不存在: fileid is error
```

##### 返回示例

成功示例：

```json
{
    "code":200,
    "msg":"success",
    "data":{
        "share_url":"http://www.fengmi.tv/wap/video?video_id=971",
        "status":"success",
        "play_num":0
    }
}
```

错误示例：

```json
{
    "code":400,
    "msg":"fileid is error"
}
```