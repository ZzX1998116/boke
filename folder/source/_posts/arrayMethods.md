---
title: ES6数组方法
date: 2022-10-13 15:40:00
cover: ./uncategorized/arrayMethods/4.jpg
cover_info:
  meta:  ES6数组方法
  title: 这里的是es6新增的数组扩展,很多不常用的整理一下方便查找
tags: [数组,ES6]
---
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
任何定义了遍历器（Iterator）接口的对象（参阅 Iterator 一章），都可以用扩展运算符转为真正的数组.
{% codeblock lang:objc %}
let nodeList = document.querySelectorAll('div');
let array = [...nodeList];
//上面代码中，querySelectorAll()方法返回的是一个NodeList对象。它不是数组，而是一个类似数组的对象。这时，扩展运算符可以将其转为真正的数组，原因就在于NodeList对象实现了 Iterator
{% endcodeblock %}
{% codeblock lang:objc %}
Number.prototype[Symbol.iterator] = function*() {
  let i = 0;
  let num = this.valueOf();
  while (i < num) {
    yield i++;
  }
}
console.log([...5]) // [0, 1, 2, 3, 4]
//上面代码中，先定义了Number对象的遍历器接口，扩展运算符将5自动转成Number实例以后，就会调用这个接口，就会返回自定义的结果。
{% endcodeblock %}
##### (6)Map 和 Set 结构，Generator 函数
扩展运算符内部调用的是数据结构的 Iterator 接口，因此只要具有 Iterator 接口的对象，都可以使用扩展运算符，比如 Map 结构。
{% codeblock lang:objc %}
let map = new Map([
  [1, 'one'],
  [2, 'two'],
  [3, 'three'],
]);

let arr = [...map.keys()]; // [1, 2, 3]
{% endcodeblock %}
Generator 函数运行后，返回一个遍历器对象，因此也可以使用扩展运算符。
{% codeblock lang:objc %}
const go = function*(){
  yield 1;
  yield 2;
  yield 3;
};

[...go()] // [1, 2, 3]
//上面代码中，变量go是一个 Generator 函数，执行后返回的是一个遍历器对象，对这个遍历器对象执行扩展运算符，就会将内部遍历得到的值，转为一个数组。
{% endcodeblock %}
### 2.Array.from
Array.from()方法用于将两类对象转为真正的数组：类似数组的对象（array-like object）和可遍历（iterable）的对象（包括 ES6 新增的数据结构 Set 和 Map）。
下面是一个类似数组的对象，Array.from()将它转为真正的数组。
{% codeblock lang:objc %}
let arrayLike = {
    '0': 'a',
    '1': 'b',
    '2': 'c',
    length: 3
};

// ES5 的写法
var arr1 = [].slice.call(arrayLike); // ['a', 'b', 'c']

// ES6 的写法
let arr2 = Array.from(arrayLike); // ['a', 'b', 'c']
{% endcodeblock %}
实际应用中，常见的类似数组的对象是 DOM 操作返回的 NodeList 集合，以及函数内部的arguments对象。Array.from()都可以将它们转为真正的数组。
{% codeblock lang:objc %}
// NodeList 对象
let ps = document.querySelectorAll('p');
Array.from(ps).filter(p => {
  return p.textContent.length > 100;
});

// arguments 对象
function foo() {
  var args = Array.from(arguments);
  // ...
}
//上面代码中，querySelectorAll()方法返回的是一个类似数组的对象，可以将这个对象转为真正的数组，再使用filter()方法。
{% endcodeblock %}
只要是部署了 Iterator 接口的数据结构，Array.from()都能将其转为数组。
{% codeblock lang:objc %}
Array.from('hello')
// ['h', 'e', 'l', 'l', 'o']

let namesSet = new Set(['a', 'b'])
Array.from(namesSet) // ['a', 'b']
//上面代码中，字符串和 Set 结构都具有 Iterator 接口，因此可以被Array.from()转为真正的数组。
//如果参数是一个真正的数组，Array.from()会返回一个一模一样的新数组。
//值得提醒的是，扩展运算符（...）也可以将某些数据结构转为数组。
{% endcodeblock %}
Array.from()还可以接受一个函数作为第二个参数，作用类似于数组的map()方法，用来对每个元素进行处理，将处理后的值放入返回的数组。
{% codeblock lang:objc %}
Array.from(arrayLike, x => x * x);
// 等同于
Array.from(arrayLike).map(x => x * x);

