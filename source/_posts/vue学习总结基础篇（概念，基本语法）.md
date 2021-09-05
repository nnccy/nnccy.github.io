---
title: vue学习总结基础篇（概念，基本语法）
categories:
  - 前端
tags:
  - Vue
  - 前端
abbrlink: d3428e7e
copyright: true
date: 2020-09-16 16:01:12
---

vue学习笔记，用于自己查阅,[b站教程](https://www.bilibili.com/video/BV15741177Eh?share_source=copy_web) 学习笔记。
<!--more-->

# 一、VUE基础

## 1、概念部分

官网：Vue (读音 /vjuː/，类似于 view) 是一套用于构建用户界面的渐进式框架。与其它大型框架不同的是，Vue 被设计为可以自底向上逐层应用。Vue 的核心库只关注视图层，不仅易于上手，还便于与第三方库或既有项目整合。另一方面，当与现代化的工具链以及各种支持类库结合使用时，Vue 也完全能够为复杂的单页应用提供驱动。
有了Vue帮助我们完成VueModel层的任务，在后续的开发，我们就可以专注于数据的处理，以及DOM的编写工作了。

## 2、安装方式

1. 直接CDN引入，直接在html文件中添加如下代码
   开发版本
   `<script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>`
   生产环境版本
   `<script src="https://cdn.jsdelivr.net/npm/vue@2.6.12"></script>`
2. 下载和引入
   在[官网](https://cn.vuejs.org/v2/guide/installation.html)直接下载vue.js代码，并用`script`标签引入
3. 通过NPM安装
   使用webpack和cli时，用这种方式

## 3、第一个vue程序

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>

<div id="app">
    <h2>{{message}}</h2>
    <h1>{{name}}</h1>
</div>

<div>{{message}}</div>

<script src="../js/vue.js"></script>
<script>
    // let(变量)/const(常量)
    // 编程范式: 声明式编程
    const app = new Vue({
        el: '#app', // 用于挂载要管理的元素
        data: { // 定义数据
            message: '你好',
            name: 'coderwhy'
        }
    })


</script>

</body>
</html>
```

- vue做法
  创建Vue对象的时候，传入了一些options：{}
  {}中包含了el属性：该属性决定了这个Vue对象挂载到哪一个元素上，很明显，我们这里是挂载到了id为app的元素上
  {}中包含了data属性：该属性中通常会存储一些数据
  这些数据可以是我们直接定义出来的，比如像上面这样。
  也可能是来自网络，从服务器加载的。
  在标签中间加入`{{   }}`，可以直接使用vue实例中的数据
   - js的做法(编程范式: 命令式编程)
     1.创建div元素,设置id属性
      2.定义一个变量叫message
      3.将message变量放在前面的div元素中显示
      4.修改message的数据: 今天天气不错!
      5.将修改后的数据再次替换到div元素

## 4、创建vue实例中传入的options

1. el挂载点
   决定之后vue实例会管理哪一个DOM
2. data
   实例对应的数据对象
3. methods
   定义vue的一些方法

# VUE语法

## 计算属性的使用

1、我们知道可以通过`{{  }}`显示一些data中的数据
但在一些情况我们需要对数据进行一些转化后再显示，比如我们有firstName和lastName两个变量，我们需要显示完整的名称，那么差值语法就是{{firstName}}+{{lastName}}，这样显得很啰嗦。
我们可以将其改为计算属性显示

```html
 <h2>{{fullName}}</h2>
 .........
computed: {
      fullName: function () {
        return this.firstName + ' ' + this.lastName
      }
    }
```

计算属性内部看似是一个函数，但是`{{  }}`中的“函数”可以不用加小括号（），而函数要达到相同效果，就如下个例子，需要加上（）

```html
<h2>{{getFullName()}}</h2>
...............
methods: {
      getFullName() {
        return this.firstName + ' ' + this.lastName
      }
    }
```

2、计算属性也可以进行一些更加复杂的操作
例：计算图书的总价格

```html
 <h2>总价格: {{totalPrice}}</h2>
..................
data: {
      books: [
        {id: 110, name: 'Unix编程艺术', price: 119},
        {id: 111, name: '代码大全', price: 105},
        {id: 112, name: '深入理解计算机原理', price: 98},
        {id: 113, name: '现代操作系统', price: 87},
      ]
    },
    computed: {
      totalPrice: function () {
        let result = 0
        for (let i=0; i < this.books.length; i++) {
          result += this.books[i].price
        }
        return result
      }
    }
```

补充：methods和computed看起来都可以实现我们的功能，那么为什么还要多一个计算属性这个东西呢?计算属性会进行缓存，如果多次使用时，计算属性只会调用一次。而方法会调用多次，造成缓存增大。

## v-once指令

在某些情况下，我们希望data第一次加载之后，界面不能随意的改变
`<h2 v-once>{{message}}</h2>`
这种情况下message不能随意更改。方法也不可以，一次定义。

##  v-text指令

v-text作用和`{{  }}`比较相似：都是用于将数据显示在界面中
`<h2 v-text="message"> </h2>`

## v-html指令

某些情况下，我们从服务器请求到的数据本身就是一个HTML代码
如果我们直接通过`{{ }}`来输出，会将HTML代码也一起输出。
但是我们可能希望的是按照HTML格式进行解析，并且显示对应的内容。
例：

```html
data: {
      message: '你好啊',
      url: '<a href="http://www.baidu.com">百度一下</a>'
```

那我们就可以通过
`<h2 v-html="url"></h2>`
将其解析出来
![解析后的效果](2020091823545529.png)

## v-pre 

v-pre用于跳过这个元素和它子元素的编译过程，用于显示原本的Mustache语法。
例 

```html
  <h2>{{message}}</h2>
  <h2 v-pre>{{message}}</h2>
```

第一个h2元素中的内容会被编译解析出来对应的内容
第二个h2元素中会直接显示{{message}}
![在这里插入图片描述](20200919095226188.png)

## v-cloak指令

在vue解析之前，界面还没有被渲染，可能界面会直接出现{{message}},也就是卡住，那我们可以使用v-cloak指令，在vue解析之前, div中有一个属性v-cloak，属性v-cloak消失。那我们可以在style标签中设置vue还没解析之前，v-cloak指令所在的标签该如何显示。

```html
<head>
  <meta charset="UTF-8">
  <title>Title</title>
  <style>
    [v-cloak] {
      display: none;
    }
  </style>
</head>
<body>

<div id="app" v-cloak>
  <h2>{{message}}</h2>
</div>
```

## v-bind指令

### 简介

前面我们学习的指令主要作用是将值插入到我们模板的内容当中。
除了内容需要动态来决定外，某些属性我们也希望动态来绑定。
比如动态绑定a元素的href属性
比如动态绑定img元素的src属性

```html
 //错误的做法: 这里不可以使用mustache语法
  <img src="{{imgURL}}" alt="">
```

  ```html
  <!-- 正确的做法: 使用v-bind指令 -->
   <img v-bind:src="imgURL" alt="">
  <a v-bind:href="aHref">百度一下</a>


........


   <script>
  const app = new Vue({
    el: '#app',
    data: {
      message: '你好啊',
      imgURL: 'https://img11.360buyimg.com/mobilecms/s350x250_jfs/t1/20559/1/1424/73138/5c125595E3cbaa3c8/74fc2f84e53a9c23.jpg!q90!cc_350x250.webp',
      aHref: 'http://www.baidu.com'
    }
  })
  </script>
  ```

这样我们就可以向属性传值。

### 简写

`<img v-bind:src="imgSrc" alt="">`
可以简写成为
`<img  :src="imgSrc" alt="">`

### v-bind绑定class

有时我们希望动态的切换class,比如根据数据变化改变字体颜色。

1. 对象语法，:class后面跟一个对象

- 用法一：直接通过{}绑定一个类
  `<h2 :class="{'active': isActive}">Hello World</h2>`
- 用法二：也可以通过判断，传入多个值,其中isActive,isLine是布尔值，通过它们的值来确定相应类是否存在
  `<h2 :class="{'active': isActive, 'line': isLine}">Hello `
- 用法四：如果过于复杂，可以放在一个methods或者computed中
  注：classes是一个计算属性,它是一个函数，可以return相应的值
  `<h2 class="title" :class="classes">Hello World</h2>`

```html
getClasses: function () {
        return {active: this.isActive, line: this.isLine}
      }
```

2. 数组语法
   数组语法的含义是:class后面跟的是一个数组。
   `<h2 :class=“[‘active’, 'line']">Hello World</h2>`
   注意：去掉引号是active变量

### v-bind绑定style

我们可以利用v-bind:style来绑定一些CSS内联样式。
在写CSS属性名的时候，比如font-size
我们可以使用驼峰式 (camelCase)  fontSize 
或短横线分隔 (kebab-case，记得用单引号括起来) ‘font-size’
绑定class有两种方式：

1. 对象语法
   `<!--<h2 :style="{key(属性名): value(属性值)}">{{message}}</h2>-->`

例1：
`<!--<h2 :style="{fontSize: '50px'}">{{message}}</h2>-->`
其中，50px'必须加上单引号, 否则是当做一个变量去解析

例2：
finalSize当成一个变量使用,
  `<!--<h2 :style="{fontSize: finalSize}">{{message}}</h2>-->`

  `<h2 :style="{fontSize: finalSize + 'px', backgroundColor: finalColor}">{{message}}</h2>`
  例3：使用函数返回值为对象传值

  ```html
<h2 :style="getStyles()">{{message}}</h2>

    methods: {
    getStyles: function () {
      return {fontSize: this.finalSize + 'px', backgroundColor: this.finalColor}
    }
  }
  ```

3. 数组语法
   当要改动的样式比较多的时候，可以考虑用一下数组语法

```html
<h2 :style="[baseStyle, baseStyle1]">{{message}}</h2>

    data: {
      message: '你好啊',
      baseStyle: {backgroundColor: 'red'},
      baseStyle1: {fontSize: '100px'},
    }
```

## v-on指令

1、v-on指令用于事件监听，与用户交互，缩写是@
例，计数器

```html
<div id="app">
  <h2>{{counter}}</h2>
  <button @click="increment">+</button>
  <button @click="decrement">-</button>
</div>
<script src="../js/vue.js"></script>
<script>
  const app = new Vue({
    el: '#app',
    data: {
      counter: 0
    },
    methods: {
      increment() {
        this.counter++
      },
      decrement() {
        this.counter--
      }
    }
  })
</script>
```

2、参数问题
当方法中无参传入时，如上例所示，可省略小括号。

3、v-on修饰符

- .stop - 调用 event.stopPropagation()。
  阻止子元素方法事件冒泡到父元素。例：

```html
 <div @click="divClick">
    aaaaaaa
    <button @click.stop="btnClick">按钮</button>
  </div>
```

若不使用.stop修饰符，在点击按钮时，父元素的divClick方法也会被执行，这种情况一般是我们不想遇到的。

- .prevent - 调用 event.preventDefault()。
  阻止元素发生默认的行为
- . @keyup.enter="keyUp" - 
  监听某个键盘的键帽，此例只有按下回车才会执行回调。。
- .native - 监听组件根元素的原生事件。
- .once - 只触发一次回调。

## v-if指令

v-if、v-else-if、v-else
条件指令，可根据表达式的值在DOM中渲染销毁组件
v-if的原理：
v-if后面的条件为false时，对应的元素以及其子元素不会渲染。
也就是根本没有不会有对应的标签出现在DOM中。
例：根据学生分数值，在页面显示优秀，良好，及格等标签

```html
  <h2 v-if="score>=90">优秀</h2>
  <h2 v-else-if="score>=80">良好</h2>
  <h2 v-else-if="score>=60">及格</h2>
  <h2 v-else>不及格</h2>
```


## v-show指令

` <h2 v-show="isShow" id="bbb">{{message}}</h2>`
当isShow为true时,message内容显示，否则内容消失

v-show的用法和v-if非常相似，也用于决定一个元素是否渲染：
两者的区别：
v-if当条件为false时，压根不会有对应的元素在DOM中。
v-show当条件为false时，仅仅是将元素的display属性设置为none而已。


## v-for指令

### v-for遍历数组

格式：v-for=(item, index) in 数组
其中的index就代表了取出的item在原数组的索引值。
`<li v-for="item in names">{{item}}</li>`

### v-for遍历对象

v-for可以用户遍历对象：
比如某个对象中存储着你的个人信息，我们希望以列表的形式显示出来。

```html
 <ul>
    <li v-for="item in info">{{item}}</li>
  </ul>

  <!--2.获取key和value 格式: (value, key) -->
  <ul>
    <li v-for="(value, key) in info">{{value}}-{{key}}</li>
  </ul>
  
  <!--3.获取key和value和index 格式: (value, key, index) -->
  <ul>
    <li v-for="(value, key, index) in info">{{value}}-{{key}}-{{index}}</li>
  </ul>

......
data: {
      info: {
        name: 'why',
        age: 18,
        height: 1.88
      }
      }
```

## v-model指令

1、简介
v-model用于表单绑定
来实现表单元素和数据的双向绑定。
当我们在输入框输入内容时
因为input中的v-model绑定了message，所以会实时将输入的内容传递给message，message发生改变。
当message发生改变时，因为上面我们使用Mustache语法，将message的值插入到DOM中，所以DOM会发生响应的改变。
所以，通过v-model实现了双向的绑定。
2、原理
v-model其实是一个语法糖，它的背后本质上是包含两个操作：

1. v-bind绑定一个value属性
2. v-on指令给当前元素绑定input事件
   ![在这里插入图片描述](20200926105854937.png)

3、radio
当存在多个单选框时
lable标签：为input元素定义标注，其不会为用户呈现任何特殊效果，他的作用是在label元素中点击文本，则会触发控件，其for属性应当与相关元素的 id 属性相同。for" 属性可把 label 绑定到另外一个元素，规定应与哪个表单元素绑定。
input 标签规定了用户可以在其中输入数据的输入字段。

- 输入字段可通过多种方式改变，取决于 type 属性。type 属性规定要显示的 input元素的类型。
- value属性指定 `<input>` 元素 value 的值
  例程：

```html
<div id="app">
  <label for="male">
    <input type="radio" id="male" value="男" v-model="sex">男
  </label>
  <label for="female">
    <input type="radio" id="female" value="女" v-model="sex">女
  </label>
  <h2>您选择的性别是: {{sex}}</h2>
</div>

<script src="../js/vue.js"></script>
<script>
  const app = new Vue({
    el: '#app',
    data: {
      message: '你好啊',
      sex: '女'
    }
  })
</script>
```

4、checkbox	
定义复选框。
例程：

```html
  <h2>您的爱好是: {{hobbies}}</h2>

  <label v-for="item in originHobbies" :for="item">
    <input type="checkbox" :value="item" :id="item" v-model="hobbies">{{item}}
  </label>
```

5、select
select标签用来创建下拉列表。
select 标签中的 <option> 标签定义了列表中的可用选项。
multiple	当该属性为 true 时，可选择多个选项。
例程：

```html
  <select name="abc" v-model="fruit">
    <option value="苹果">苹果</option>
    <option value="香蕉">香蕉</option>
    <option value="榴莲">榴莲</option>
    <option value="葡萄">葡萄</option>
  </select>
  <h2>您选择的水果是: {{fruit}}</h2>
```

6、v-model修饰符

- lazy修饰符：
  默认情况下，v-model默认是在input事件中同步输入框的数据的。
  lazy修饰符可以让数据在失去焦点或者回车时才会更新：
- number修饰符：
  默认情况下，在输入框中无论我们输入的是字母还是数字，都会被当做字符串类型进行处理。
  number修饰符可以让在输入框中输入的内容自动转成数字类型：
- trim修饰符：
  如果输入的内容首尾有很多空格，通常我们希望将其去除
  trim修饰符可以过滤内容左右两边的空格

## 检测数组更新

因为Vue是响应式的，所以当数据发生变化时，Vue会自动检测数据变化，视图会发生对应的更新。
Vue中包含了一组观察数组编译的方法，使用它们改变数组也会触发视图的更新。

```vue
		// 1.push() 在数组最后添加元素
         //this.letters.push('aaa')
        // this.letters.push('aaaa', 'bbbb', 'cccc')

        // 2.pop(): 删除数组中的最后一个元素
        // this.letters.pop();

        // 3.shift(): 删除数组中的第一个元素
        // this.letters.shift();

        // 4.unshift(): 在数组最前面添加元素
        // this.letters.unshift()
        // this.letters.unshift('aaa', 'bbb', 'ccc')

        // 5.splice作用: 删除元素/插入元素/替换元素
        //第一个参数，定位开始位置
        // 删除元素: 第二个参数传入你要删除几个元素(如果没有传,就删除后面所有的元素)
        // 替换元素: 第二个参数, 表示我们要替换几个元素, 后面是用于替换前面的元素
        // 插入元素: 第二个参数, 传入0, 并且后面跟上要插入的元素
        // splice(start)
        // splice(start):
        //this.letters.splice(1, 3, 'm', 'n', 'l', 'x')
        // this.letters.splice(1, 0, 'x', 'y', 'z')

        // 5.sort()    数组排序
        // this.letters.sort()

        // 6.reverse()  数组翻转
        // this.letters.reverse()
```

// 注意: 通过索引值修改数组中的元素
         this.letters[0] = 'bbbbbb';  这种界面不能实时更新
        

        // set(要修改的对象, 索引值, 修改后的值)
        // Vue.set(this.letters, 0, 'bbbbbb'

##  js高阶函数

###  1.filter函数的使用 

filter函数，用来过滤符合要求的数组中的值
filter中的回调函数有一个要求: 必须返回一个boolean值
true: 当返回true时, 函数内部会自动将这次回调的n加入到新的数组中
false: 当返回false时, 函数内部会过滤掉这次的n
例：
给出一个数组  const nums = [10, 20, 111, 222, 444, 40, 50]
挑出所有少于100的值

```html
 let newNums = nums.filter(function (n) {
   return n < 100
 })
 console.log(newNums);
```

###  2.map函数的使用

对数组中的值进行处理，例如都乘二

```html
let new2Nums = newNums.map(function(n)){
	return n*2;
}
 console.log(newNums);
```

### 3.reduce函数的使用

对数组中的元素进行汇总
例：求和

```html
 let total = new2Nums.reduce(function (preValue, n) {
  return preValue + n
 }, 0)
 console.log(total);
```

其中上面的0，为初始值，preValue值为每一次运算返回的值

### 4，三种汇总

```vue
let total = nums.filter(function (n) {
  return n < 100
}).map(function (n) {
  return n * 2
}).reduce(function (prevValue, n) {
  return prevValue + n
}, 0)
console.log(total);
```

## 组件化开发

### 一、概念

将复杂的问题拆解成简单的小问题，便于处理，放在网页中，就是将一个页面分成一个个小的功能块，每个功能块完成属于自己这部分独立的功能，便于后续地管理和扩展，也便于移植，可复用
![watermark](watermark.png)

### 二、使用步骤

分为

- 创建组件构造器（调用Vue.extend()方法，template下加入模板）
- 注册组件 （调用Vue.component(方法)）
- 使用组件 （在Vue实例范围内使用）

代码：

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Title</title>
</head>
<body>

<div id="app">
  <my-cpn></my-cpn> 
  <!--使用组件-->
</div>

<script src="../js/vue.js"></script>
<script>
  //创建组件构造器对象

  const cpnC = Vue.extend({
    template:`
    <div>
      <h2>我是标题</h2>
      <p>我是内容1</p>
      <p>我是内容2</p>
    </div>
    `
  })

  //注册组件
  Vue.component('my-cpn',cpnC)

    const app = new Vue({
    el: '#app',
    data: {

    }
  })

</script>
</body>
</html>

```

### 3、全局组件和局部组件

当我们通过调用Vue.component()注册组件时，组件的注册是全局的
这意味着该组件可以在任意Vue示例下使用。
如果我们注册的组件是挂载在某个实例中, 那么就是一个局部组件
例：下面组件挂载到app中，那么就只能在id=app下使用。

```html
<div id="app">
  <cpn></cpn>
  <cpn></cpn>
  <cpn></cpn>
</div>

<div id="app2">
  <cpn></cpn>
</div>

<script src="../js/vue.js"></script>
<script>
  // 1.创建组件构造器
  const cpnC = Vue.extend({
    template: `
      <div>
        <h2>我是标题</h2>
        <p>我是内容,哈哈哈哈啊</p>
      </div>
    `
  })

  // 2.注册组件(全局组件, 意味着可以在多个Vue的实例下面使用)
  // Vue.component('cpn', cpnC)

  // 疑问: 怎么注册的组件才是局部组件了?

  const app = new Vue({
    el: '#app',
    data: {
      message: '你好啊'
    },
    components: {
      // cpn使用组件时的标签名
      cpn: cpnC
    }
  })

  const app2 = new Vue({
    el: '#app2'
  })
</script>

```

### 父组件和子组件

组件和组件之间存在层级关系
而其中一种非常重要的关系就是父子组件的关系
将子组件注册到父组件的组件构造器中
子组件不能直接使用，通过父组件调用。

```html
<script src="../js/vue.js"></script>
<script>
  // 1.创建第一个组件构造器(子组件)
  const cpnC1 = Vue.extend({
    template: `
      <div>
        <h2>我是标题1</h2>
        <p>我是内容, 哈哈哈哈</p>
      </div>
    `
  })
  // 2.创建第二个组件构造器(父组件)
  const cpnC2 = Vue.extend({
    template: `
      <div>
        <h2>我是标题2</h2>
        <p>我是内容, 呵呵呵呵</p>
        <cpn1></cpn1>
      </div>
    `,
    components: {
      cpn1: cpnC1
    }
  })
  // root组件
  const app = new Vue({
    el: '#app',
    data: {
      message: '你好啊'
    },
    components: {
      cpn2: cpnC2
    }
  })
</script>
```

### 注册组件的简写

在注册之时将组件构造器加到注册时
简写前:

```html
  const cpnC = Vue.extend({
    template:`
    <div>
      <h2>我是标题</h2>
      <p>我是内容1</p>
      <p>我是内容2</p>
    </div>
    `
  })
  //注册组件
  Vue.component('my-cpn',cpnC)

    const app = new Vue({
    el: '#app',
    data: {

    }
  })
```

简写后：

- 全局组件

```html
  Vue.component('cpn1', {
    template: `
      <div>
        <h2>我是标题1</h2>
        <p>我是内容, 哈哈哈哈</p>
      </div>
    `
  })

```

- 局部组件

```html
  const app = new Vue({
    el: '#app',
    data: {
      message: '你好啊'
    },
    components: {
      'cpn2': {
        template: `
          <div>
            <h2>我是标题2</h2>
            <p>我是内容, 呵呵呵</p>
          </div>
    `
      }
    }
  })
```

### 模板分离写法

以上组件html代码都在template中，我们希望将html代码分离出来，然后挂载到相应组件中，这样更加清晰
给template标签加个id,然后再template中调用

```html
<template id="cpn">
  <div>
    <h2>{{title}}</h2>
    <p>我是内容,呵呵呵</p>
  </div>
</template>

<script src="../js/vue.js"></script>
<script>

  // 1.注册一个全局组件
  Vue.component('cpn', {
    template: '#cpn',
    data() {
      return {
        title: 'abc'
      }
    }
  })
```

### 组件数据的存放

Vue组件有自己保存数据的地方。组件对象也有一个data属性，只是这个data属性必须是一个函数。而且这个函数返回一个对象，对象内部保存着数据

```html
Vue.component('cpn', {
    template: '#cpn',
     data() {
       return {
         counter: 0
       }
     },

    methods: {
      increment() {
        this.counter++
      },
      decrement() {
        this.counter--
      }
    }
  })
```

### 组件通信：

如何进行父子组件间的通信呢？Vue官方提到
通过props向子组件传递数据
通过事件向父组件发送消息
![watermark](watermark.png)

####  父组件向子组件传递数据

在开发中，往往一些数据确实需要从上层传递到下层：
比如在一个页面中，我们从服务器请求到了很多的数据。
其中一部分数据，并非是我们整个页面的大组件来展示的，而是需要下面的子组件进行展示。
这个时候，并不会让子组件再次发送一个网络请求，而是直接让大组件(父组件)将数据传递给小组件(子组件)
Vue实例和子组件的通信和父组件和子组件的通信过程是一样的。
props基本用法
在组件中，使用选项props来声明需要从父级接收到的数据。
props的值有两种方式：
方式一：字符串数组，数组中的字符串就是传递时的名称。
方式二：对象，对象可以设置传递时的类型，也可以设置默认值等。（常用）

1. 一个最简单的props传递。

```html
<div id="app">
  <child-cpn :message="message"></child-cpn>
  
</div>

<template id="childCpn">
  <div>
 显示的信息：{{message}}
  </div>
</template>
<script src="../js/vue.js"></script>
<script>
  // 1.注册组件

  const app = new Vue({
    el: '#app',
    data: {
      message: '你好啊'
    },
    components:{
      'child-cpn':{
        template:'#childCpn',
        props:['message']  //props传值
      }
    }

  })
</script>

```

2. 传对象值，可以设置默认类型

```html
<script>
  // 父传子: props
  const cpn = {
    template: '#cpn',
    // props: ['cmovies', 'cmessage'],
    props: {
      // 1.类型限制
      // cmovies: Array,
      // cmessage: String,
      // 2.提供一些默认值, 以及必传值
      cmessage: {
        type: String,
        default: 'aaaaaaaa',
        required: true
      },
      // 类型是对象或者数组时, 默认值必须是一个函数
      cmovies: {
        type: Array,
        default() {
          return []
        }
      }
    },
    data() {
      return {}
    },
    methods: {

    }
  }

  const app = new Vue({
    el: '#app',
    data: {
      message: '你好啊',
      movies: ['海王', '海贼王', '海尔兄弟']
    },
    components: {
      cpn
    }
  })
```

####  子传父

当子组件需要向父组件传递数据时，就要用到自定义事件了。
我们之前学习的v-on不仅仅可以用于监听DOM事件，也可以用于组件间的自定义事件。
自定义事件的流程：

- 在子组件中，通过$emit()来触发事件。
- 在父组件中，通过v-on来监听子组件事件。

```html
<!--父组件模板-->
<div id="app">
  <cpn @item-click="cpnClick"></cpn>
</div>

<!--子组件模板-->
<template id="cpn">
  <div>
    <button v-for="item in categories"
            @click="btnClick(item)">
      {{item.name}}
    </button>
  </div>
</template>

<script src="../js/vue.js"></script>
<script>

  // 1.子组件
  const cpn = {
    template: '#cpn',
    data() {
      return {
        categories: [
          {id: 'aaa', name: '热门推荐'},
          {id: 'bbb', name: '手机数码'},
          {id: 'ccc', name: '家用家电'},
          {id: 'ddd', name: '电脑办公'},
        ]
      }
    },
    methods: {
      btnClick(item) {
        // 发射事件: 自定义事件
        this.$emit('item-click', item)
      }
    }
  }

  // 2.父组件
  const app = new Vue({
    el: '#app',
    data: {
      message: '你好啊'
    },
    components: {
      cpn
    },
    methods: {
      cpnClick(item) {
        console.log('cpnClick', item);
      }
    }
  })
</script>

```

### 父子之间

有时候我们需要父组件直接访问子组件，子组件直接访问父组件，或者是子组件访问跟组件。
父组件访问子组件：使用`$children`或`$refs reference`(引用)
子组件访问父组件：使用`$parent`

#### 父访问子

1. $children方式 ,(数组方式)

```
         console.log(this.$children);
         for (let c of this.$children) {
           console.log(c.name);
           c.showMessage();
        }
         console.log(this.$children[3].name);
```

2，`$refs =>` 对象类型, 默认是一个空的对象
通过`$children`访问子组件时，是一个数组类型，访问其中的子组件必须通过索引值。
但是当子组件过多，我们需要拿到其中一个时，往往不能确定它的索引值，甚至还可能会发生变化。
有时候，我们想明确获取其中一个特定的组件，这个时候就可以使用`$refs`

`$refs` 的使用：
`$refs` 和ref指令通常是一起使用的。
首先，我们通过ref给某一个子组件绑定一个特定的ID。
其次，通过`this.$refs.ID`就可以访问到该组件了。

```html
<div id="app">
  <cpn></cpn>
  <cpn></cpn>
  <my-cpn></my-cpn>
  <y-cpn></y-cpn>
  <cpn ref="aaa"></cpn>
  <button @click="btnClick">按钮</button>
</div>

<template id="cpn">
  <div>我是子组件</div>
</template>
<script src="../js/vue.js"></script>
<script>
  const app = new Vue({
    el: '#app',
    data: {
      message: '你好啊'
    },
    methods: {
      btnClick() {
        
        console.log(this.$refs.aaa.name);
      }
    },
    components: {
      cpn: {
        template: '#cpn',
        data() {
          return {
            name: '我是子组件的name'
          }
        },
        methods: {
          showMessage() {
            console.log('showMessage');
          }
        }
      },
    }
  })
</script>
```

#### 子访问父（避免使用）

如果我们想在子组件中直接访问父组件，可以通过$parent

```html
<div id="app">
  <cpn></cpn>
</div>

<template id="cpn">
  <div>
    <h2>我是cpn组件</h2>
    <ccpn></ccpn>
  </div>
</template>

<template id="ccpn">
  <div>
    <h2>我是子组件</h2>
    <button @click="btnClick">按钮</button>
  </div>
</template>

<script src="../js/vue.js"></script>
<script>
  const app = new Vue({
    el: '#app',
    data: {
      message: '你好啊'
    },
    components: {
      cpn: {
        template: '#cpn',
        data() {
          return {
            name: '我是cpn组件的name'
          }
        },
        components: {
          ccpn: {
            template: '#ccpn',
            methods: {
              btnClick() {
                 1.访问父组件$parent
                 console.log(this.$parent);
                 console.log(this.$parent.name);

                 2.访问根组件$root
                console.log(this.$root);
                console.log(this.$root.message);
              }
            }
          }
        }
      }
    }
  })
</script>

```

### 组件化高级

#### slot（插槽）

组件的插槽是为了让我们封装的组件更加具有扩展性。
让使用者可以决定组件内部的一些内容到底展示什么。
最好的封装方式就是将共性抽取到组件中，将不同暴露为插槽。
一旦我们预留了插槽，就可以让使用者根据自己的需求，决定插槽中插入什么内容。
插槽的基本使用 <slot></slot>
插槽的默认值 <slot>button</slot>
如果有多个值, 同时放入到组件进行替换时, 一起作为替换元素
例：在模板中加入插槽，默认值是`<button>`,然后在使用时可以加入新标签进行替换。

```html
<div id="app">
  <cpn></cpn>

  <cpn><span>哈哈哈</span></cpn>
  <cpn><i>呵呵呵</i></cpn>
  <cpn>
    <i>呵呵呵</i>
    <div>我是div元素</div>
    <p>我是p元素</p>
  </cpn>

  <cpn></cpn>
  <cpn></cpn>
  <cpn></cpn>
  <cpn></cpn>
</div>


<template id="cpn">
  <div>
    <h2>我是组件</h2>
    <p>我是组件, 哈哈哈</p>
    <slot><button>按钮</button></slot>
    <!--<button>按钮</button>-->
  </div>
</template>
```

#### 具名插槽

确定插槽中的某个位置

```html
<div id="app">
  <cpn><span slot="center">标题</span></cpn>
  <cpn><button slot="left">返回</button></cpn>
</div>


<template id="cpn">
  <div>
    <slot name="left"><span>左边</span></slot>
    <slot name="center"><span>中间</span></slot>
    <slot name="right"><span>右边</span></slot>
  </div>
</template>
```

