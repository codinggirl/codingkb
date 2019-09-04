---
tags: [GitHub]
title: 'Uncaught ReferenceError: process is not defined #18139'
created: '2019-09-04T07:21:32.406Z'
modified: '2019-09-04T07:21:42.453Z'
---

# Uncaught ReferenceError: process is not defined #18139

Closed

[msudgh](https://github.com/msudgh) opened this issue on 3 May · 4 comments

Closed

# [Uncaught ReferenceError: process is not defined](#) #18139

[msudgh](https://github.com/msudgh) opened this issue on 3 May · 4 comments

## Comments

[![@msudgh](https://avatars1.githubusercontent.com/u/11756815?s=88&v=4)](https://github.com/msudgh)

Pick your reaction

  👍   👎   😄   🎉   😕   ❤️   🚀  👀 Copy link Quote reply

### **[msudgh](https://github.com/msudgh)** commented [on 3 May](#issue-440057039) •

edited

|

### Preflight Checklist

*   [x]  I have read the [Contributing Guidelines](https://github.com/electron/electron/blob/master/CONTRIBUTING.md) for this project.
*   [x]  I agree to follow the [Code of Conduct](https://github.com/electron/electron/blob/master/CODE_OF_CONDUCT.md) that this project adheres to.
*   [x]  I have searched the issue tracker for an issue that matches the one I want to file, without success.

### Issue Details

*   **Electron Version:** 5.0.0
*   **Operating System:** Arch Linux

### Expected Behavior

I'd expect to access `process` object in my renderer process

### Actual Behavior

Chrome dev tools throw `ReferenceError` while accessing to `process` object.

```js
Uncaught ReferenceError: process is not defined
```

 |

👍 2

Pick your reaction

  👍   👎   😄   🎉   😕   ❤️   🚀  👀

[![@codebytere](https://avatars1.githubusercontent.com/u/2036040?s=88&v=4)](https://github.com/codebytere)

### This comment has been minimized.

Show comment

Hide comment

Copy link Quote reply

[![@codebytere](https://avatars0.githubusercontent.com/u/2036040?s=60&v=4)](https://github.com/codebytere)

#### **[codebytere](https://github.com/codebytere)** [on 3 May](#issuecomment-489137050)

Member

[@msudgh](https://github.com/msudgh) `nodeIntegration` is turned off by default in `5`, so you'll need to enable it via:

```js
const { BrowserWindow } = require('electron')
let win = new BrowserWindow({
  webPreferences: {
    nodeIntegration: true
  }
})
```

which we caution against, or add it via preload script.

Pick your reaction

  👍   👎   😄   🎉   😕   ❤️   🚀  👀 Copy link Quote reply

Member

### **[codebytere](https://github.com/codebytere)** commented [on 3 May](#issuecomment-489137050) •

edited

|

[@msudgh](https://github.com/msudgh) `nodeIntegration` is turned off by default in `5`, so you'll need to enable it via:

```js
const { BrowserWindow } = require('electron')
let win = new BrowserWindow({
  webPreferences: {
    nodeIntegration: true
  }
})
```

which we caution against, or add it via preload script.

 |