Array.from([1, 2, 3], (x) => x * x)
// [1, 4, 9]
{% endcodeblock %}
如果map()函数里面用到了this关键字，还可以传入Array.from()的第三个参数，用来绑定this。
{% codeblock lang:objc %}
Array.from({ length: 2 }, () => 'jack')
// ['jack', 'jack']
//上面代码中，Array.from()的第一个参数指定了第二个参数运行的次数。这种特性可以让该方法的用法变得非常灵活。
{% endcodeblock %}
Array.from()的另一个应用是，将字符串转为数组，然后返回字符串的长度。因为它能正确处理各种 Unicode 字符，可以避免 JavaScript 将大于\uFFFF的 Unicode 字符，算作两个字符的 bug。
{% codeblock lang:objc %}
function countSymbols(string) {
  return Array.from(string).length;
}
{% endcodeblock %}
### 3.Array.of()
Array.of()方法用于将一组值,转换为数组。
{% codeblock lang:objc %}
Array.of(3, 11, 8) // [3,11,8]
Array.of(3) // [3]
Array.of(3).length // 1
{% endcodeblock %}
这个方法的主要目的，是弥补数组构造函数Array()的不足。因为参数个数的不同，会导致Array()的行为有差异。
{% codeblock lang:objc %}
Array() // []
Array(3) // [, , ,]
Array(3, 11, 8) // [3, 11, 8]
//上面代码中，Array()方法没有参数、一个参数、三个参数时，返回的结果都不一样。只有当参数个数不少于 2 个时，Array()才会返回由参数组成的新数组。参数只有一个正整数时，实际上是指定数组的长度。
{% endcodeblock %}
Array.of()基本上可以用来替代Array()或new Array()，并且不存在由于参数不同而导致的重载。它的行为非常统一
{% codeblock lang:objc %}
Array.of() // []
Array.of(undefined) // [undefined]
Array.of(1) // [1]
Array.of(1, 2) // [1, 2]
{% endcodeblock %}
{% mark Array.of()总是返回参数值组成的数组。如果没有参数，就返回一个空数组。 color:orange %}
### 4.实例方法：copyWithin()
数组实例的copyWithin()方法，在当前数组内部，将指定位置的成员复制到其他位置（会覆盖原有成员），然后返回当前数组。也就是说，使用这个方法，会{% emp 修改当前数组 %}。
{% codeblock lang:objc %}
Array.prototype.copyWithin(target, start = 0, end = this.length)
//target（必需）：从该位置开始替换数据。如果为负值，表示倒数。
//start（可选）：从该位置开始读取数据，默认为 0。如果为负值，表示从末尾开始计算。
//end（可选）：到该位置前停止读取数据，默认等于数组长度。如果为负值，表示从末尾开始计算。 
{% endcodeblock %}
{% mark 这三个参数都应该是数值，如果不是，会自动转为数值。 color:orange %}
{% codeblock lang:objc %}
[1, 2, 3, 4, 5].copyWithin(0, 3)
// [4, 5, 3, 4, 5]
//上面代码表示将从 3 号位直到数组结束的成员（4 和 5），复制到从 0 号位开始的位置，结果覆盖了原来的 1 和 2。
{% endcodeblock %}
还有其他
{% codeblock lang:objc %}
// 将3号位复制到0号位
[1, 2, 3, 4, 5].copyWithin(0, 3, 4)
// [4, 2, 3, 4, 5]

// -2相当于3号位，-1相当于4号位
[1, 2, 3, 4, 5].copyWithin(0, -2, -1)
// [4, 2, 3, 4, 5]

// 将3号位复制到0号位
[].copyWithin.call({length: 5, 3: 1}, 0, 3)
// {0: 1, 3: 1, length: 5}

// 将2号位到数组结束，复制到0号位
let i32a = new Int32Array([1, 2, 3, 4, 5]);
i32a.copyWithin(0, 2);
// Int32Array [3, 4, 5, 4, 5]

