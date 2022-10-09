---
title: es6数组方法
date: 2022-10-09 09:34:51
cover: /folder/source/_posts/catk.jpg
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
{% mark 注意，只有函数调用时，扩展运算符才可以放在圆括号中，否则会报错 color:green %}