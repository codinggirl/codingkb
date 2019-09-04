---
title: avaScript 数组方法对比
created: '2019-09-04T10:05:38.939Z'
modified: '2019-09-04T10:05:40.008Z'
---

# avaScript 数组方法对比

发表于 2017\-02\-04 |   分类于 [JavaScript](https://jinlong.github.io/categories/JavaScript/)   |  [](https://jinlong.github.io/2017/02/04/javascript-array-methods-mutating-vs-non-mutating/#comments)

> 原文：“[JavaScript Array Methods: Mutating vs. Non\-Mutating](http://lorenstewart.me/2017/01/22/javascript-array-methods-mutating-vs-non-mutating/)”
> 笔记：[涂鸦码龙](http://weibo.com/newwave)

JavaScript 提供了多种新增，移除，替换数组元素的方法，但是有些会影响原来的数组；有些则不会，它是新建了一个数组。

**注意**：区分以下两个方法的不同点：

1.  `array.splice()` 影响原来的数组
2.  `array.slice()` 不影响原来的数组

# I. 新增：影响原数组

使用 `array.push()` 和 `array.ushift()` 新增元素会影响原来的数组。

|

1

2

3

 |

let mutatingAdd = \[
    'a', 'b', 'c', 'd', 'e'\];

mutatingAdd.push('f'); // \['a', 'b', 'c', 'd', 'e', 'f'\]

mutatingAdd.unshift('z'); // \['z', 'b', 'c', 'd', 'e' 'f'\]

 |

# II. 新增：不影响原数组

两种方式新增元素不会影响原数组，第一种是 `array.concat()` 。

|

1

2

3

4

 |

const arr1 = \['a', 'b', 'c', 'd', 'e'\];

const arr2 = arr1.concat('f'); // \['a', 'b', 'c', 'd', 'e', 'f'\]  （注：原文有误，我做了修改 “.” \-\-\-> “,”）

console.log(arr1); // \['a', 'b', 'c', 'd', 'e'\]

 |

第二种方法是使用 JavaScript 的展开（spread）操作符，展开操作符是三个点（…）

|

1

2

3

4

 |

const arr1 = \['a', 'b', 'c', 'd', 'e'\];

const arr2 = \[...arr1, 'f'\]; // \['a', 'b', 'c', 'd', 'e', 'f'\]

const arr3 = \['z', ...arr1\]; // \['z', 'a', 'b', 'c', 'd', 'e'\]

 |

展开操作符会复制原来的数组，从原数组取出所有元素，然后存入新的环境。

# III. 移除：影响原数组

使用 `array.pop()` 和 `array.shift()` 移除数组元素时，会影响原来的数组。

|

1

2

3

 |

let mutatingRemove = \['a', 'b', 'c', 'd', 'e'\];

mutatingRemove.pop(); // \['a', 'b', 'c', 'd'\]

mutatingRemove.shift(); // \['b', 'c', 'd'\]

 |

`array.pop()` 和 `array.shift()` 返回被移除的元素，你可以通过一个变量获取被移除的元素。

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

let mutatingRemove = \['a', 'b', 'c', 'd', 'e'\];

const returnedValue1 = mutatingRemove.pop();

console.log(mutatingRemove); // \['a', 'b', 'c', 'd'\]

console.log(returnedValue1); // 'e'

const returnedValue2 = mutatingRemove.shift();

console.log(mutatingRemove); // \['b', 'c', 'd'\]

console.log(returnedValue2); // 'a'

 |

`array.splice()` 也可以删除数组的元素。

|

1

2

 |

let mutatingRemove = \['a', 'b', 'c', 'd', 'e'\];

mutatingRemove.splice(0, 2); // \['c', 'd', 'e'\]

 |

像 `array.pop()` 和 `array.shift()` 一样，`array.splice()` 同样返回移除的元素。

|

1

2

3

4

 |

let mutatingRemove = \['a', 'b', 'c', 'd', 'e'\];

let returnedItems = mutatingRemove.splice(0, 2);

console.log(mutatingRemove); // \['c', 'd', 'e'\]

console.log(returnedItems) // \['a', 'b'\]

 |

# IV. 移除：不影响原数组

JavaScript 的 `array.filter()` 方法基于原数组创建一个新数组，新数组仅包含匹配特定条件的元素。

|

1

2

3

4

5

6

7

 |

const arr1 = \['a', 'b', 'c', 'd', 'e'\];

const arr2 = arr1.filter(a => a !== 'e'); // \['a', 'b', 'c', 'd'\]（注：原文有误，我做了修改）

// 或者

const arr2 = arr1.filter(a => {

  return a !== 'e';

}); // \['a', 'b', 'c', 'd'\]（注：原文有误，我做了修改）

 |

以上代码的条件是“不等于 ‘e’ ”，因此新数组（`arr2`）里面没有包含 ‘e’。

---

**箭头函数的独特性**：

**单行箭头函数**，’return’ 关键字是默认自带的，不需要手动书写。

可是，**多行箭头函数**就需要明确地返回一个值。

---

另一种不影响原数组的方式是 `array.slice()`（不要与 `array.splice()` 混淆）。

|

1

2

3

 |

const arr1 = \['a', 'b', 'c', 'd', 'e'\];

const arr2 = arr1.slice(1, 5) // \['b', 'c', 'd', 'e'\]

const arr3 = arr1.slice(2) // \['c', 'd', 'e'\]

 |

# V. 替换：影响原数组

如果知道替换哪一个元素，可以使用 `array.splice()` 。

|

1

2

3

4

 |

let mutatingReplace = \['a', 'b', 'c', 'd', 'e'\];

mutatingReplace.splice(2, 1, 30); // \['a', 'b', 30, 'd', 'e'\]

// 或者

mutatingReplace.splice(2, 1, 30, 31); // \['a', 'b', 30, 31, 'd', 'e'\]

 |

# VI. 替换：不影响原数组

可以使用 `array.map()` 创建一个新数组，并且可以检查每一个元素，根据特定的条件替换它们。

|

1

2

3

4

5

6

7

 |

const arr1 = \['a', 'b', 'c', 'd', 'e'\]

const arr2 = arr1.map(item => {

  if(item === 'c') {

    item = 'CAT';

  }

  return item;

}); // \['a', 'b', 'CAT', 'd', 'e'\]

 |

**使用 `array.map()` 转换数据**

`array.map()` 是个强力方法，可以用于转换数据，而不污染原先的数据源。

|

1

2

3

 |

const origArr = \['a', 'b', 'c', 'd', 'e'\];

const transformedArr = origArr.map(n => n + 'Hi!'); // \['aHi!', 'bHi!', 'cHi!', 'dHi!', 'eHi!'\]

console.log(origArr); // \['a', 'b', 'c', 'd', 'e'\]; // 原数组毫发无损

 |
