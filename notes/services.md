在 sails 中， Service 是一段可在 Controller 之间共享的一些可重用代码。

我们应当把业务逻辑存放于 Service 中，而不是 Controller 中。

## 如何编写 Sails Service

下面是一个简单的例子。

```javascript
// File location: /api/services/MyFirstService.js

var MyFirstService = {
    sayHello: function sayHelloService() {
        return 'Hi, there!'
    }
}

module.exports = MyFirstService
```

## 如何在控制器中使用 Sails Service

```
var TestingServicesController = {

  index: function(req, res) {

    // Gets hello message from service
    var helloMessage = MyFirstService.sayHello()

    // Returns hello message to screen
    res.send('Our service has a message for you: ' + helloMessage)
  }
}

module.exports = TestingServicesController
```
