## 获取轮播信息

> 本期开发数据有限，更多数据，后期沟通添加

##### 请求 URL `GET`

```
/oauth2/carousel/info
```

##### 私有参数

```
无
```

> 通用参数必须，请参见：[通用参数](must.md)

##### 返回 格式 `json`

| 字段名 | 变量名 | 类型 | 是否必须 | 示例 |
| ---- | ------ | -------- | ---- | --------- |
| 状态码 | `code` | `int` | 是 | `200` |
| 状态信息 | `msg` | `string` | 是 | `success` |
| 数据集 | `data` | `array` | 是 | ------ |

数据集

| 字段名 | 变量名 | 类型 | 是否必须 | 示例 |
| ----- | -------- | -------- | ---- | ----------------- |
| 订阅量 | `sub_num` | `int` |    | `1` |
| 视频数量 | `video_num`| `int` |     | `2` |
| 总播放量 | `play_num` | `int` |     | `4` |
| 频道状态 | `status` | `string` |     | 具体状态见下表 |

`status` 对应视频状态

| `status` | 对应状态 |
| ----- | -------- |
| `normal` | 正常状态 |
| `deleted` | 视频已删除 |
| `disabled` | 被屏蔽（审核不通过） |

状态码对应信息

```
200: 信息处理成功，返回正常data

340: 请求参数为空: uid is empty
400: 用户频道不存在: carousel not found
```

##### 返回示例

成功示例：

```json
{
    "code":200,
    "msg":"success",
    "data":{
        "sub_num":2,
        "video_num":2,
        "play_num":2,
        "status":"normal"
    }
}
```

错误示例：

```json
{
    "code":400,
    "msg":"uid is empty"
}
```