// 对于没有部署 TypedArray 的 copyWithin 方法的平台
// 需要采用下面的写法
[].copyWithin.call(new Int32Array([1, 2, 3, 4, 5]), 0, 3, 4);
// Int32Array [4, 2, 3, 4, 5]
{% endcodeblock %}
### 5.实例方法：find(),findIndex(),findLast(),findLastIndex()
数组实例的find()方法，用于找出第一个符合条件的数组成员。它的参数是一个{% emp 回调函数 %}，所有数组成员依次执行该回调函数，直到找出第一个返回值为{% u true %}的成员，然后返回该成员。如果没有符合条件的成员，则返回{% u undefined %}。
{% codeblock lang:objc %}
[1, 4, -5, 10].find((value, index, arr) => vlaue < 0)
// -5
//上面代码中，find()方法的回调函数可以接受三个参数，依次为当前的值、当前的位置和原数组。
{% endcodeblock %}
数组实例的{% sup findIndex() color:red %}方法的用法与find()方法非常类似，返回第一个符合条件的数组成员的{% u 位置 %}，如果所有成员都不符合条件，则返回{% u -1 %}。
{% codeblock lang:objc %}
[1, 5, 10, 15].findIndex(function(value, index, arr) {
  return value > 9;
}) // 2
{% endcodeblock %}
这两个方法都可以接受第二个参数，用来绑定回调函数的this对象。
{% codeblock lang:objc %}
function f(v){
  return v > this.age;
}
let person = {name: 'John', age: 20};
[10, 12, 26, 15].find(f, person);    // 26
//上面的代码中，find()函数接收了第二个参数person对象，回调函数中的this对象指向person对象。
//另外，这两个方法都可以发现NaN，弥补了数组的indexOf()方法的不足
{% endcodeblock %}
find()和findIndex()都是从数组的0号位，依次向后检查。ES2022 新增了两个方法{% sup findLast() %}和{% sup findLastIndex() %}，从数组的最后一个成员开始，依次向前检查，其他都保持不变。
{% codeblock lang:objc %}
const array = [
  { value: 1 },
  { value: 2 },
  { value: 3 },
  { value: 4 }
];

array.findLast(n => n.value % 2 === 1); // { value: 3 }
array.findLastIndex(n => n.value % 2 === 1); // 2
{% endcodeblock %}
### 6.实例方法：fill()
{% sup fill %}方法使用给定值，填充一个数组。
{% codeblock lang:objc %}
['a', 'b', 'c'].fill(7)
// [7, 7, 7]

new Array(3).fill(7)
// [7, 7, 7]
//上面代码表明，fill方法用于空数组的初始化非常方便。数组中已有的元素，会被全部抹去
['a', 'b', 'c'].fill(7, 1, 2)
// ['a', 7, 'c']
//fill方法还可以接受第二个和第三个参数，用于指定填充的起始位置和结束位置。
{% endcodeblock %}
注意，如果填充的类型为对象，那么被赋值的是同一个内存地址的对象，而不是深拷贝对象。
{% codeblock lang:objc %}
let arr = new Array(3).fill({name: "Mike"});
arr[0].name = "Ben";
arr
// [{name: "Ben"}, {name: "Ben"}, {name: "Ben"}]

let arr = new Array(3).fill([]);
arr[0].push(5);
arr
// [[5], [5], [5]]
{% endcodeblock %}
### 7.实例方法：entries(),keys(),values()
ES6 提供三个新的方法——entries()，keys()和values()——用于遍历数组。它们都返回一个遍历器对象（详见《Iterator》一章），可以用for...of循环进行遍历，唯一的区别是keys()是对键名的遍历、values()是对键值的遍历，entries()是对键值对的遍历。
{% codeblock lang:objc %}
for (let index of ['a', 'b'].keys()) {
  console.log(index);
}
// 0
// 1

for (let elem of ['a', 'b'].values()) {
  console.log(elem);
}
// 'a'
// 'b'

