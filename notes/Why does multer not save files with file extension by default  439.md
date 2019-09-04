---
tags: [GitHub]
title: 'Why does multer not save files with file extension by default? #439'
created: '2019-09-04T07:23:59.185Z'
modified: '2019-09-04T07:24:02.061Z'
---

# Why does multer not save files with file extension by default? #439

Open

[mezod](https://github.com/mezod) opened this issue on 8 Jan 2017 · 11 comments

Open

# [Why does multer not save files with file extension by default?](#) #439

[mezod](https://github.com/mezod) opened this issue on 8 Jan 2017 · 11 comments

## Comments

[![@mezod](https://avatars0.githubusercontent.com/u/1230963?s=88&v=4)](https://github.com/mezod)

Pick your reaction

  👍   👎   😄   🎉   😕   ❤️   🚀  👀 Copy link Quote reply

### **[mezod](https://github.com/mezod)** commented [on 8 Jan 2017](#issue-199379818)

|

What's the idea behind this decision? I'm asking because when serving the static files without file extension, for example, browsers cannot display images but they just download them. Seeing this is default I wonder how you serve the files then?

 |

👍 1

Pick your reaction

  👍   👎   😄   🎉   😕   ❤️   🚀  👀

[![@LinusU](https://avatars2.githubusercontent.com/u/189580?s=88&v=4)](https://github.com/LinusU)

### This comment has been minimized.

Show comment

Hide comment

Copy link Quote reply

[![@LinusU](https://avatars3.githubusercontent.com/u/189580?s=60&v=4)](https://github.com/LinusU)

#### **[LinusU](https://github.com/LinusU)** [on 8 Jan 2017](#issuecomment-271105916)

Member

Good question, this should absolutely be added to the readme. I guess that my current best answer is this comment that I made before: [#170 (comment)](https://github.com/expressjs/multer/issues/170#issuecomment-123402678).

In short, the client can send any file extension that they want, and blindly trusting that might be a bad idea. E.g. someone could upload a html file with some javascript, and give the `.jpg` extension. Since the server accepts images, it will accept this html file. When this file later is server, a "magic byte" or similar method is used to derive that this is an html file, and the file is served as such. The "attacker" now can run any arbitrary javascript under your domain name...

Pick your reaction

  👍   👎   😄   🎉   😕   ❤️   🚀  👀 Copy link Quote reply Reference in new issue

### Reference in new issue

Repository

 multer Repositories

Title

Body

Good question, this should absolutely be added to the readme. I guess that my current best answer is this comment that I made before: https://github.com/expressjs/multer/issues/170#issuecomment\-123402678. In short, the client can send any file extension that they want, and blindly trusting that might be a bad idea. E.g. someone could upload a html file with some javascript, and give the \`.jpg\` extension. Since the server accepts images, it will accept this html file. When this file later is server, a "magic byte" or similar method is used to derive that this is an html file, and the file is served as such. The "attacker" now can run any arbitrary javascript under your domain name... \_Originally posted by @LinusU in https://github.com/expressjs/multer/issues/439#issuecomment\-271105916\_

Create issue

Member

### **[LinusU](https://github.com/LinusU)** commented [on 8 Jan 2017](#issuecomment-271105916) •

edited

|

Good question, this should absolutely be added to the readme. I guess that my current best answer is this comment that I made before: [#170 (comment)](https://github.com/expressjs/multer/issues/170#issuecomment-123402678).

In short, the client can send any file extension that they want, and blindly trusting that might be a bad idea. E.g. someone could upload a html file with some javascript, and give the `.jpg` extension. Since the server accepts images, it will accept this html file. When this file later is server, a "magic byte" or similar method is used to derive that this is an html file, and the file is served as such. The "attacker" now can run any arbitrary javascript under your domain name...

 |

[![@mezod](https://avatars0.githubusercontent.com/u/1230963?s=88&v=4)](https://github.com/mezod)

### This comment has been minimized.

Show comment

Hide comment

Copy link Quote reply

[![@mezod](https://avatars1.githubusercontent.com/u/1230963?s=60&v=4)](https://github.com/mezod)

#### **[mezod](https://github.com/mezod)** [on 8 Jan 2017](#issuecomment-271116640)

Author

Hey Linus, thanks for your answer. I'm not sure if by linking to that answer you suggest to override the default behaviour by adding the file extension (even if its at our own risk of the problem you defined) or you completely disregard it.

I understand the problem and would like to keep it that way, but then, how am I supposed to serve such files? As they have no extension the library I'm using (koa\-static) seems to not know the type of the file and send the wrong headers, which ultimately results in the browsers autodownloading the files instead of displaying them. I understand this is something koa\-static should solve and not a general issue right?

Pick your reaction

  👍   👎   😄   🎉   😕   ❤️   🚀  👀 Copy link Quote reply Reference in new issue

### Reference in new issue

Repository

 multer Repositories

Title

Body

Hey Linus, thanks for your answer. I'm not sure if by linking to that answer you suggest to override the default behaviour by adding the file extension (even if its at our own risk of the problem you defined) or you completely disregard it. I understand the problem and would like to keep it that way, but then, how am I supposed to serve such files? As they have no extension the library I'm using (koa\-static) seems to not know the type of the file and send the wrong headers, which ultimately results in the browsers autodownloading the files instead of displaying them. I understand this is something koa\-static should solve and not a general issue right? \_Originally posted by @mezod in https://github.com/expressjs/multer/issues/439#issuecomment\-271116640\_

Create issue

Author

### **[mezod](https://github.com/mezod)** commented [on 8 Jan 2017](#issuecomment-271116640)

|

Hey Linus, thanks for your answer. I'm not sure if by linking to that answer you suggest to override the default behaviour by adding the file extension (even if its at our own risk of the problem you defined) or you completely disregard it.

I understand the problem and would like to keep it that way, but then, how am I supposed to serve such files? As they have no extension the library I'm using (koa\-static) seems to not know the type of the file and send the wrong headers, which ultimately results in the browsers autodownloading the files instead of displaying them. I understand this is something koa\-static should solve and not a general issue right?

 |

[![@LinusU](https://avatars2.githubusercontent.com/u/189580?s=88&v=4)](https://github.com/LinusU)

### This comment has been minimized.

Show comment

Hide comment

Copy link Quote reply

[![@LinusU](https://avatars3.githubusercontent.com/u/189580?s=60&v=4)](https://github.com/LinusU)

#### **[LinusU](https://github.com/LinusU)** [on 8 Jan 2017](#issuecomment-271142011)

Member

Hmm, I actually don't know exactly how koa\-static works, but I think that most static servers (apache, nginx) serves the Content\-Type based on the magic bytes 🤔

Okay, it turns out that serve\-static uses [`mime`](https://github.com/broofa/node-mime) which only looks at the file extension... That's unfortunate...

I would probably recommend you to use the [`file-type` library by sindresorhus](https://github.com/sindresorhus/file-type), it seems great and will actually look at the bytes instead of the client provided file extension.

You can use store the files to a temporary location on upload, then use the file\-type library to get a file extension, and then move the file into place.

Since this is a very broadly requested feature, I think that we maybe should add it to v2.x ([#399](https://github.com/expressjs/multer/pull/399)) in the form of a `detectedMimeType` and `detectedFileExtension`, what do you think?

Pick your reaction

  👍   👎   😄   🎉   😕   ❤️   🚀  👀 Copy link Quote reply Reference in new issue

### Reference in new issue

Repository

 multer Repositories

Title

Body

Hmm, I actually don't know exactly how koa\-static works, but I think that most static servers (apache, nginx) serves the Content\-Type based on the magic bytes 🤔 Okay, it turns out that serve\-static uses \[\`mime\`\](https://github.com/broofa/node\-mime) which only looks at the file extension... That's unfortunate... I would probably recommend you to use the \[\`file\-type\` library by sindresorhus\](https://github.com/sindresorhus/file\-type), it seems great and will actually look at the bytes instead of the client provided file extension. You can use store the files to a temporary location on upload, then use the file\-type library to get a file extension, and then move the file into place. Since this is a very broadly requested feature, I think that we maybe should add it to v2.x (#399) in the form of a \`detectedMimeType\` and \`detectedFileExtension\`, what do you think? \_Originally posted by @LinusU in https://github.com/expressjs/multer/issues/439#issuecomment\-271142011\_

Create issue

Member

### **[LinusU](https://github.com/LinusU)** commented [on 8 Jan 2017](#issuecomment-271142011) •

edited

|

Hmm, I actually don't know exactly how koa\-static works, but I think that most static servers (apache, nginx) serves the Content\-Type based on the magic bytes 🤔

Okay, it turns out that serve\-static uses [`mime`](https://github.com/broofa/node-mime) which only looks at the file extension... That's unfortunate...

I would probably recommend you to use the [`file-type` library by sindresorhus](https://github.com/sindresorhus/file-type), it seems great and will actually look at the bytes instead of the client provided file extension.

You can use store the files to a temporary location on upload, then use the file\-type library to get a file extension, and then move the file into place.

Since this is a very broadly requested feature, I think that we maybe should add it to v2.x ([#399](https://github.com/expressjs/multer/pull/399)) in the form of a `detectedMimeType` and `detectedFileExtension`, what do you think?

 |

[![@mezod](https://avatars0.githubusercontent.com/u/1230963?s=88&v=4)](https://github.com/mezod)

### This comment has been minimized.

Show comment

Hide comment

Copy link Quote reply

[![@mezod](https://avatars1.githubusercontent.com/u/1230963?s=60&v=4)](https://github.com/mezod)

#### **[mezod](https://github.com/mezod)** [on 8 Jan 2017](#issuecomment-271145517)

Author

I think it would be a great feature, even though I must admit that I barely know how all of these works nor the security risks involved so my opinion shouldn't really matter here :d I am just surprised of the default behaviour when putting multer together with koa\-static (serve\-static). I'd have expected everyone to have my needs! Thanks for your help, I'll try to do it the way you suggest!

Pick your reaction

  👍   👎   😄   🎉   😕   ❤️   🚀  👀 Copy link Quote reply Reference in new issue

### Reference in new issue

Repository

 multer Repositories

Title

Body

I think it would be a great feature, even though I must admit that I barely know how all of these works nor the security risks involved so my opinion shouldn't really matter here :d I am just surprised of the default behaviour when putting multer together with koa\-static (serve\-static). I'd have expected everyone to have my needs! Thanks for your help, I'll try to do it the way you suggest! \_Originally posted by @mezod in https://github.com/expressjs/multer/issues/439#issuecomment\-271145517\_

Create issue

Author

### **[mezod](https://github.com/mezod)** commented [on 8 Jan 2017](#issuecomment-271145517)

|

I think it would be a great feature, even though I must admit that I barely know how all of these works nor the security risks involved so my opinion shouldn't really matter here :d I am just surprised of the default behaviour when putting multer together with koa\-static (serve\-static). I'd have expected everyone to have my needs! Thanks for your help, I'll try to do it the way you suggest!

 |

[![@DiegoRBaquero](https://avatars1.githubusercontent.com/u/2006839?s=88&v=4)](https://github.com/DiegoRBaquero)

### This comment has been minimized.

Show comment

Hide comment

Copy link Quote reply

[![@DiegoRBaquero](https://avatars0.githubusercontent.com/u/2006839?s=60&v=4)](https://github.com/DiegoRBaquero)

#### **[DiegoRBaquero](https://github.com/DiegoRBaquero)** [on 31 Jan 2017](#issuecomment-276255945)

You can keep original file name and extension uploaded:

```js
const multer = require('multer')
const storage = multer.diskStorage({
  destination: function (req, file, cb) {
    cb(null, 'uploads/')
  },
  filename: function (req, file, cb) {
    cb(null, file.originalname)
  }
})
const upload = multer({storage: storage})
```

Pick your reaction

  👍   👎   😄   🎉   😕   ❤️   🚀  👀 Copy link Quote reply Reference in new issue

### Reference in new issue

Repository

 multer Repositories

Title

Body

You can keep original file name and extension uploaded: \`\`\`js const multer = require('multer') const storage = multer.diskStorage({ destination: function (req, file, cb) { cb(null, 'uploads/') }, filename: function (req, file, cb) { cb(null, file.originalname) } }) const upload = multer({storage: storage}) \`\`\` \_Originally posted by @DiegoRBaquero in https://github.com/expressjs/multer/issues/439#issuecomment\-276255945\_

Create issue

### **[DiegoRBaquero](https://github.com/DiegoRBaquero)** commented [on 31 Jan 2017](#issuecomment-276255945)

|

You can keep original file name and extension uploaded:

```js
const multer = require('multer')
const storage = multer.diskStorage({
  destination: function (req, file, cb) {
    cb(null, 'uploads/')
  },
  filename: function (req, file, cb) {
    cb(null, file.originalname)
  }
})
const upload = multer({storage: storage})
```

 |

👍 18 🎉 8 ❤️ 4 🚀 1

Pick your reaction

  👍   👎   😄   🎉   😕   ❤️   🚀  👀

[![@abhishekdgeek](https://avatars2.githubusercontent.com/u/6406766?s=60&v=4)](https://github.com/abhishekdgeek) [abhishekdgeek](https://github.com/abhishekdgeek) referenced this issue [on 15 Mar 2017](#ref-issue-214313328)

[No way to enable/disable saving files with extensions. #469](https://github.com/expressjs/multer/issues/469)

Open

[![@fasterthan](https://avatars3.githubusercontent.com/u/13704150?s=88&v=4)](https://github.com/fasterthan)

### This comment has been minimized.

Show comment

Hide comment

Copy link Quote reply

[![@fasterthan](https://avatars2.githubusercontent.com/u/13704150?s=60&v=4)](https://github.com/fasterthan)

#### **[fasterthan](https://github.com/fasterthan)** [on 24 Apr 2018](#issuecomment-383734378)

It would be convenient if this original file name snippet was included in the readme

Pick your reaction

  👍   👎   😄   🎉   😕   ❤️   🚀  👀 Copy link Quote reply Reference in new issue

### Reference in new issue

Repository

 multer Repositories

Title

Body

It would be convenient if this original file name snippet was included in the readme \_Originally posted by @fasterthan in https://github.com/expressjs/multer/issues/439#issuecomment\-383734378\_

Create issue

### **[fasterthan](https://github.com/fasterthan)** commented [on 24 Apr 2018](#issuecomment-383734378)

|

It would be convenient if this original file name snippet was included in the readme

 |

[![@LinusU](https://avatars2.githubusercontent.com/u/189580?s=88&v=4)](https://github.com/LinusU)

### This comment has been minimized.

Show comment

Hide comment

Copy link Quote reply

[![@LinusU](https://avatars3.githubusercontent.com/u/189580?s=60&v=4)](https://github.com/LinusU)

#### **[LinusU](https://github.com/LinusU)** [on 24 Apr 2018](#issuecomment-383809254)

Member

> It would be convenient if this original file name snippet was included in the readme

I would prefer not to do that since I don't think it's a good practice at all. It would be trivial for anyone to overwrite any file on the server by sending that name up. Also, it would cause problems if a user happens to upload a file with the same name as another file.

[@fasterthan](https://github.com/fasterthan) would you mind explaining your use case? I could give you a recommendation on how to solve it without the problems I just mentioned...

Pick your reaction

  👍   👎   😄   🎉   😕   ❤️   🚀  👀 Copy link Quote reply Reference in new issue

### Reference in new issue

Repository

 multer Repositories

Title

Body

\> It would be convenient if this original file name snippet was included in the readme I would prefer not to do that since I don't think it's a good practice at all. It would be trivial for anyone to overwrite any file on the server by sending that name up. Also, it would cause problems if a user happens to upload a file with the same name as another file. @fasterthan would you mind explaining your use case? I could give you a recommendation on how to solve it without the problems I just mentioned... \_Originally posted by @LinusU in https://github.com/expressjs/multer/issues/439#issuecomment\-383809254\_

Create issue

Member

### **[LinusU](https://github.com/LinusU)** commented [on 24 Apr 2018](#issuecomment-383809254)

|

> It would be convenient if this original file name snippet was included in the readme

I would prefer not to do that since I don't think it's a good practice at all. It would be trivial for anyone to overwrite any file on the server by sending that name up. Also, it would cause problems if a user happens to upload a file with the same name as another file.

[@fasterthan](https://github.com/fasterthan) would you mind explaining your use case? I could give you a recommendation on how to solve it without the problems I just mentioned...

 |

[![@dannysofftie](https://avatars0.githubusercontent.com/u/17042186?s=88&v=4)](https://github.com/dannysofftie)

### This comment has been minimized.

Show comment

Hide comment

Copy link Quote reply

[![@dannysofftie](https://avatars1.githubusercontent.com/u/17042186?s=60&v=4)](https://github.com/dannysofftie)

#### **[dannysofftie](https://github.com/dannysofftie)** [on 6 Jul 2018](#issuecomment-402827468)

I used this little trick to get file extension, and as a workaround for what [@LinusU](https://github.com/LinusU) mentioned

> It would be trivial for anyone to overwrite any file on the server by sending that name up. Also, it would cause problems if a user happens to upload a file with the same name as another file.

```js
const crypto = require('crypto')
let upload = multer({
    storage: multer.diskStorage({
       destination: (req, file, cb) => {
          cb(null, path.join(__dirname, '../uploads'))
     },
     filename: (req, file, cb) => {
        let customFileName = crypto.randomBytes(18).toString('hex'),
            fileExtension = file.originalname.split('.')[1] // get file extension from original file name
            cb(null, customFileName + '.' + fileExtension)
         }
      })
})

```

Pick your reaction

  👍   👎   😄   🎉   😕   ❤️   🚀  👀 Copy link Quote reply Reference in new issue

### Reference in new issue

Repository

 multer Repositories

Title

Body

I used this little trick to get file extension, and as a workaround for what @LinusU mentioned > It would be trivial for anyone to overwrite any file on the server by sending that name up. Also, it would cause problems if a user happens to upload a file with the same name as another file. \`\`\`js const crypto = require('crypto') let upload = multer({ storage: multer.diskStorage({ destination: (req, file, cb) => { cb(null, path.join(\_\_dirname, '../uploads')) }, filename: (req, file, cb) => { let customFileName = crypto.randomBytes(18).toString('hex'), fileExtension = file.originalname.split('.')\[1\] // get file extension from original file name cb(null, customFileName + '.' + fileExtension) } }) }) \`\`\` \_Originally posted by @dannysofftie in https://github.com/expressjs/multer/issues/439#issuecomment\-402827468\_

Create issue

### **[dannysofftie](https://github.com/dannysofftie)** commented [on 6 Jul 2018](#issuecomment-402827468) •

edited

|

I used this little trick to get file extension, and as a workaround for what [@LinusU](https://github.com/LinusU) mentioned

> It would be trivial for anyone to overwrite any file on the server by sending that name up. Also, it would cause problems if a user happens to upload a file with the same name as another file.

```js
const crypto = require('crypto')
let upload = multer({
    storage: multer.diskStorage({
       destination: (req, file, cb) => {
          cb(null, path.join(__dirname, '../uploads'))
     },
     filename: (req, file, cb) => {
        let customFileName = crypto.randomBytes(18).toString('hex'),
            fileExtension = file.originalname.split('.')[1] // get file extension from original file name
            cb(null, customFileName + '.' + fileExtension)
         }
      })
})

```

 |

🎉 1

Pick your reaction

  👍   👎   😄   🎉   😕   ❤️   🚀  👀

[![@mizozobu](https://avatars0.githubusercontent.com/u/27652080?s=88&v=4)](https://github.com/mizozobu)

### This comment has been minimized.

Show comment

Hide comment

Copy link Quote reply

[![@mizozobu](https://avatars1.githubusercontent.com/u/27652080?s=60&v=4)](https://github.com/mizozobu)

#### **[mizozobu](https://github.com/mizozobu)** [on 6 Oct 2018](#issuecomment-427553586)

You can get the extension of the uploaded file with `path.extname(file.originalname)` as well.

Pick your reaction

  👍   👎   😄   🎉   😕   ❤️   🚀  👀 Copy link Quote reply Reference in new issue

### Reference in new issue

Repository

 multer Repositories

Title

Body

You can get the extension of the uploaded file with \`path.extname(file.originalname)\` as well. \_Originally posted by @mizozobu in https://github.com/expressjs/multer/issues/439#issuecomment\-427553586\_

Create issue

### **[mizozobu](https://github.com/mizozobu)** commented [on 6 Oct 2018](#issuecomment-427553586)

|

You can get the extension of the uploaded file with `path.extname(file.originalname)` as well.

 |

[![@bikevit2008](https://avatars3.githubusercontent.com/u/6545120?s=88&v=4)](https://github.com/bikevit2008)

### This comment has been minimized.

Show comment

Hide comment

Copy link Quote reply

[![@bikevit2008](https://avatars2.githubusercontent.com/u/6545120?s=60&v=4)](https://github.com/bikevit2008)

#### **[bikevit2008](https://github.com/bikevit2008)** [on 24 Apr](#issuecomment-486023942)

> Hmm, I actually don't know exactly how koa\-static works, but I think that most static servers (apache, nginx) serves the Content\-Type based on the magic bytes 🤔
>
> Okay, it turns out that serve\-static uses [`mime`](https://github.com/broofa/node-mime) which only looks at the file extension... That's unfortunate...
>
> I would probably recommend you to use the [`file-type` library by sindresorhus](https://github.com/sindresorhus/file-type), it seems great and will actually look at the bytes instead of the client provided file extension.
>
> You can use store the files to a temporary location on upload, then use the file\-type library to get a file extension, and then move the file into place.
>
> Since this is a very broadly requested feature, I think that we maybe should add it to v2.x ([#399](https://github.com/expressjs/multer/pull/399)) in the form of a `detectedMimeType` and `detectedFileExtension`, what do you think?

Hi, i think it's not correctly solution, because if i want filtering files on upload and reject files with disallowed extensions, but i can't, when i use DiskStorage. Thank you the library file\-type, it's very useful for me.
I see now only one possible choose for me, but i think it's bad variant: i need use MemoryStorage then get Buffer, then check real file\-type and save to fs, and after delete buffer from Memory. But i think it's too bad, because i need do identity functional like multer. What do you think about it?

Pick your reaction

  👍   👎   😄   🎉   😕   ❤️   🚀  👀 Copy link Quote reply Reference in new issue

### Reference in new issue

Repository

 multer Repositories

Title

Body

\> Hmm, I actually don't know exactly how koa\-static works, but I think that most static servers (apache, nginx) serves the Content\-Type based on the magic bytes 🤔 > > Okay, it turns out that serve\-static uses \[\`mime\`\](https://github.com/broofa/node\-mime) which only looks at the file extension... That's unfortunate... > > I would probably recommend you to use the \[\`file\-type\` library by sindresorhus\](https://github.com/sindresorhus/file\-type), it seems great and will actually look at the bytes instead of the client provided file extension. > > You can use store the files to a temporary location on upload, then use the file\-type library to get a file extension, and then move the file into place. > > Since this is a very broadly requested feature, I think that we maybe should add it to v2.x (#399) in the form of a \`detectedMimeType\` and \`detectedFileExtension\`, what do you think? Hi, i think it's not correctly solution, because if i want filtering files on upload and reject files with disallowed extensions, but i can't, when i use DiskStorage. Thank you the library file\-type, it's very useful for me. I see now only one possible choose for me, but i think it's bad variant: i need use MemoryStorage then get Buffer, then check real file\-type and save to fs, and after delete buffer from Memory. But i think it's too bad, because i need do identity functional like multer. What do you think about it? \_Originally posted by @bikevit2008 in https://github.com/expressjs/multer/issues/439#issuecomment\-486023942\_

Create issue

### **[bikevit2008](https://github.com/bikevit2008)** commented [on 24 Apr](#issuecomment-486023942)

|

> Hmm, I actually don't know exactly how koa\-static works, but I think that most static servers (apache, nginx) serves the Content\-Type based on the magic bytes 🤔
>
> Okay, it turns out that serve\-static uses [`mime`](https://github.com/broofa/node-mime) which only looks at the file extension... That's unfortunate...
>
> I would probably recommend you to use the [`file-type` library by sindresorhus](https://github.com/sindresorhus/file-type), it seems great and will actually look at the bytes instead of the client provided file extension.
>
> You can use store the files to a temporary location on upload, then use the file\-type library to get a file extension, and then move the file into place.
>
> Since this is a very broadly requested feature, I think that we maybe should add it to v2.x ([#399](https://github.com/expressjs/multer/pull/399)) in the form of a `detectedMimeType` and `detectedFileExtension`, what do you think?

Hi, i think it's not correctly solution, because if i want filtering files on upload and reject files with disallowed extensions, but i can't, when i use DiskStorage. Thank you the library file\-type, it's very useful for me.
I see now only one possible choose for me, but i think it's bad variant: i need use MemoryStorage then get Buffer, then check real file\-type and save to fs, and after delete buffer from Memory. But i think it's too bad, because i need do identity functional like multer. What do you think about it?

 |

[![@mathewk2017](https://avatars1.githubusercontent.com/u/31842995?s=88&v=4)](https://github.com/mathewk2017)

### This comment has been minimized.

Show comment

Hide comment

Copy link Quote reply

[![@mathewk2017](https://avatars0.githubusercontent.com/u/31842995?s=60&v=4)](https://github.com/mathewk2017)

#### **[mathewk2017](https://github.com/mathewk2017)** [on 10 Jul](#issuecomment-509955995)

if I use diskStorage, how can give dynamic destinations?

Pick your reaction

  👍   👎   😄   🎉   😕   ❤️   🚀  👀 Copy link Quote reply Reference in new issue

### Reference in new issue

Repository

 multer Repositories

Title

Body

if I use diskStorage, how can give dynamic destinations? \_Originally posted by @mathewk2017 in https://github.com/expressjs/multer/issues/439#issuecomment\-509955995\_

Create issue

### **[mathewk2017](https://github.com/mathewk2017)** commented [on 10 Jul](#issuecomment-509955995)

|

if I use diskStorage, how can give dynamic destinations?

 |
