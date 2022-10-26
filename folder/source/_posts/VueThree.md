---
title: Vue3学习之路
date: 2022-10-25 17：10
cover: 肚兜
tags: [vue,扩展,最新]
---
学习点最新的东西丰富下自己.....好吧，其实是没事干！！！！！
<!-- more -->
### 前言
 #### vue2 => vue3
 ##### 1.初始化
 vue2和vue3的初始化就存在着一定区别，比如vue3可以安装脚手架同时提前安装好一些项目开发必备的插件，并且3提供了可视化创建脚手架，可以更加方便地对插件和依赖进行管理和配置，同时两个版本的目录结构也有些许差异。
 ##### 2.使用方法
 ###### （1）双向绑定
 vue2的双向数据绑定是利用es5的一个API，Object.definePropert()对数据进行劫持结合发布订阅模式的方法实现；
 vue3中使用了es6的ProxyAPI对数据代理，通过reactive()函数给每一个对象都包一层Proxy，通过Proxy监听属性变化，从而实现对数据的监控。
 {% codeblock lang:objc %}
// 这⾥是引相⽐于vue2版本，使⽤proxy的优势如下
1.defineProperty只能监听某个属性，不能对全对象监听
可以省去for in、闭包等内容来提升效率（直接绑定整个对象即可）
2.可以监听数组，不⽤再去单独的对数组做特异性操作,通过Proxy可以直接拦截所有对象类型数据的操作，完美⽀持对数组的监听。
 {% endcodeblock %}
 ##### 3.新增内置的组件和方法
 ###### （1）默认进行懒观察
 使用Function-basedAPI,setup函数，对与插件或对象的一个按需引入，Computed Value，新加入了TypeScript以及PWA的支持等等
 ###### （2）按需引入
 vue2中new出的实例对象，所有的东西都在这个vue对象上，这样其实无论你用到还是没用到，都会跑一遍，这样不仅提高了性能消耗，也无疑新增了用户加载时间。

 而vue3中可以用ES module imports按需引入，如：keep-alive内置组件、v-model指令，等等。不仅我们开发起来更加的便捷，减少 了内存消耗，也同时减少了⽤户加载时间，优化⽤户体验
 ###### （3）创建项目文件发生的变化
 在main.js中：
 {% image ./main.png  download:./main.png fancybox:true %}
  {% codeblock lang:objc %}
//核心代码：
createApp(App).mount('#app') = createApp(根组件).mount('public/index.html中的div容器')

//1.vue2.0中是直接创建了一个vue实例
//2.vue3.0中按需导出了一个createApp （ceateApp做了什么）
//3.vue3中的app单文件不再强制要求必须有根元素 也就是说 在vue2.0中必须要有一个根元素，在vue3中没这个要求
  {% endcodeblock %}
##### 4.生命周期
 {% image ./smzq.png vue3生命周期 download:./smzq.png fancybox:true %}
vue3 中的生命周期函数, 需要在 setup 中调用
{% codeblock lang:objc %}
import { onMounted, onUpdated,onUnmounted } from 'vue'

const MyComponent = {
  setup() {
    onMounted(() => {
      console.log('mounted!')
    })
    onUpdated(() => {
      console.log('updated!')
    })
    onUnmounted(() => {
      console.log('unmounted!')
    })
  }
}
{% endcodeblock %}
##### 5.获取props
{% codeblock lang:objc %}
vue2：console.log(‘props’,this.xxx)
vue3：setup(props,context){ console.log(‘props’,props) }
{% endcodeblock %}
##### 6.给父组件传值emit
{% codeblock lang:objc %}
vue2：this.$emit()
vue3：setup(props,context){context.emit()}
{% endcodeblock %}
#### 组合式API
##### vue2-optionsAPI
1.优点：易于学习和使用，每个代码有着明确的位置(数据放data中，方法放methods中)
2.缺点：相似的逻辑，不容易复用，在大项目中尤为明显
3.虽然optionsAPI可以通过mixins提取相同的逻辑，但是也并不是特别好维护
##### vue3-compositionAPI
1.compositionAPI是基于逻辑功能组织代码的，一个功能api相关放到一起
2.即使项目大了，功能多了，也能快速定位功能相关的api
3.大大的提升了代码可读性和可维护性
##### {% mark vue3 推荐使用 composition API, 也保留了options API color:orange %}
#### 面试题
###### Vue3动机和新特性
{% quot Vue3设计理念：组合式API %}
动机与目的：
1.更好的逻辑复用与代码组织（{% mark composition组合式api color:purple %}）
optionsAPI(旧) => compositionAPI(新)
效果：代码组织更方便，逻辑复用更方便，非常利于维护
2.更好的类型推导（typescript支持）
vue3源码用了ts重写，vue3对ts的支持更友好。(ts可以让代码更加稳定)
{% quot Vue3新特性 %}
1.数据响应式原理重新实现（ES6 {% u proxy %} 替代了ES5 {% u Object.defineProperty %}）
解决了数组的更新检测等bug，大大优化了响应式监听的性能(原来检测对象属性的变化，需要一个个对属性递归监听){% u proxy %}可以直接对整个对象劫持
2.虚拟DOM-新算法(更小，更快)
3.提供了composition api 可以更加的逻辑复用
4.模板可以有多个根元素
5.源码用ts重写，废弃了eventbus过滤器