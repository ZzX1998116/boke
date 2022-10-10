---
title: es6数组方法
date: 2022-10-09 09:34:51
cover: /folder/source/_posts/catk.jpg
tags: [数组,es6]
---
这里的是es6新增的数组扩展,很多不常用的整理一下方便查找
<!-- more -->

## 数组的扩展

### 1.扩展运算符
扩展运算符(spread)是三个点(...)。将一个数组转为用逗号分隔的参数序列
``` bash
function f(v, w, x, y, z) {
  return v+w+x+y+z
 }
const args = [0, 1];
f(-1, ...args, 2, ...[3]); //5
```
扩展运算符后面还能放置表达式 {% mark 如果扩展运算符后面是一个空数组，则不产生任何效果 color:purple %}

```bash
let x=2
const arr = [...(x > 0 ? ['a'] : []),'b'];
alert(arr) //a,b
```
{% mark 注意，只有函数调用时，扩展运算符才可以放在圆括号中，否则会报错 color:orange %}

{% codeblock lang:objc %}
 替代函数的apply()方法 
 由于扩展运算符可以展开数组,所以不需要apply()方法将数组转为函数的参数

// ES5 的写法                            // ES5 的写法
Math.max.apply(null, [14, 3, 77])        var arr1 = [0, 1, 2];
                                         var arr2 = [3, 4, 5];
                                         Array.prototype.push.apply(arr1, arr2);
// ES6 的写法                             // ES6 的写法    
Math.max(...[14, 3, 77])                 let arr1 = [0, 1, 2];
                                         let arr2 = [3, 4, 5];
// 等同于                                 
Math.max(14, 3, 77);                      arr1.push(...arr2);

{% endcodeblock %}

#### 扩展运算符的应用

##### (1)复制数组
数组是复合的数据类型，直接复制的话，只是复制了指向底层数据结构的指针，而不是克隆一个全新的数组。
{% codeblock lang:objc %}
const a1 = [1, 2];
const a2 = a1;

a2[0] = 2;
a1 // [2, 2]
//上面代码中，a2并不是a1的克隆，而是指向同一份数据的另一个指针。修改a2，会直接导致a1的变化(深拷贝)。
{% endcodeblock %}
ES5 只能用变通方法来复制数组。
{% codeblock lang:objc %}
const a1 = [1, 2];
const a2 = a1.concat();

a2[0] = 2;
a1 // [1, 2]
a2 // [2, 2]
a1 === a2 //false
//上面代码中，a1会返回原数组的克隆，再修改a2就不会对a1产生影响(浅拷贝)。
{% endcodeblock %}
扩展运算符提供了复制数组的简便写法。
{% codeblock lang:objc %}
const a1 = [1, 2];
// 写法一
const a2 = [...a1];
// 写法二
const [...a2] = a1;
//都是浅拷贝
{% endcodeblock %}

##### (2)合并数组
扩展运算符提供了数组合并的新写法。
{% codeblock lang:objc %}
const arr1 = ['a', 'b'];
const arr2 = ['c'];
const arr3 = ['d', 'e'];

// ES5 的合并数组
arr1.concat(arr2, arr3);
// [ 'a', 'b', 'c', 'd', 'e' ]

// ES6 的合并数组
[...arr1, ...arr2, ...arr3]  // [ 'a', 'b', 'c', 'd', 'e' ]
//不过，这两种方法都是浅拷贝，使用的时候需要注意。
{% endcodeblock %}
{% codeblock lang:objc %}
const a1 = [{ foo: 1 }];
const a2 = [{ bar: 2 }];

const a3 = a1.concat(a2);
const a4 = [...a1, ...a2];

a3[0] === a1[0] // true
a4[0] === a1[0] // true
//a3和a4是用两种不同方法合并而成的新数组，但是它们的成员都是对原数组成员的引用，这就是浅拷贝。如果修改了引用指向的值，会同步反映到新数组。
{% endcodeblock %}
##### (3)与解构赋值结合
扩展运算符可以与解构赋值结合起来，用于生成数组。
{% codeblock lang:objc %}
const [first, ...rest] = [1, 2, 3, 4, 5];
first // 1
rest  // [2, 3, 4, 5]

const [first, ...rest] = [];
first // undefined
rest  // []

const [first, ...rest] = ["foo"];
first  // "foo"
rest   // []
const [...butLast, last] = [1, 2, 3, 4, 5];
// 报错
//如果将扩展运算符用于数组赋值，只能放在参数的最后一位，否则会报错。
{% endcodeblock %}
##### (4)字符串
扩展运算符还可以将字符串转为真正的数组。
{% codeblock lang:objc %}
[...'hello']
// [ "h", "e", "l", "l", "o" ]
//凡是涉及到操作四个字节的 Unicode 字符的函数，都有这个问题。因此，最好都用扩展运算符改写

let str = 'x\uD83D\uDE80y';
str.split('').reverse().join('')
// 'y\uDE80\uD83Dx'
[...str].reverse().join('')
// 'y\uD83D\uDE80x'
//上面代码中，如果不用扩展运算符，字符串的reverse()操作就不正确。
{% endcodeblock %}
##### (5)实现Iterator 接口的对象
