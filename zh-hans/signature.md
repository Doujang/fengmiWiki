## 请求打签

### 签名算法

将所有请求参数（不包含signature的通用参数+私有参数）

PHP示例：

```
$tmpArr = array_merge($_POST, [APP_KEY]); // 所有请求参数 + APP_KEY
sort($tmpArr, SORT_STRING); // 作为字符串从低到高排列
$tmpStr = implode("", $tmpArr);
$tmpStr = md5( $tmpStr );
```