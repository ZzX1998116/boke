---
title: 基本数据类型和引用数据类型的区别--面试题
date: 2022-10-13 15:40:00
cover: ./uncategorized/basicQuote/dm2.png
cover_info:
  meta: 基本数据类型和引用数据类型的区别
  title: 核心就是数据存储位置的区别
tags: [面试题]
---
<!-- more -->
## 存储上的区别
基本数据类型是存放在栈中的简单数据段
引用数据类型是存放在堆内存中的对象，在栈内存中存放的是堆内存中具体内容的引入地址，通过这个地址可以快速查找到对象
## 比较上的区别
基本数据类型的比较是值的比较，直接比较值，看起来一样就是一样
引用数据类型的比较是引用的比较，比较的是地址。也就是比较两个对象保存在栈区的指向堆内存的地址是否相同，虽然看起来一样，但是他们指向堆内存的地址是不一样的。
## 赋值上的区别
基本数据类型的赋值是简单赋值，如果一个变量向另一个变量赋值基本类型的值，会在变量对象上创建一个新值，然后把这个值复制到为新变量分配的位置上。
引用数据类型的赋值是对象引用，当一个变量向另一个变量赋值引用类型的值时，同样也会将栈内存中的值复制一份放在新变量分配的空间中，但是引用数据类型保存在栈内存中的变量是一个地址，这个地址指向的是堆内存中的对象，所以这个变量其实复制了一个地址，两个地址指向同一个对象，改变其中任意一个变量都会互相影响。
