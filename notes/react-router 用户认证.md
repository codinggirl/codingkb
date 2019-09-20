---
title: react-router 用户认证
created: '2019-09-20T04:00:15.244Z'
modified: '2019-09-20T07:20:10.245Z'
---

# react-router 用户认证

本文讲述给 React Router 增加用户认证功能。

一般上了规模的 React 应用，都会把一部分内容限制，仅对已登录用户开放。

给应用添加认证功能并不是非常艰难，有许多种方法来达到目的。

## 途径一：路由拦截

取决于你使用的路由库。

如果使用了 [react\-router](https://github.com/reactjs/react-router)，已经内置了路由导航回调。
React Router 的 route 上有一个 *onEnter* 属性，在用户进入指定的路径前会调用。 

```jsx
function loggedIn() {
  // ...
}

function requireAuth(nextState, replace) {
  if (!loggedIn()) {
    replace({
      pathname: '/login'
    })
  }
}

function routes() {
  return (
    <Route path="/" component={App}>
      <Route path="login" component={Login} />
      <Route path="logout" component={Logout} />
      <Route path="checkout" component={Checkout} onEnter={requireAuth} />
    </Route>
  );
}
```

这种方法有诸多限制：

- 需要给每个需要认证的路由增加 *onEnter* 属性。
- *onEnter* 需要你回答是否已经登录。对于依赖 state 的类型的 App 来说，这很复杂，甚至不可能。

# 方法二：使用有登录逻辑的组件

使用组件包装所有需要用户登录的其他路由。

```jsx
import React from 'react';
import { Switch, Route } from 'react-router-dom';
import Cms from './Cms';
import Login from './Login';

const App = () => (
  <div className="app-routes">
    <Switch>
      <Route path="/login" component={Login} />
      <Route path="/" component={Cms} />
    </Switch>
  </div>
);

export default App;
```

我们把登录逻辑放在`Cms`组件中。

在 `Cms` 组件中，如果发现用户没有登录，则直接转到登录页面。

## 结论

使用路由组件比使用组件属性驱动的设计更高效、更易于理解。

使用路由组件时，仅有两个组件需要知道用户的当前登录状态，大大简化了数据的跨路由传递。

## 参考资料

- [Adding Login and Authentication Sections to your React or React Native app](https://medium.com/the-many/adding-login-and-authentication-sections-to-your-react-or-react-native-app-7767fd251bd1)
- [Authentication with Redirect ― Scotch.io](https://scotch.io/courses/using-react-router-4/authentication-with-redirect)

