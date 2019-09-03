---
tags: [http]
title: HTTP协议中的body部分格式
created: '2019-09-03T03:05:33.116Z'
modified: '2019-09-03T09:16:05.857Z'
---

# HTTP协议中的body部分格式

通常，HTTP中客户端Content-Type 有以下几种格式：

- `application/x-www-form-urlencoded`
- `application/json`
- multipart

HTML表单默认是按照`application/x-www-form-urlencoded`进行编码的。

## application/x-www-form-urlencoded

以 URLEncoded 的方式进行编码

`name=test&sub%5B%5D=1&sub%5B%5D=2`
 
解码后就是：
  
`name=test&sub[]=1&sub[]=2`

不像 JSON，`application/x-www-form-urlencoded` 的方式对复杂类型（例如数组）的处理，并没有严格的标准。

例如，对于`{key: ['a', 'b']}`形式的 js 对象，有以下不同的表示方法，不同的客户端、服务器端处理不一定一致。这会带来许多隐患和未知的问题。

- `key[]=a&key[]=b`
- `key=a&key=b`
- `key[0]=a&key[1]=b`

## application/json

消息主体是序列化后的 JSON 字符串，要求服务器端能够支持JSON 

`{"name":"test","sub":[1,2]}`

如果要传递的内容是数组或嵌套更深的内容，建议使用 JSON。


