---
tags: [GitHub]
title: 'Whats the reason to always use .jsx instead of .js #1235'
created: '2019-09-04T07:24:18.358Z'
modified: '2019-09-04T07:24:36.954Z'
---

# Whats the reason to always use .jsx instead of .js #1235

Closed

[danielfeelfine](https://github.com/danielfeelfine) opened this issue on 7 Jan 2017 · 3 comments

Closed

# [Whats the reason to always use .jsx instead of .js](#) #1235

[danielfeelfine](https://github.com/danielfeelfine) opened this issue on 7 Jan 2017 · 3 comments

## Comments

[![@danielfeelfine](https://avatars2.githubusercontent.com/u/1668289?s=88&v=4)](https://github.com/danielfeelfine)

Pick your reaction

  👍   👎   😄   🎉   😕   ❤️   🚀  👀 Copy link Quote reply

### **[danielfeelfine](https://github.com/danielfeelfine)** commented [on 7 Jan 2017](#issue-199361309)

|

Hello, I would like to know about using **.js** and **.jsx** as an extension of the React files:

Are there any differences? Which are?

*   In which cases would the use of **.jsx** instead of **.js** is more recommended?
*   This question would be relevant when using: *Babel*/*Webpack* for precompiling files?

Thanks

 |

[![@ljharb](https://avatars1.githubusercontent.com/u/45469?s=88&v=4)](https://github.com/ljharb)

### This comment has been minimized.

Show comment

Hide comment

Copy link Quote reply

[![@ljharb](https://avatars0.githubusercontent.com/u/45469?s=60&v=4)](https://github.com/ljharb)

#### **[ljharb](https://github.com/ljharb)** [on 8 Jan 2017](#issuecomment-271094968)

Collaborator

See the discussion in [#985](https://github.com/airbnb/javascript/pull/985), but essentially, `.js` should only ever be for files that use features of standard JavaScript \- anything nonstandard, like jsx, flow, typescript, or early\-stage proposals, needs a different extension. For convenience, we've chosen `.jsx` but you could choose any unique extension you like.

Pick your reaction

  👍   👎   😄   🎉   😕   ❤️   🚀  👀 Copy link Quote reply Reference in new issue

### Reference in new issue

Repository

 javascript Repositories

Title

Body

See the discussion in #985, but essentially, \`.js\` should only ever be for files that use features of standard JavaScript \- anything nonstandard, like jsx, flow, typescript, or early\-stage proposals, needs a different extension. For convenience, we've chosen \`.jsx\` but you could choose any unique extension you like. \_Originally posted by @ljharb in https://github.com/airbnb/javascript/issues/1235#issuecomment\-271094968\_

Create issue

Collaborator

### **[ljharb](https://github.com/ljharb)** commented [on 8 Jan 2017](#issuecomment-271094968)

|

See the discussion in [#985](https://github.com/airbnb/javascript/pull/985), but essentially, `.js` should only ever be for files that use features of standard JavaScript \- anything nonstandard, like jsx, flow, typescript, or early\-stage proposals, needs a different extension. For convenience, we've chosen `.jsx` but you could choose any unique extension you like.

 |

❤️ 22

Pick your reaction

  👍   👎   😄   🎉   😕   ❤️   🚀  👀

### [![@ljharb](https://avatars0.githubusercontent.com/u/45469?s=60&v=4)](https://github.com/ljharb) [ljharb](https://github.com/ljharb) closed this [on 8 Jan 2017](#event-914994875)

### [![@ljharb](https://avatars0.githubusercontent.com/u/45469?s=60&v=4)](https://github.com/ljharb) [ljharb](https://github.com/ljharb) added the  [question](https://github.com/airbnb/javascript/labels/question)  label [on 8 Jan 2017](#event-914994907)

[![@JoshuaVSherman](https://avatars3.githubusercontent.com/u/13340120?s=60&v=4)](https://github.com/JoshuaVSherman) [JoshuaVSherman](https://github.com/JoshuaVSherman) referenced this issue [on 1 Apr 2018](#ref-issue-310264441)

[make the /components/ReactTestComp.js be in jsx format, but be a .js file extension and work as does other .js file #1048](https://github.com/WebJamApps/combined-front/issues/1048)

Closed

[![@jk1121](https://avatars0.githubusercontent.com/u/16221733?s=88&v=4)](https://github.com/jk1121)

### This comment has been minimized.

Show comment

Hide comment

Copy link Quote reply

[![@jk1121](https://avatars1.githubusercontent.com/u/16221733?s=60&v=4)](https://github.com/jk1121)

#### **[jk1121](https://github.com/jk1121)** [on 3 Oct 2018](#issuecomment-426395317)

**.jxs** will describe the **pupose/contents** of the file from a higher level .It'll be easier while navigating to different files across the project.

Pick your reaction

  👍   👎   😄   🎉   😕   ❤️   🚀  👀 Copy link Quote reply Reference in new issue

### Reference in new issue

Repository

 javascript Repositories

Title

Body

\*\*.jxs\*\* will describe the \*\*pupose/contents\*\* of the file from a higher level .It'll be easier while navigating to different files across the project. \_Originally posted by @jk1121 in https://github.com/airbnb/javascript/issues/1235#issuecomment\-426395317\_

Create issue

### **[jk1121](https://github.com/jk1121)** commented [on 3 Oct 2018](#issuecomment-426395317)

|

**.jxs** will describe the **pupose/contents** of the file from a higher level .It'll be easier while navigating to different files across the project.

 |

[![@JoshuaVSherman](https://avatars3.githubusercontent.com/u/13340120?s=60&v=4)](https://github.com/JoshuaVSherman) [JoshuaVSherman](https://github.com/JoshuaVSherman) referenced this issue [on 6 Oct 2018](#ref-issue-225961299)

[userutil.js after the new user verifies their email address, route them directly to the login page with the login form displayed (not to the homepage) #248](https://github.com/WebJamApps/combined-front/issues/248)

Closed

[![@williscool](https://avatars0.githubusercontent.com/u/394931?s=88&v=4)](https://github.com/williscool)

### This comment has been minimized.

Show comment

Hide comment

Copy link Quote reply

[![@williscool](https://avatars1.githubusercontent.com/u/394931?s=60&v=4)](https://github.com/williscool)

#### **[williscool](https://github.com/williscool)** [on 10 Oct 2018](#issuecomment-428413675)

It seems like the react team at facebook and the airbnb team that own the most popular javascript style guide here are at odds with each other over file extensions.

Airbnb team seems adamant about not having any non standard code in a `js` file re this issue ([#1235](https://github.com/airbnb/javascript/issues/1235))

and this pull [#985](https://github.com/airbnb/javascript/pull/985)

and names any files with the stuff should have `jsx`

Facebook team seems adamant that `js` should be the only extension no matter what is in the file

[facebook/create\-react\-app#87 (comment)](https://github.com/facebook/create-react-app/issues/87#issuecomment-234627904)

For my (and probably others) future reference when setting up a new js repo with the `eslint-config-airbnb`

Pick your reaction

  👍   👎   😄   🎉   😕   ❤️   🚀  👀 Copy link Quote reply Reference in new issue

### Reference in new issue

Repository

 javascript Repositories

Title

Body

It seems like the react team at facebook and the airbnb team that own the most popular javascript style guide here are at odds with each other over file extensions. Airbnb team seems adamant about not having any non standard code in a \`js\` file re this issue (#1235) and this pull https://github.com/airbnb/javascript/pull/985 and names any files with the stuff should have \`jsx\` Facebook team seems adamant that \`js\` should be the only extension no matter what is in the file https://github.com/facebook/create\-react\-app/issues/87#issuecomment\-234627904 For my (and probably others) future reference when setting up a new js repo with the \`eslint\-config\-airbnb\` \_Originally posted by @williscool in https://github.com/airbnb/javascript/issues/1235#issuecomment\-428413675\_

Create issue

### **[williscool](https://github.com/williscool)** commented [on 10 Oct 2018](#issuecomment-428413675)

|

It seems like the react team at facebook and the airbnb team that own the most popular javascript style guide here are at odds with each other over file extensions.

Airbnb team seems adamant about not having any non standard code in a `js` file re this issue ([#1235](https://github.com/airbnb/javascript/issues/1235))

and this pull [#985](https://github.com/airbnb/javascript/pull/985)

and names any files with the stuff should have `jsx`

Facebook team seems adamant that `js` should be the only extension no matter what is in the file

[facebook/create\-react\-app#87 (comment)](https://github.com/facebook/create-react-app/issues/87#issuecomment-234627904)

For my (and probably others) future reference when setting up a new js repo with the `eslint-config-airbnb`

 |
