---
title: URL 设计规范
created: '2019-09-04T10:03:12.097Z'
modified: '2019-09-04T10:03:20.142Z'
---

# URL 设计规范

发表于 2018\-09\-01 |   分类于 [URL](https://zhulichao.github.io/categories/URL/)   |  [](https://zhulichao.github.io/2018/09/01/url-design/#comments)

*   字母全部用小写，尽量短
*   使用 kebab\-case（短横线分割），谷歌官方建议，有利于 SEO
*   使用 / 标识资源层级
*   在查询字符串或资源字段中使用 camelCase
*   不应包含任何用户相关的信息
*   避免太多参数，目录层次尽量少
*   总是使用复数名词来命名指向一个集合的 url：/users
*   在源代码中，将复数转换为具有列表后缀名描述的变量和属性
*   坚持这样一个概念：始终以集合名起始并以标识符结束，如 /students/123
*   URLs 里面请尽量少用动词
*   为非资源型请求使用动词，这种情况下，API 并不需要返回任何资源，而是去执行一个操作并返回执行结果，这些不是 CRUD 操作
*   请求体或响应类型如果是 JSON，那么请遵循 camelCase 规范为 JSON 属性命名来保持一致性
*   即使资源类似于对象实例或数据库记录的单一概念，也不应该将 table\_name 用作资源名称或将 column\_name 作为资源属性
*   只有在您的 URL 上面命名资源时才使用名词，不要尝试解释其功能
*   对于嵌套资源，请在 URL 中把他们的关系表现出来，如 /schools/2/students/31，表示得到 2 学校的 31 学生的信息

[URL 设计规范 | 前端历险记](https://zhulichao.github.io/2018/09/01/url-design/)
