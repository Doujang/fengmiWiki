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
$signature = md5($tmpStr);
```

测试代码：http://www.dooccn.com/php7/#id/9b897172b9f0c1d2f9e9ec0cb85c3803

JAVA示例：

```
// 补充 APP_KEY
string[] tmpArr = postData.add(APP_KEY);
// 排序
java.util.Arrays.sort(tmpArr);

// 拼接字符串
String tmpStr = "";
for(String str:tmpArr) {
       tmpStr += str;
}

// 生成一个MD5加密计算摘要
java.security.MessageDigest md = java.security.MessageDigest.getInstance("MD5");
md.update(tmpStr.getBytes());
String signature = new java.math.BigInteger(1, md.digest()).toString(16);
```

测试代码：http://www.dooccn.com/java1.7/#id/37c3d51d851f1150993e79741eca50b6


Python示例

```
import hashlib 

postData.append(APP_KEY);
postData.sort();
tmpStr = "".join(postData)
m2 = hashlib.md5()   
m2.update(tmpStr.encode("utf-8"))   
signature = m2.hexdigest()
```

测试代码：http://www.dooccn.com/python3/#id/697ad3b857e08e2dd315fd496ceeaf9e

Golang示例

```
postData = append(postData, APP_KEY)
tmpStr = strings.Join(postData, "")
sort.Strings(postData)
tmpStr := strings.Join(postData, "")
h := md5.New()
io.WriteString(h, tmpStr)
signature := fmt.Sprintf("%x", h.Sum(nil))
```

测试代码：http://www.dooccn.com/go/#id/7de60ea92352c9bc172c6aada4e336ac