for (let [index, elem] of ['a', 'b'].entries()) {
  console.log(index, elem);
}
// 0 "a"
// 1 "b"
{% endcodeblock %}
如果不使用for...of循环，可以手动调用遍历器对象的next方法，进行遍历。
{% codeblock lang:objc %}
let letter = ['a', 'b', 'c'];
let entries = letter.entries();
console.log(entries.next().value); // [0, 'a']
console.log(entries.next().value); // [1, 'b']
console.log(entries.next().value); // [2, 'c']
{% endcodeblock %}
### 8.实例方法:includes()
Array.prototype.includes方法返回一个布尔值，表示某个数组是否包含给定的值，与字符串的includes方法类似。ES2016 引入了该方法。
{% codeblock lang:objc %}
[1, 2, 3].includes(2)     // true
[1, 2, 3].includes(4)     // false
[1, 2, NaN].includes(NaN) // true
{% endcodeblock %}
该方法的第二个参数表示搜索的起始位置 ，默认为0.如果第二个参数为负值,则表示倒数的位置,如果这时它大于数组长度(比如第二个参数为-4,但数组长度为3),则会重置为从0开始。
{% codeblock lang:objc %}
[1, 2, 3].includes(3, 3);  // false
[1, 2, 3].includes(3, -1); // true
{% endcodeblock %}
没有该方法之前,我们通常使用数组的indexOf方法,检查是否包含某个值。
{% codeblock lang:objc %}
if (arr.indexOf(el) !== -1) {
  // ...
}
{% endcodeblock %}
indexOf方法有两个缺点,一是不够语义化，它的含义是找到参数值的第一个出现位置，所以要去比较是否不等于-1，表达起来不够直观。二是，它内部使用严格相等运算符(===)进行判断，这会导致对NaN的误判。
{% codeblock lang:objc %}
[NaN].indexOf(NaN)  // -1
{% endcodeblock %}
includes使用的是不一样的判断算法,就没有这个问题。
{% codeblock lang:objc %}
[NaN].includes(NaN)  //true
{% endcodeblock %}
下面代码用来检查当前环境是否支持该方法，如果不支持，部署一个简易的替代版本。
{% codeblock lang:objc %}
const contains = (() =>
  Array.prototype.includes
    ? (arr, value) => arr.includes(value)
    : (arr, value) => arr.some(el => el === value)
)();
contains(['foo', 'bar'], 'baz'); // => false
{% endcodeblock %}
另外，Map和Set数据结构有一个has方法，需要注意与includes区分。
Map 结构的has方法，是用来查找键名的，比如Map.prototype.has(key)、WeakMap.prototype.has(key)、Reflect.has(target, propertyKey)。
Set 结构的has方法，是用来查找值的，比如Set.prototype.has(value)、WeakSet.prototype.has(value)。
### 9.实例方法:flat(),flatMap()
数组的成员有时还是数组，Array.prototype.flat()用于将嵌套的数组“拉平”，变成一维的数组。该方法返回一个新数组，对原数组没有影响。
{% codeblock lang:objc %}
[1,2,[3,4]].flat()  //[1,2,3,4]
{% endcodeblock %} 
上面代码自己中，原数组的成员里面有一个数组，flat()方法将子组件的成员取出来，添加在原来的位置。
flat()默认只会“拉平”一层，如果想要“拉平”多层的嵌套数组，可以将flat()方法的参数写成一个整数，表示想要拉平的层数，默认为1。
{% codeblock lang:objc %}
[1,2,[3,[4,5]]].flat()    //[1,2,3,[4,5]]
[1,2,[3,[4,5]]].flat(2)   //[1,2,3,4,5]
{% endcodeblock %}
上面代码中，flat()的参数为2，表示要“拉平”两层的嵌套数组。
如果不管有多少层嵌套，都要转成一维数组，可以用Infinity关键字作为参数。
{% codeblock lang:objc %}
[1,[2,[3]]].flat(Infinity)  //[1,2,3]
{% endcodeblock %}
如果原数组有空位，flat()方法会跳过空位。
{% codeblock lang:objc %}
[1,2, ,4,5].flat()          //[1,2,4,5]
{% endcodeblock %}
flatMap()方法对原数组的每个成员执行一个函数(相当于执行Array.prototype.map()),然后对返回值组成的数组执行flat()方法。该方法返回一个新数组，不改变原数组
{% codeblock lang:objc %}
//相当于[[2,40],[3,6],[4,8]].flat()
[2,3,4].flatMap((x)=>[x,x*2])  //[2,4,3,6,4,8]
{% endcodeblock %}
flatMap()只能展开一层数组。
{% codeblock lang:objc %}
//相当于[[[2]],[[4]]，[[6]],[[8]]].flat()
[1,2,3,4].flatMap(x=>[[x*2]])   //[[2],[4],[6],[8]]
{% endcodeblock %}
上面代码中，遍历函数返回的是一个双层的数组，但是默认只能展开一层，因此flatMap()返回的还是一个嵌套数组。
flatMap()方法的参数是一个遍历函数，该函数可以接受三个参数，分别是当前数组成员，当前数组成员位置(从零开始),原数组。
{% codeblock lang:objc %}
arr.flatMap(function callback(currentValue[,index[,array]]){
  // ...
}[,thisArg])
{% endcodeblock %}
flatMap()方法还可以有第二个参数，用来绑定遍历函数里面的this。
### 10.实例方法:at()
长久以来，JavaScript不支持数组的负索引，如果要引用数组的最后一个成员，不能写成arr[-1]，只能使用arr[arr.length - 1]。