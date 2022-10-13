---
title: js原生实现表格模糊查询.....
date: 2022-10-13 15:40:00
cover: hope
tags: [js,表格]
---
遇到的一个功能总结一下，因为是原生的写法不常用，怕自己忘记在这里总结一下。有很多方法我用的是test
ps:因为不想整理,搬运自[大佬](https://blog.csdn.net/weixin_43867847?type=blog)的文章
<!-- more -->
## test
这里还用到了一个[new RegExp](https://baike.baidu.com/item/RegExp/11017063?fr=aladdin),是VBScritp5.0提供的“正则表达式”对象(只要你的服务器 安装了IE5.x，就会带VBScript5.0)。
{% codeblock lang:objc %}
/**
   * 使用test方法实现模糊查询
   * @param  {Array}  list     原数组
   * @param  {String} keyWord  查询的关键词
   * @return {Array}           查询的结果
   */
  function fuzzyQuery(list, keyWord) {
    var reg =  new RegExp(keyWord);
    var arr = [];
    for (var i = 0; i < list.length; i++) {
      if (reg.test(list[i])) {
        arr.push(list[i]);
      }
    }
    return arr;
  }
————————————————
版权声明：本文为CSDN博主「木方佳学」的原创文章，遵循CC 4.0 BY-SA版权协议，转载请附上原文出处链接及本声明。
原文链接：https://blog.csdn.net/weixin_43867847/article/details/118491381
{% endcodeblock %}
## indexOf
{% codeblock lang:objc %}
/**
   * 使用indexof方法实现模糊查询
   * @param  {Array}  list     进行查询的数组
   * @param  {String} keyWord  查询的关键词
   * @return {Array}           查询的结果
   */
  function fuzzyQuery(list, keyWord) {
    var arr = [];
    for (var i = 0; i < list.length; i++) {
      if (list[i].indexOf(keyWord) >= 0) {
        arr.push(list[i]);
      }
    }
    return arr;
  }
{% endcodeblock %}
## split
{% codeblock lang:objc %}
 /**
   * 使用spilt方法实现模糊查询
   * @param  {Array}  list     进行查询的数组
   * @param  {String} keyWord  查询的关键词
   * @return {Array}           查询的结果
   */
  function fuzzyQuery(list, keyWord) {
    var arr = [];
    for (var i = 0; i < list.length; i++) {
      if (list[i].split(keyWord).length > 1) {
        arr.push(list[i]);
      }
    }
    return arr;
  }
{% endcodeblock %}
## match
{% codeblock lang:objc %}
 /**
   * 使用match方法实现模糊查询
   * @param  {Array}  list     进行查询的数组
   * @param  {String} keyWord  查询的关键词
   * @return {Array}           查询的结果
   */
  function fuzzyQuery(list, keyWord) {
    var arr = [];
    for (var i = 0; i < list.length; i++) {
      if (list[i].match(keyWord) != null) {
        arr.push(list[i]);
      }
    }
    return arr;
  }
{% endcodeblock %}
————————————————
版权声明：本文为CSDN博主[木方佳学](https://blog.csdn.net/weixin_43867847?type=blog)的原创文章，遵循CC 4.0 BY-SA版权协议，转载请附上原文出处链接及本声明。
原文链接：https://blog.csdn.net/weixin_43867847/article/details/118491381