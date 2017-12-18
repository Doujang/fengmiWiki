## 获取视频信息 `new`

此接口，可以用来获取用户/平台在上传视频后，获得最新的视频转码信息，以及成功后的平台分享链接。

### 请求 URL `GET`

```
/oauth2/video/info
```

### 请求 参数 格式

| 字段名 | 变量名 | 类型 | 是否必须 | 示例 |
| ------ | ---------- | -------- | ---- | -------- |
| 文件id | `fileid` | `string` | 是 | `D1C35E26899F263CC84B5C6F872986CB`

#### 返回 格式 `json`

| 字段名 | 变量名 | 类型 | 是否必须 | 示例 |
| ---- | ------ | -------- | ---- | --------- |
| 状态码 | `code` | `int` | 是 | `200` |
| 状态信息 | `msg` | `string` | 是 | `success` |
| 数据集 | `data` | `array` | 是 | 见下表 |

数据集

| 字段名 | 变量名 | 类型 | 是否必须 | 示例 |
| ----- | -------- | -------- | ---- | ----------------- |
| 视频分享网页链接 | `shareUrl` | `string` | 否 | `1` |
| 视频状态 | `status` | `string` | 否 | 具体状态见下表 |
| 视频播放量 | `playNum` | `int` | 否 | `无` |

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
        "shareUrl":"http://www.fengmi.tv/wap/video?video_id=971",
        "status":"success",
        "playNum":0
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