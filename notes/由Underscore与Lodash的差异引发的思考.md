---
title: 由Underscore与Lodash的差异引发的思考
created: '2019-09-04T09:36:23.278Z'
modified: '2019-09-04T09:36:24.808Z'
---

# 由Underscore与Lodash的差异引发的思考

2014年06月18日 | [Comments](#disqus_thread)

自打接触[Underscore](http://underscorejs.org/)以来就对其爱不释手，尔后又了解到[Lodash](http://lodash.com/)。这两个类库为我们提供了一系列相当不错的跟函数式编程相关的方法。Underscore以API实现简洁著称。Lodash作为Underscore的后继者，除了对Underscore现有API功能使用上进行扩充外，更是添加了不少令人难忘的API，在性能上也更为出彩，而且还能根据需要构建自己的子集方法。相较而言，我更亲睐于Lodash，而且成了日常项目开发的标配。

目前Lodash的最新版本是v2.4.1，功能上可以说完全是Underscore的超集。只要Underscore添加了啥新功能时，Lodash都会及时覆盖更新，以维护它一如既往超集的地位。然而，当Underscore更新到v1.6.0时，这个版本添加了一个很棒的功能，其为方法`_.partial`添加了占位符参数的支持（如果没猜错的话，这个特性应该是从[functional.js](http://osteele.com/sources/javascript/functional/)中移植过来了）。自然地，希望Lodash也能很快地对`_.partial`做增强处理。可是，四个月过去了，Lodash似乎处于冬眠状态，一点都不见有动静。于是乎，到目前为止，Lodash的功能不足以完全覆盖Underscore了，于是也就有了这篇文章。

## 循序渐进

还是以例子引入话题，假如我要将数组`['4', '8', '15', '16', '23', '42']`([神奇数字](http://www.douban.com/group/topic/6251101/))中所有字符串元素变换为数字型。不假思索后，我们会这样写：

|

1
2
3
4

 |

```
//=> [4, 8, 15, 16, 23, 42]
_.map(['4', '8', '15', '16', '23', '42'], function (val) {
  return Number(val);
});

```

 |

没错，这没什么问题。稍微观察一下，我们会发现，传递给`_.map`方法的匿名回调方法仅仅做了件简单的事，将其第一个参数传进`Number`函数中调用后直接返回。这种做法其实就相当于：你实际要执行的是`f`函数，而你却通过调用`g`函数间接执行`f`函数，而事实上你完全可以直接执行`f`函数的，如下所示：

|

1
2
3
4
5

 |

```
var f = function (val) { return val; };
var g = function (val) { return f(val); };

//=> true
f('stupid') === g('stupid');

```

 |

因此，上述数组变换实际可以简化为：

|

1
2

 |

```
//=> [4, 8, 15, 16, 23, 42]
_.map(['4', '8', '15', '16', '23', '42'], Number);

```

 |

嗯，确实是这么回事^\-^。

再比如，我想将数组`['1NO', '2FOOL', '3ME']`中所有字符串元素解析为数字型。我们知道将字符串解析为数字型可以通过`parseInt`直接得到，像这样：

|

1
2
3

 |

```
var first = parseInt('1NO');     //=> 1
var middle = parseInt('2FOOL');  //=> 2
var last = parseInt('3ME');      //=> 3

```

 |

It works! 于是以迅雷不及掩耳的速度得出结果：

|

1
2
3
4

 |

```
//=> [1, 2, 3]
_.map(['1NO', '2FOOL', '3ME'], function (val) {
  return parseInt(val);
});

```

 |

Perfect！细看一下，咿，这不是和之前那个例子一样的嘛，这次学聪明了，窃喜之下后马上将结果改为如下，也没忘夸奖下自己随机应变的能力：

|

1
2

 |

```
//=> [1, NaN, NaN]
_.map(['1NO', '2FOOL', '3ME'], parseInt);

```

 |

等等，我勒个去，结果不符合预期了，这是怎么回事？！

脑袋稍微转下，原来是这么回事，当你把`parseInt`直接作为`_.map`方法的回调时，`parseInt`执行的时候实际是传入了三个参数(元素值，元素索引，数组本身)。 所以上述代码实际等价于：

|

1
2
3
4
5
6
7
8
9
10

 |

```
var ary = ['1NO', '2FOOL', '3ME'];

//=> [1, NaN, NaN]
_.map(ary, function (val, idx, ary) {
  return parseInt(val, idx, ary);
});

var first = parseInt('1NO', 0, ary);     //=> 1
var middle = parseInt('2FOOL', 1, ary);  //=> NaN
var last = parseInt('3ME', 2, ary);      //=> NaN

```

 |

`parseInt`调用时可接收可选的第二个参数，元素索引值作为第二个参数无形中传入到`parseInt`，呜呼哀哉！第一个例子为什么没问题？因为Number只接收一个参数，而把其后的所有参数都忽略，所以安然无恙。这么看来`Explicit is better than implicit(显优于隐)`的确是真理啊！

在此，我不想弹劾`parseInt`直接作为`_.map`回调使用的情况，我只想吐嘈下`parseInt`不显式指定第二个参数调用的隐患。MDN上关于[parseInt API](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/parseInt)的定义是强调指明要传入`radix(基数)`值作为解析数字的依据的。

除此以外，我想强调的是`Number`和`parseInt`是否有`共同特征(pattern recognition)`值得我们去挖掘的呢？答案是有的。`_.map`遍历数组元素时，其都是将数组元素传入到`Number`或`parseInt`作为其第一个参数进行执行的。而这是函数式编程很重要的一个特点，对函数进行`柯里化(curry)`或`偏应用(partial)`处理时，传给函数的第一个参数往往是数据流，`数据流(data flow)`也是函数式编程不同于`控制流(control flow)`的非函数式编程的一个显著区别。

回到之前的问题，既然`parseInt`不能直接作为`_.map`回调处理，而我又不想使用匿名函数间接调用`parseInt`的刻板方法，那么还有其他办法吗？答案是有的。

试想，我们遇到的问题是`parseInt`作为`_.map`回调执行时，无形中其第二个参数被污染了。反过来想，我们要找到一个办法使得其第二个参数免受污染。所幸的是，Underscore v1.6.0版本中提供的`_.partial`正好能满足这一需求。

我的想法是，利用`_.partial`对`parseInt`进行偏应用处理，返回得到的新函数再作为回调传入`_.map`中，如下所示：

|

1
2

 |

```
//=> [1, 2, 3]
_.map(['1NO', '2FOOL', '3ME'], _.partial(parseInt, _, 10));

```

 |

在这个例子中，我们为`parseInt`预填充了两个参数：第一个参数传入`_`代表参数占位符，它是动态值；第二个传入基数值10，它是不变的，这样我们就将其第第二个参数“锁定”了。当`_.map`回调函数被执行时，它还是依次接收三个参数回来，只不过这次接收的第一个参数（数组元素作为数据填补）代替了参数占位符的位置，第二、三个参数被依次追加到`parseInt`末尾而被忽略，于是我们的代码正常工作了。

然而，这行代码Underscore v1.6.0+版本中才有效，对于Lodash或更低版本的Underscore，我们该怎么办呢？这时函数柯里化的威力就体现出来了。因为`parseInt`接收两个参数，于是我构建如下的二级柯里化函数：

|

1
2
3
4
5
6
7

 |

```
function curry2(fun) {
  return function (second) {
    return function (first) {
      return fun(first, second);
    };
  };
}

```

 |

函数`curry2`调用时接收一个希望被柯里化的函数作为参数传入，方法体中返回一个匿名函数，接收单一参数，而在匿名函数中又再次返回一个匿名函数，同样接收单一参数，直到柯里化函数执行两次时，被柯里化的函数得以执行，两级柯里化参数逆序传入而返回。

应用到这个例子，即：我们对`parseInt`进行柯里化处理，执行一次传入参数10，也是预填充第二个参数，结果返回新的函数等待`_.map`被执行时将第一个参数传递进行而返回结果值，代码如下：

|

1
2

 |

```
//=> [1, 2, 3]
_.map(['1NO', '2FOOL', '3ME'], curry2(parseInt)(10));

```

 |

在这个妥协的解决方法里，我们借助了自己构建的柯里化函数作为辅助。这不禁让我这么想：难道在这泱泱几十号工具方法中，在这个问题上一无是处，无一能为我所用，反倒要依赖外援？思考片刻后，依照`curry2`的思路，想到Lodash不是提供有`_.partialRight`方法嘛！从左往右填充参数不行，那就从右往左介入。就像`curry2`那样给`parseInt`预填充第二个参数，然后等待`_.map`填充第一个参数后执行，于是写了如下代码：

|

1
2

 |

```
//=> [1, NaN, NaN]
_.map(['1NO', '2FOOL', '3ME'], _.partialRight(parseInt, 10));

```

 |

结果不尽如人意，和`parseInt`直接作为`_.map`回调函数传入时结果没什么两样，执行时`parseInt`仍是被`_.map`传回来的多余参数污染了，基数10成为了`parseInt`的末尾参数而直接忽略了。多余？这给我提了个醒，如果我能将`_.map`传回来的多余的后两参数过滤掉，那么`parseInt`被执行时作为基数10的末尾参数是不是就是作为其第二个参数传入而正常了。Great！现在的问题变成了如何将`_.map`传回来的多余的后两参数过滤掉？思考良久后我辗转想到了`_.identity`。`_.identity`应该算Lodash中倒数第二简单的方法了吧，最简单的是`_.noop`，它们各自定义如下所示：

|

1
2

 |

```
_.noop = function () {};
_.identity = function (val) { return val; };

```

 |

简单的东西并非一无是处。麻雀虽小，五脏俱全。有时候往往是简单的东西才是我们需要的东西。咋一看，`_.identity`不就是你给它什么输入，它就给你返回什么嘛。可是当你换个角度想，你却发现了别样的东西，直接上代码：

|

1
2
3

 |

```
_.identity('I');                  //=> 'I'
_.identity('I', 'FOOL');          //=> 'I'
_.identity('I', 'FOOL', 'YOU');   //=> 'I'

```

 |

发现了什么？所以，更准确地说，无论你给`_.identity`传入多少个参数，结果都是返回给你传入的第一个参数。这样一想，我传三个参数给它，结果它只把第一个参数返回了，剩余那两个参数呢？消失在茫茫人海之中，不见了。Good Job！

好了，现在解决了参数过滤的问题，我该怎么把过滤后存活下来的第一个参数传给`partial right`后的`parseInt`函数使用呢。我们需要管道进行衔接，一个函数的输出作为接下来另一个函数的输入，`_.compose`不正是专门做这事的嘛，Yeah！

假设`f`和`g`是两函数，`f`函数调用后的输出作为`g`函数的输入，其中`x`是`f`的输入值，则以下等式是成立的：

|

1
2

 |

```
//=> true
_.isEqual( g(f(x)), _.compose(g, f)(x) );

```

 |

有了以上的分析后，我们通过`_.compose`作为纽带将`partial right`后的`parseInt`和`_.identity`衔接起来，最终得到答案：

|

1
2

 |

```
//=> [1, 2, 3]
_.map( ['1NO', '2FOOL', '3ME'], _.compose( _.partialRight(parseInt, 10), _.identity ) );

```

 |

看到以上这行代码，你在脑海中涌动的应该就是数据在各个函数间依次流动着而最终得到结果，即函数式编程的`数据流(data flow)`思想。我并不是倡导在实际工作中要写这种代码，更多的是体会函数式编程的思维，那种自成一体的曼妙。函数式编程的思想表现的不是`MARVEL`旗下的那些个个人英雄主义气概，而是各个功能单一的函数组合在一起才能体现的威力。

## 思维发散

另一个例子：将数组`['left ', ' center ', ' right']`中各个字符串元素两边的空白符去除，长驱直入：

|

1
2
3
4

 |

```
//=> ['left', 'center', 'right']
_.map(['left  ', ' center ', '  right'], function (s) {
  return s.trim();
});

```

 |

仔细看这段代码，你会看到另一种模式，匿名函数里返回的是第一个参数调用某个方法的结果。函数式编程的核心是函数，而不是方法。我们要将方法调用转化为函数调用，才更能体现函数式风格。Underscore和Lodash都提供了`_.result`方法。通过这个方法我们可以将方法调用转化为函数调用，也即：

|

1
2
3
4

 |

```
var str = ' center ';

//=> true
str.trim() === _.result(str, 'trim');

```

 |

`_.result`调用像不像此前的`parseInt`？这样，我们之前的方法再次奏效：

|

1
2
3
4
5
6
7
8
9
10

 |

```
var ary = ['left  ', ' center ', '  right'];

// Underscore v1.6.0
_.map(ary, _.partial( _.result, _, 'trim' ));

// Lodash v2.4.1
_.map(ary, _.compose( _.partialRight( _.result, 'trim' ), _.identity ));

// Underscore & Lodash
_.map(ary, curry2( _.result )( 'trim' ));

```

 |

## 延伸拓展

前面提到了`_.identity`方法，如果换种角度来看，可以把它当作参数过滤器使用。可是它是有局限性的，它只能过滤第一个参数。假如某种情况下我要过滤出输入参数的前两个参数，这回该怎么办？我们知道函数只能返回一个值，要返回多个值的话，则可以将多个值以数组形式返回。以下是过滤前两个参数的代码：

|

1
2
3
4
5
6
7
8
9

 |

```
function take2(first, second) {
  return [first, second];
}

//=> ['first', 'second']
take2('first', 'second', 'third');

//=> ['first', 'second']
take2('first', 'second', 'third', 'forth');

```

 |

解决了过滤两个参数的问题，可是它有个瑕疵，它的输出是以数组形式返回的。如果这种输出直接作为另一个函数的输入，这将无法衔接起来，因为我们的接收方函数要求参数是单个单个传入，而不是给它灌入单个数组。于是，我们接下来需要某种能够将数组变换为单个单个参数传入接收方函数的方法。

先看以下举例：

|

1
2
3
4
5
6

 |

```
function max(/* args */) {
  return Math.max.apply(Math, arguments);
}

//=> 42
max(8, 4, 15, 42, 23, 16);

```

 |

`max`方法很简单，它返回任意输入Number型参数的最大值。给它输入的单个单个参数依次是`8, 4, 15, 42, 23, 16`。现在假设我只想知道输入参数的前两个哪个最大。我们脑海里首先闪过的可能是如下答案：

|

1
2
3
4
5
6

 |

```
function max_initial(first, second) {
  return max(first, second);
}

//=> 8
max_initial(8, 4, 15, 42, 23, 16);

```

 |

看起来还不错！但我们前面定义了专门充当“参数过滤器”的函数`take2`，让我们试图将它介入其中，像如下那样：

|

1
2
3
4
5
6

 |

```
function max_initial2(/* args */) {
  return max.apply(null, take2.apply(null, arguments));
}

//=> 8
max_initial2(8, 4, 15, 42, 23, 16);

```

 |

感谢`Function.prototype.apply`方法，我们成功地将参数过滤了，并传给了`max`，It Works！可是盯着它看下，怎么看怎么不舒服。还是再改进下吧～前面我们提到`_.compose`方法可以将参数输入输出串联起来，那再试图让`_.compose`介入进来吧！

|

1
2
3
4

 |

```
var max_initial3 = _.compose(max, take2);

//=> NaN
max_initial3(8, 4, 15, 42, 23, 16);

```

 |

Does it work？Nope！Why？`max_initial3`函数执行实际等价于如下代码：

|

1
2
3
4
5

 |

```
//=> [8, 4]
var result = take2(8, 4, 15, 42, 23, 16);

//=> NaN
max(result);

```

 |

我们将`take2`执行后返回的数组值直接传递给了`max`，而`max`需接收的是单个单个的参数。由于接口的不一致，我们的尝试失败了。既然接口不一致，我们何不制造个`适配器(Adapter)`以适配接口呢！我们需要什么样的适配器？我们需要将数组参数转化为单个单个参数的适配器——我们需要`splat`适配器：

|

1
2
3
4
5
6
7
8
9
10

 |

```
function splat(fun) {
  return function (array) {
    return fun.apply(null, array);
  };
}

var maxAdapted = splat(max);

//=> 8
maxAdapted([8, 4]);

```

 |

Looks pretty neat, doesn’t it?有了适配器后，我们再用`_.compose`将它们衔接起来：

|

1
2
3
4

 |

```
var max_initial4 = _.compose(splat(max), take2);

//=> 8
max_initial4(8, 4, 15, 42, 23, 16);

```

 |

瞧，它又正常工作了，多亏了`splat`适配器。适配器用于适配不同的接口，以此将不同的接口衔接起来，它的职责是单一的，因此也可以复用。

现在我们有了将数组参数转化为单个单个参数的`splat`适配器。反过来，我们何不再制造个将单个单个参数转化为数组参数的适配器呢？我们估且叫它`unsplat`吧！

|

1
2
3
4
5
6
7
8
9
10
11
12

 |

```
function unsplat(fun) {
  return function () {
    return fun.call(null, [].slice.call(arguments));
  };
}

// 既然我们使用Underscore或Lodash，我们何不写成这样呢！
function unsplat(fun) {
  return function () {
    return fun.call(null, _.toArray(arguments));
  };
}

```

 |

`unsplat`能用在什么地方呢？Let’s have a try!

前面我们定义有`take2`，我们何不定义个更一般的`take`函数呢？

|

1
2
3
4
5
6

 |

```
function take(n, array) {
  return array.slice(0, n);
}

//=> [8, 4]
take(2, [8, 4, 15, 42, 23, 16]);

```

 |

有了更一般的`take`函数，我们就可以基于它生成过滤任意多个前置参数的“参数过滤器”函数了。

|

1
2
3
4
5
6
7
8

 |

```
var take2 = _.partial(take, 2);
var take3 = _.partial(take, 3);

//=> [8, 4]
take2([8, 4, 15, 42, 23, 16]);

//=> [8, 4, 15]
take3([8, 4, 15, 42, 23, 16]);

```

 |

这不对！之前我们调用`take2`的时候传递给它的是单个单个的参数，可是这回调用时传递的却是数组参数。不行，我们需要适配器，我们需要将单个单个参数转化为数组参数的适配器——我们需要`unsplat`适配器！

|

1
2
3
4
5
6
7
8

 |

```
var take2 = unsplat(_.partial(take, 2));
var take3 = unsplat(_.partial(take, 3));

//=> [8, 4]
take2(8, 4, 15, 42, 23, 16);

//=> [8, 4, 15]
take3(8, 4, 15, 42, 23, 16);

```

 |

这回正常了！回到最初的问题，针对`max`函数，给它输入的单个单个参数依次是`8, 4, 15, 42, 23, 16`。现在假设我只想知道输入参数的前三个哪个最大，我们有了一般性的答案：

|

1
2
3
4

 |

```
var max_revamped = _.compose( splat(max), unsplat( _.partial(take, 3) ) );

//=> 15
max_revamped(8, 4, 15, 42, 23, 16);

```

 |

Now everything is under control! 如果我们了解`_.wrap`方法，我们可以做的更疯狂：

|

1
2
3
4
5
6
7

 |

```
var max_fancy = _.compose(
  _.wrap( max, splat )(),
  _.wrap( _.partial(take, 3), unsplat )()
);

//=> 15
max_fancy(8, 4, 15, 42, 23, 16);

```

 |

太过天马行空了，似乎偏离主旨了。。。还是就此收笔吧！

## 总结

函数式编程就像搭积木一样，像`_.identity`、`_.partial`、`_.compose`、`splat`、`unsplat`等都是职责单一的函数。别看它们简单，把它们当作积木看待，它们释放的是无尽的活力。 越是简单的东西，蕴藏的越是更为无限的可能性。

最后，我的感悟是：函数式编程以数据流动为导向，是函数装配的艺术。

Posted by 俞乐 2014年06月18日 [javascript](http://myunlessor.github.io/blog/categories/javascript/)
