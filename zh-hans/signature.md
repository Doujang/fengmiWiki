## 请求打签

### 签名算法

```
1) 将所有请求参数（不包含signature的通用参数+私有参数）组装数组
2) 安装字符串从低到高排序
3）然后拼接字符串（没有拼接符）
4) 再计算MD5值
```

PHP示例：

```
$tmpArr = array_merge($_POST, [APP_KEY]); // 所有请求参数 + APP_KEY
sort($tmpArr, SORT_STRING); // 作为字符串从低到高排列
$tmpStr = implode("", $tmpArr);
$tmpStr = md5($tmpStr);
```

