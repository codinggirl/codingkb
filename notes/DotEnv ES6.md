---
tags: [GitHub]
title: DotEnv ES6
created: '2019-09-04T07:20:22.682Z'
modified: '2019-09-04T07:20:40.126Z'
---

# DotEnv ES6

# Using with ES6 modules #133

Closed

[jsonmaur](https://github.com/jsonmaur) opened this issue on 3 Apr 2016 · 15 comments

Closed

# [Using with ES6 modules](#) #133

[jsonmaur](https://github.com/jsonmaur) opened this issue on 3 Apr 2016 · 15 comments

## Comments

[![@jsonmaur](https://avatars3.githubusercontent.com/u/911274?s=88&v=4)](https://github.com/jsonmaur)

Pick your reaction

  👍   👎   😄   🎉   😕   ❤️   🚀  👀 Copy link Quote reply

### **[jsonmaur](https://github.com/jsonmaur)** commented [on 3 Apr 2016](#issue-145471155)

|

In the case of using ES6 modules via `import`, configuring dotenv in the base file doesn't set the environment vars in sub\-modules. For example:

###### index.js

```js
import dotenv from 'dotenv'
dotenv.config({ silent: true })
import hello from './hello'
```

###### hello.js

```js
console.log(process.env) /* this doesn't have the environment vars in it */
```

This is because babel compiles the above as

```js
var dotenv = require('dotenv')
var hello = require('./hello')
dotenv.config({ silent: true })
```

It works if I do `import 'dotenv/config'` instead, but then I can't specify the `silent` flag, so it throws an error if `.env` doesn't exist (for example, if running in production). I think a simple solution would be to allow something such as `import 'dotenv/register'` for a one\-line, silent setup of the variables.

 |

👍 1

Pick your reaction

  👍   👎   😄   🎉   😕   ❤️   🚀  👀

[![@jsonmaur](https://avatars3.githubusercontent.com/u/911274?s=88&v=4)](https://github.com/jsonmaur)

### This comment has been minimized.

Show comment

Hide comment

Copy link Quote reply

[![@jsonmaur](https://avatars2.githubusercontent.com/u/911274?s=60&v=4)](https://github.com/jsonmaur)

#### **[jsonmaur](https://github.com/jsonmaur)** [on 3 Apr 2016](#issuecomment-204918300)

Author

I see that there were a few other issues for this in the past, but I didn't see that a solution was ever reached? I know preloading the module was recommended, but unfortunately that's not an option in this case. I'd be happy to submit a PR for this if you'd like.

Pick your reaction

  👍   👎   😄   🎉   😕   ❤️   🚀  👀 Copy link Quote reply Reference in new issue

### Reference in new issue

Repository

 dotenv Repositories

Title

Body

I see that there were a few other issues for this in the past, but I didn't see that a solution was ever reached? I know preloading the module was recommended, but unfortunately that's not an option in this case. I'd be happy to submit a PR for this if you'd like. \_Originally posted by @jsonmaur in https://github.com/motdotla/dotenv/issues/133#issuecomment\-204918300\_

Create issue

Author

### **[jsonmaur](https://github.com/jsonmaur)** commented [on 3 Apr 2016](#issuecomment-204918300)

|

I see that there were a few other issues for this in the past, but I didn't see that a solution was ever reached? I know preloading the module was recommended, but unfortunately that's not an option in this case. I'd be happy to submit a PR for this if you'd like.

 |

[![@jcblw](https://avatars0.githubusercontent.com/u/578259?s=88&v=4)](https://github.com/jcblw)

### This comment has been minimized.

Show comment

Hide comment

Copy link Quote reply

[![@jcblw](https://avatars1.githubusercontent.com/u/578259?s=60&v=4)](https://github.com/jcblw)

#### **[jcblw](https://github.com/jcblw)** [on 7 Apr 2016](#issuecomment-206944138)

Collaborator

hey [@jsonmaur](https://github.com/jsonmaur) you can set options using the preload script. You can pass in the options via the a arg on the command.

```shell
node -r dotenv/config your_script.js dotenv_config_silent=true
```

Pick your reaction

  👍   👎   😄   🎉   😕   ❤️   🚀  👀 Copy link Quote reply Reference in new issue

### Reference in new issue

Repository

 dotenv Repositories

Title

Body

hey @jsonmaur you can set options using the preload script. You can pass in the options via the a arg on the command. \`\`\` shell node \-r dotenv/config your\_script.js dotenv\_config\_silent=true \`\`\` \_Originally posted by @jcblw in https://github.com/motdotla/dotenv/issues/133#issuecomment\-206944138\_

Create issue

Collaborator

### **[jcblw](https://github.com/jcblw)** commented [on 7 Apr 2016](#issuecomment-206944138)

|

hey [@jsonmaur](https://github.com/jsonmaur) you can set options using the preload script. You can pass in the options via the a arg on the command.

```shell
node -r dotenv/config your_script.js dotenv_config_silent=true
```

 |

 [Using with ES6 modules · Issue #133 · motdotla/dotenv](https://github.com/motdotla/dotenv/issues/133)
 
