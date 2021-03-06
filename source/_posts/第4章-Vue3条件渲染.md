---
title: 第4章 Vue3[条件]渲染
date: 2022-02-27 15:43:04
tags: vue
categories:
  - - WEB前端
    - 框架
description: 
---

> 文档参考地址：https://cn.vuejs.org/v2/guide/conditional.html#v-if-vs-v-show
>
> **区别**
>
> - 1.手段：v-if是通过控制dom节点的存在与否来控制元素的显隐；v-show是通过设置DOM元素的display样式，block为显示，none为隐藏；
> - 2.编译过程：v-if切换有一个局部编译/卸载的过程，切换过程中合适地销毁和重建内部的事件监听和子组件；v-show只是简单的基于css切换；
> - 3.编译条件：v-if是惰性的，如果初始条件为假，则什么也不做；只有在条件第一次变为真时才开始局部编译（编译被缓存？编译被缓存后，然后再切换的时候进行局部卸载); v-show是在任何条件下（首次条件是否为真）都被编译，然后被缓存，而且DOM元素保留；
> - 4.性能消耗：v-if有更高的切换消耗；v-show有更高的初始渲染消耗；
>
> **使用场景**
>
> 基于以上区别，因此，如果需要非常频繁地切换，则使用 v-show 较好；如果在运行时条件很少改变，则使用 v-if 较好。
>
> **总结**
>
> `v-if`判断是否加载，可以减轻服务器的压力，在需要时加载,但有更高的切换开销
>
> `v-show`调整DOM元素的CSS的`display`属性，可以使客户端操作更加流畅，但有更高的初始渲染开销。如果需要<strong>非常频繁</strong>地切换，则使用 `v-show` 较好；如果在运行时条件很少改变，则使用` v-if `较好。

### 条件渲染

**代码**

```js
<div id="root"></div>
    <script>
      Vue.createApp({
        //数据
        data() {
          return {
            show: false,
            message1: "XL",
            message2: "LX",
            conditionOne: false,
            conditioTwo: true,
          };
        },
        //if-else条件判断,必须在一起使用
        template: `
        <p v-if="show">{{ message1 }}</p> 
        <p v-show="show">{{ message2 }}</p> 

        
        <h4 v-if='conditionOne'>if</h4>
        <h4 v-else-if="conditioTwo">v-else-if</h4>
        <h4 v-else>else</h4>

        `,
      }).mount("#root");
    </script>
```

**图示**

![](https://cdn.jsdelivr.net/gh/webyang-male/yangimgs/vueifshow.png)

### 列表循环渲染

教程：https://v3.cn.vuejs.org/guide/list.html#%E7%BB%B4%E6%8A%A4%E7%8A%B6%E6%80%81

为了给 Vue 一个提示，以便它能跟踪每个节点的身份，从而重用和重新排序现有元素，你需要为每项提供一个唯一的 `key` attribute：

```html
<div v-for="item in items" :key="item.id">
  <!-- 内容 -->
</div>
```

[建议](https://v3.cn.vuejs.org/style-guide/#keyed-v-for-essential)尽可能在使用 `v-for` 时提供 `key` attribute，除非遍历输出的 DOM 内容非常简单，或者是刻意依赖默认行为以获取性能上的提升。

```js
    <script>
      Vue.createApp({
        //数据
        data() {
          return {
            show: true,
            show1: false,
            uid: 0,
            array: ["XL", "is", "a", "beauty"],
            listObj: {
              name: "LX",
              age: 18,
            },
          };
        },
        methods: {
          handerAddItem() {
            this.array.push(`new item` + (++this.uid));
          },
        },
        template: `
        <ul v-show="show">
          <li v-for="(item,index) in array " :key="item">{{++index}}---{{ item }}</li>
        </ul>

        <ul v-show="show1">
          <li v-for="(value,key,index) in listObj">{{value}}---{{ key }} #{{index}}</li>
        </ul>

        <button @click="handerAddItem">增加</button>
        `,
      }).mount("#root");
    </script>
```

#### 拓展

````html
  <body>
    <div id="root"></div>
  </body>
  <script>
    const app = Vue.createApp({
      data() {
        return {
          listArray: ["Li", "xia", "friends"],
          listObject: {
            firstName: "Li",
            lastName: "xia",
            relationship: "friends",
          },
        };
      },
      methods: {
        handleAddBtnClick() {
          // 1. 使用数组的变更函数 push, pop, shift, unshift, splice, sort, reverse
          // this.listArray.push('hello');
          // this.listArray.pop();
          // this.listArray.shift();
          // this.listArray.unshift('hello');
          // this.listArray.reverse();

          // 2. 直接替换数组
          // this.listArray = ['bye', 'world']
          // this.listArray = ['bye', 'wolrd'].filter(item => item === 'bye');

          // 3. 直接更新数组的内容
          // this.listArray[1] = 'hello'
          // 直接添加对象的内容，也可以自动的展示出来
          // this.listObject.age = 100;
          // this.listObject.sex = 'male';
        },
      },
      //template标签相当于占位符,可解决dom元素嵌套如div套div
      template: `
      <div>
        <template
          v-for="(value, key, index) in listObject"
          :key="index"
        >
          <div v-if="key !== 'lastName'">
            {{value}} -- {{key}}
          </div>
        </template>
        <div v-for="item in 10">{{item}}</div>
        <button @click="handleAddBtnClick">新增</button>
      </div>
    `,
    });

    const vm = app.mount("#root");
  </script>
````

