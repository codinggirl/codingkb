---
title: 为您详细比较三个 CSS 预处理器（框架）：Sass、LESS 和 Stylus
created: '2019-09-26T07:26:02.960Z'
modified: '2019-09-26T07:27:05.091Z'
---

## 为您详细比较三个 CSS 预处理器（框架）：Sass、LESS 和 Stylus


# [sass/scss 和 less的区别](https://www.cnblogs.com/wangpenghui522/p/5467560.html)

### 一. Sass/Scss、Less是什么?

Sass (Syntactically Awesome Stylesheets)是一种动态样式语言，Sass语法属于缩排语法，比css比多出好些功能(如变量、嵌套、运算,混入(Mixin)、继承、颜色处理，函数等)，更容易阅读。

**Sass与Scss是什么关系?**

Sass的缩排语法，对于写惯css前端的web开发者来说很不直观，也不能将css代码加入到Sass里面，因此sass语法进行了改良，Sass 3就变成了Scss(sassy css)。与原来的语法兼容，只是用{}取代了原来的缩进。

Less也是一种动态样式语言. 对CSS赋予了动态语言的特性，如变量，继承，运算， 函数.  Less 既可以在客户端上运行 (支持IE 6+, Webkit, Firefox)，也可在服务端运行 (借助 Node.js)。

### 二. Sass/Scss与Less区别

### 1.编译环境不一样

Sass的安装需要Ruby环境，是在服务端处理的，而Less是需要引入less.js来处理Less代码输出css到浏览器，也可以在开发环节使用Less，然后编译成css文件，直接放到项目中，也有 Less.app、SimpleLess、CodeKit.app这样的工具，也有在线编译地址。

### 2.变量符不一样，Less是@，而Scss是$，而且变量的作用域也不一样。

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0); "复制代码")

Less\-作用域
@color: #00c; /\* 蓝色 \*/ #header { @color: #c00; /\* red \*/ border: 1px solid @color; /\* 红色边框 \*/
} #footer { border: 1px solid @color; /\* 蓝色边框 \*/
} Less\-作用域编译后
#header{border:1px solid #cc0000;} #footer{border:1px solid #0000cc;} scss\-作用域
$color: #00c; /\* 蓝色 \*/ #header { $color: #c00; /\* red \*/ border: 1px solid $color; /\* 红色边框 \*/
} #footer { border: 1px solid $color; /\* 蓝色边框 \*/
} Sass\-作用域编译后

#header{border:1px solid #c00} #footer{border:1px solid #c00} 我们可以看出来，less和scss中的变量会随着作用域的变化而不一样。

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0); "复制代码")

### 3.输出设置，Less没有输出设置，Sass提供4中输出选项：nested, compact, compressed 和 expanded。

输出样式的风格可以有四种选择，默认为nested

*   nested：嵌套缩进的css代码
*   expanded：展开的多行css代码
*   compact：简洁格式的css代码
*   compressed：压缩后的css代码

#### 4.Sass支持条件语句，可以使用if{}else{},for{}循环等等。而Less不支持。

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0); "复制代码")

/\* Sample Sass “if” statement \*/ @if lightness($color) > 30% {

} @else {

}

/\* Sample Sass “for” loop \*/ @for $i from 1 to 10 { .border\-#{$i} { border: #{$i}px solid blue;
  }
}

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0); "复制代码")

###  5. 引用外部CSS文件

scss引用的外部文件命名必须以\_开头, 如下例所示:其中\_test1.scss、\_test2.scss、\_test3.scss文件分别设置的h1 h2 h3。文件名如果以下划线\_开头的话，Sass会认为该文件是一个引用文件，不会将其编译为css文件.

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0); "复制代码")

 // 源代码：
@import "\_test1.scss";
@import "\_test2.scss";
@import "\_test3.scss";

// 编译后：
h1 { font\-size: 17px;
} h2 { font\-size: 17px;
} h3 { font\-size: 17px;
}

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0); "复制代码")

Less引用外部文件和css中的@import没什么差异。

### 6.Sass和Less的工具库不同

Sass有工具库Compass, 简单说，Sass和Compass的关系有点像Javascript和jQuery的关系,Compass是Sass的工具库。在它的基础上，封装了一系列有用的模块和模板，补充强化了Sass的功能。

Less有UI组件库Bootstrap,Bootstrap是web前端开发中一个比较有名的前端UI组件库，Bootstrap的样式文件部分源码就是采用Less语法编写。

### 三. 总结

不管是Sass，还是Less，都可以视为一种基于CSS之上的高级语言，其目的是使得CSS开发更灵活和更强大，Sass的功能比Less强大,基本可以说是一种真正的编程语言了，Less则相对清晰明了,易于上手,对编译环境要求比较宽松。考虑到编译Sass要安装Ruby,而Ruby官网在国内访问不了,个人在实际开发中更倾向于选择Less。




[红薯](https://my.oschina.net/javayou) 发布于 2012/03/13 22:29

阅读 145K+

收藏 321

[评论 58](#comments)

[![](https://static.oschina.net/img/logo/lesscss.gif)Less.js](https://www.oschina.net/question/tag/lesscss) [![](https://static.oschina.net/img/logo/sass.png)Sass](https://www.oschina.net/question/tag/sass) [![](https://www.oschina.net/question/12_44255)Stylus](https://www.oschina.net/question/tag/stylus) [OSCHINA原创翻译](https://www.oschina.net/question/tag/OSCHINA%E5%8E%9F%E5%88%9B%E7%BF%BB%E8%AF%91)

[**为什么80%的码农都做不了架构师？\->>>**](https://www.oschina.net/question/2663968_2311322
            )   ![](https://www.oschina.net/img/hot3.png )

CSS 预处理器技术已经非常的成熟，而且也涌现出了越来越多的 CSS 的预处理器框架。本文向你介绍使用最为普遍的三款 CSS 预处理器框架，分别是 Sass、Less CSS、Stylus。

首先我们来简单介绍下什么是 CSS 预处理器，CSS 预处理器是一种语言用来为 CSS 增加一些编程的的特性，无需考虑浏览器的兼容性问题，例如你可以在 CSS 中使用变量、简单的程序逻辑、函数等等在编程语言中的一些基本技巧，可以让你的 CSS 更见简洁，适应性更强，代码更直观等诸多好处。

不要再停留在石器时代了，下面让我们开始 CSS 预处理器之旅。

我们将会从语法、变量、嵌套、混入(Mixin)、继承、导入、函数和操作符等方面分别对这三个框架进行比较介绍。

语法

在使用 CSS 预处理器之前最重要的是理解语法，幸运的是基本上大多数预处理器的语法跟 CSS 都差不多。

首先 Sass 和 Less 都使用的是标准的 CSS 语法，因此如果你可以很方便的将已有的 CSS 代码转为预处理器代码，默认 Sass 使用 .sass 扩展名，而 Less 使用 .less 扩展名。

下面是这二者的语法：

/\* style.scss or style.less \*/
h1 {
  color: #0982C1;
}

你注意到了，这是一个再普通不过的，不过 Sass 同时也支持老的语法，就是不包含花括号和分号的方式：

/\* style.sass \*/
h1
  color: #0982c1

而 Stylus 支持的语法要更多样性一点，它默认使用 .styl 的文件扩展名，下面是 Stylus 支持的语法：

/\* style.styl \*/
h1 {
  color: #0982C1;
}

/\* omit brackets \*/
h1
  color: #0982C1;

/\* omit colons and semi\-colons \*/
h1
  color #0982C1

你也可以在同一个样式单中使用不同的变量，例如下面的写法也不会报错：

h1 {
  color #0982c1
}
h2
  font\-size: 1.2em

变量

你可以在 CSS 预处理器中声明变量，并在整个样式单中使用，支持任何类型的变量，例如颜色、数值（不管是否包括单位）、文本。然后你可以任意引用该变量。

Sass 的变量必须是 $ 开始，然后变量名和值使用冒号隔开，跟 CSS 的属性一致：

$mainColor: #0982c1;
$siteWidth: 1024px;
$borderStyle: dotted;

body {
  color: $mainColor;
  border: 1px $borderStyle $mainColor;
  max\-width: $siteWidth;
}

而 Less 的变量名使用 @ 符号开始：

@mainColor: #0982c1;
@siteWidth: 1024px;
@borderStyle: dotted;

body {
  color: @mainColor;
  border: 1px @borderStyle @mainColor;
  max\-width: @siteWidth;
}

Stylus 对变量名没有任何限定，你可以是 $ 开始，也可以是任意的字符，而且与变量值之间可以用冒号、空格隔开，需要注意的是 Stylus (0.22.4) 将会编译 @ 开始的变量，但其对应的值并不会赋予该变量，换句话说，在 Stylus 的变量名不要用 @ 开头。

mainColor = #0982c1
siteWidth = 1024px
$borderStyle = dotted

body
  color mainColor
  border 1px $borderStyle mainColor
  max\-width siteWidth

上面的三种不同的 CSS 预处理器的写法，最终都将产生相同的结果：

body {
  color: #0982c1;
  border: 1px dotted #0982c1;
  max\-width: 1024px;
}

你可以想象，加入你的 CSS 中使用了某个颜色的地方多达数十次，那么要修改颜色时你必须找到这数十次的地方并一一修改，而有了 CSS 预处理器，修改一个地方就够了！

嵌套

如果我们需要在CSS中相同的 parent 引用多个元素，这将是非常乏味的，你需要一遍又一遍地写 parent。例如：

section {
  margin: 10px;
}
section nav {
  height: 25px;
}
section nav a {
  color: #0982C1;
}
section nav a:hover {
  text\-decoration: underline;
}

而如果用 CSS 预处理器，就可以少些很多单词，而且父子节点关系一目了然。我们这里提到的三个 CSS 框架都是允许嵌套语法：

section {
  margin: 10px;

  nav {
    height: 25px;

    a {
      color: #0982C1;

      &amp;:hover {
        text\-decoration: underline;
      }
    }
  }
}

最终生成的 CSS 结果是：

section {
  margin: 10px;
}
section nav {
  height: 25px;
}
section nav a {
  color: #0982C1;
}
section nav a:hover {
  text\-decoration: underline;
}

非常之方便！

Mixins (混入)

Mixins 有点像是函数或者是宏，当你某段 CSS 经常需要在多个元素中使用时，你可以为这些共用的 CSS 定义一个 Mixin，然后你只需要在需要引用这些 CSS 地方调用该 Mixin 即可。

Sass 的混入语法：

/\* Sass mixin error with (optional) argument $borderWidth which defaults to 2px if not specified \*/
@mixin error($borderWidth: 2px) {
  border: $borderWidth solid #F00;
  color: #F00;
}

.generic\-error {
  padding: 20px;
  margin: 4px;
  @ include error(); /\* Applies styles from mixin error \*/
}
.login\-error {
  left: 12px;
  position: absolute;
  top: 20px;
  @ include error(5px); /\* Applies styles from mixin error with argument $borderWidth equal to 5px\*/
}

Less CSS 的混入语法：

/\* LESS mixin error with (optional) argument @borderWidth which defaults to 2px if not specified \*/
.error(@borderWidth: 2px) {
  border: @borderWidth solid #F00;
  color: #F00;
}

.generic\-error {
  padding: 20px;
  margin: 4px;
  .error(); /\* Applies styles from mixin error \*/
}
.login\-error {
  left: 12px;
  position: absolute;
  top: 20px;
  .error(5px); /\* Applies styles from mixin error with argument @borderWidth equal to 5px \*/
}

Stylus 的混入语法：

/\* Stylus mixin error with (optional) argument borderWidth which defaults to 2px if not specified \*/
error(borderWidth= 2px) {
  border: borderWidth solid #F00;
  color: #F00;
}

.generic\-error {
  padding: 20px;
  margin: 4px;
  error(); /\* Applies styles from mixin error \*/
}
.login\-error {
  left: 12px;
  position: absolute;
  top: 20px;
  error(5px); /\* Applies styles from mixin error with argument borderWidth equal to 5px \*/
}

最终它们都将编译成如下的 CSS 样式：

.generic\-error {
  padding: 20px;
  margin: 4px;
  border: 2px solid #f00;
  color: #f00;
}
.login\-error {
  left: 12px;
  position: absolute;
  top: 20px;
  border: 5px solid #f00;
  color: #f00;
}

继承

当我们需要为多个元素定义相同样式的时候，我们可以考虑使用继承的做法。例如我们经常需要：

p,
ul,
ol {
  /\* styles here \*/
}

在 Sass 和 Stylus 我们可以这样写：

.block {
  margin: 10px 5px;
  padding: 2px;
}

p {
  @extend .block; /\* Inherit styles from '.block' \*/
  border: 1px solid #EEE;
}
ul, ol {
  @extend .block; /\* Inherit styles from '.block' \*/
  color: #333;
  text\-transform: uppercase;
}

在这里首先定义 .block 块，然后让 p 、ul 和 ol 元素继承 .block ，最终生成的 CSS 如下：

.block, p, ul, ol {
  margin: 10px 5px;
  padding: 2px;
}
p {
  border: 1px solid #EEE;
}
ul, ol {
  color: #333;
  text\-transform: uppercase;
}

在这方面 Less 表现的稍微弱一些，更像是混入写法：

.block {
  margin: 10px 5px;
  padding: 2px;
}

p {
  .block; /\* Inherit styles from '.block' \*/
  border: 1px solid #EEE;
}
ul, ol {
  .block; /\* Inherit styles from '.block' \*/
  color: #333;
  text\-transform: uppercase;
}

生成的 CSS 如下：

.block {
  margin: 10px 5px;
  padding: 2px;
}
p {
  margin: 10px 5px;
  padding: 2px;
  border: 1px solid #EEE;
}
ul,
ol {
  margin: 10px 5px;
  padding: 2px;
  color: #333;
  text\-transform: uppercase;
}

你所看到的上面的代码中，.block 的样式将会被插入到相应的你想要继承的选择器中，但需要注意的是优先级的问题。

导入 (Import)

很多 CSS 开发者对导入的做法都不太感冒，因为它需要多次的 HTTP 请求。但是在 CSS 预处理器中的导入操作则不同，它只是在语义上包含了不同的文件，但最终结果是一个单一的 CSS 文件，如果你是通过 `@ import "file.css ";` 导入 CSS 文件，那效果跟普通的 CSS 导入一样。注意：导入文件中定义的混入、变量等信息也将会被引入到主样式文件中，因此需要避免它们互相冲突。

reset.css:

/\* file.{type} \*/
body {
  background: #EEE;
}

main.xxx:

@ import "reset.css ";
@ import "file.{type} ";

p {
  background: #0982C1;
}

最终生成的 CSS：

@ import "reset.css ";
body {
  background: #EEE;
}
p {
  background: #0982C1;
}

颜色函数

CSS 预处理器一般都会内置一些颜色处理函数用来对颜色值进行处理，例如加亮、变暗、颜色梯度等。

Sass：

lighten($color, 10%); /\* returns a color 10% lighter than $color \*/
darken($color, 10%);  /\* returns a color 10% darker than $color \*/
 saturate($color, 10%);   /\* returns a color 10% more saturated than $color \*/
desaturate($color, 10%); /\* returns a color 10% less saturated than $color \*/
 grayscale($color);  /\* returns grayscale of $color \*/
complement($color); /\* returns complement color of $color \*/
invert($color);     /\* returns inverted color of $color \*/
 mix($color1, $color2, 50%); /\* mix $color1 with $color2 with a weight of 50% \*/

上面只是简单列了 Sass 的一些基本颜色处理函数，完整的列表请看 [Sass Documentation](http://sass-lang.com/docs/yardoc/Sass/Script/Functions.html ).

下面是一个具体的例子：

$color: #0982C1;

h1 {
  background: $color;
  border: 3px solid darken($color, 50%);
}

Less CSS:

lighten(@color, 10%); /\* returns a color 10% lighter than @color \*/
darken(@color, 10%);  /\* returns a color 10% darker than @color \*/

saturate(@color, 10%);   /\* returns a color 10% more saturated than @color \*/
desaturate(@color, 10%); /\* returns a color 10% less saturated than @color \*/

spin(@color, 10);  /\* returns a color with a 10 degree larger in hue than @color \*/
spin(@color, \-10); /\* returns a color with a 10 degree smaller hue than @color \*/

mix(@color1, @color2); /\* return a mix of @color1 and @color2 \*/

LESS 完整的颜色函数列表请看 [LESS Documentation](http://lesscss.org/#-color-functions ).

LESS 使用颜色函数的例子：

@color: #0982C1;

h1 {
  background: @color;
  border: 3px solid darken(@color, 50%);
}

Stylus:

lighten(color, 10%); /\* returns a color 10% lighter than 'color' \*/
darken(color, 10%);  /\* returns a color 10% darker than 'color' \*/

saturate(color, 10%);   /\* returns a color 10% more saturated than 'color' \*/
desaturate(color, 10%); /\* returns a color 10% less saturated than 'color' \*/

完整的颜色函数列表请阅读 [Stylus Documentation](http://learnboost.github.com/stylus/docs/bifs.html ).

实例：

color = #0982C1

h1
  background color
  border 3px solid darken(color, 50%)

运算符

你可以直接在 CSS 预处理器中进行样式的计算，例如：

body {
  margin: (14px/2);
  top: 50px + 100px;
  right: 100px \- 50px;
  left: 10 \* 10;
}

一些跟具体浏览器相关的处理

这是宣传使用预处理的原因之一，并且是一个很好的理由，这样可以节省的大量的时间和汗水。创建一个mixin来处理不同浏览器的CSS写法是很简单的，节省了大量的重复工作和痛苦的代码编辑。

Sass

@mixin border\-radius($values) {
  \-webkit\-border\-radius: $values;
     \-moz\-border\-radius: $values;
          border\-radius: $values;
}

div {
  @ include border\-radius(10px);
}

Less CSS

.border\-radius(@values) {
  \-webkit\-border\-radius: @values;
     \-moz\-border\-radius: @values;
          border\-radius: @values;
}

div {
  .border\-radius(10px);
}

Stylus

border\-radius(values) {
  \-webkit\-border\-radius: values;
     \-moz\-border\-radius: values;
          border\-radius: values;
}

div {
  border\-radius(10px);
}

编译结果：

div {
  \-webkit\-border\-radius: 10px;
     \-moz\-border\-radius: 10px;
          border\-radius: 10px;
}

3D文本

要生成具有 3D 效果的文本可以使用 `text-shadows` ，唯一的问题就是当要修改颜色的时候就非常的麻烦，而通过 mixin 和颜色函数可以很轻松的实现：

Sass

@mixin text3d($color) {
  color: $color;
  text\-shadow: 1px 1px 0px darken($color, 5%),
               2px 2px 0px darken($color, 10%),
               3px 3px 0px darken($color, 15%),
               4px 4px 0px darken($color, 20%),
               4px 4px 2px #000;
}

h1 {
  font\-size: 32pt;
  @ include text3d(#0982c1);
}

Less CSS

.text3d(@color) {
  color: @color;
  text\-shadow: 1px 1px 0px darken(@color, 5%),
               2px 2px 0px darken(@color, 10%),
               3px 3px 0px darken(@color, 15%),
               4px 4px 0px darken(@color, 20%),
               4px 4px 2px #000;
}

span {
  font\-size: 32pt;
  .text3d(#0982c1);
}

Stylus

text3d(color)
  color: color
  text\-shadow: 1px 1px 0px darken(color, 5%), 2px 2px 0px darken(color, 10%), 3px 3px 0px darken(color, 15%), 4px 4px 0px darken(color, 20%), 4px 4px 2px #000
span
  font\-size: 32pt
  text3d(#0982c1)

生成的 CSS

span {
  font\-size: 32pt;
  color: #0982c1;
  text\-shadow: 1px 1px 0px #097bb7,
               2px 2px 0px #0875ae,
               3px 3px 0px #086fa4,
               4px 4px 0px #07689a,
               4px 4px 2px #000;
}

效果图：

![](http://static.oschina.net/uploads/img/201203/13222932_fIcm.png )

列 (Columns)

使用数值操作和变量可以很方便的实现适应屏幕大小的布局处理。

Sass

$siteWidth: 1024px;
$gutterWidth: 20px;
$sidebarWidth: 300px;

body {
  margin: 0 auto;
  width: $siteWidth;
}
.content {
  float: left;
  width: $siteWidth \- ($sidebarWidth+$gutterWidth);
}
.sidebar {
  float: left;
  margin\-left: $gutterWidth;
  width: $sidebarWidth;
}

Less CSS

@siteWidth: 1024px;
@gutterWidth: 20px;
@sidebarWidth: 300px;

body {
  margin: 0 auto;
  width: @siteWidth;
}
.content {
  float: left;
  width: @siteWidth \- (@sidebarWidth+@gutterWidth);
}
.sidebar {
  float: left;
  margin\-left: @gutterWidth;
  width: @sidebarWidth;
}

Stylus

siteWidth = 1024px;
gutterWidth = 20px;
sidebarWidth = 300px;

body {
  margin: 0 auto;
  width: siteWidth;
}
.content {
  float: left;
  width: siteWidth \- (sidebarWidth+gutterWidth);
}
.sidebar {
  float: left;
  margin\-left: gutterWidth;
  width: sidebarWidth;
}

实际效果

body {
  margin: 0 auto;
  width: 1024px;
}
.content {
  float: left;
  width: 704px;
}
.sidebar {
  float: left;
  margin\-left: 20px;
  width: 300px;
}

错误报告

如果你经常 CSS 你会发现很难找到 CSS 中错误的地方，这也是预处理框架的好处，它会报告错误，你可以从这篇[文章](http://tjholowaychuk.com/post/5002088731/stylus-vs-sass-vs-less-error-reporting )中学习如何让 CSS 框架错误报告。

注释

以上三种框架都支持形如 /\* \*/ 的多行注释以及 // 的单行注释。

[英文原文](http://net.tutsplus.com/tutorials/html-css-techniques/sass-vs-less-vs-stylus-a-preprocessor-shootout/ )，OSCHINA 原创翻译
