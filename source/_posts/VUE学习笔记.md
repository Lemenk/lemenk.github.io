---

title: VUE学习笔记
date: 2020-03-08 21:33:21
author: Lemenk
summary: vue.js学习笔记
categories: vue
tags:
  - 前端
  - vue
---

# Vue基础

## Vue简介

**它是一种渐进式JavaScript 框架，是一套用于构建用户界面的渐进式框架**。

渐进式是指使用者可以将Vue作为应用的一部分嵌入其中，带来更丰富的交互体验，也可以使用更多的Vue核心库去进行开发。

vue 的核心库只关注视图层，不仅易于上手，还便于与第三方库或既有项目整合

官网：https://cn.vuejs.org/

Vue是目前最火的前端框架，与Angular、React并称为前端三大框架。

- 易用： HTML、CSS、JavaScript后就可以快速上手vue
- 灵活：不断繁荣的生态系统，可以在一个库和一套完整框架之间自如伸缩。
- 高效：20kB min+gzip 运行大小，超快虚拟 DOM，最省心的优化

## 安装

### 方式一：CDN引入

- 开发学习环境：

  ~~~HTML
  <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
  ~~~

- 生产环境：

  ~~~html
  <script src="https://cdn.jsdelivr.net/npm/vue@2.6.11"></script>
  ~~~

### 方式二：直接引入js文件

- 开发环境：https://cn.vuejs.org/js/vue.js
- 生产环境：https://cn.vuejs.org/js/vue.min.js

### 方式三：NPM安装

## 基本使用

### HelloWord

~~~html
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>Document</title>
</head>
<body>
	<div id="app">
		<div>{{msg}}</div>
	</div>
    <!--引入外部vue.js-->
	<script type="text/javascript" src="js/vue.js"></script>
	<script type="text/javascript">
		/*
		Vue的基本使用步骤
		1.需要提供标签用于填充数据
		2.引入vue.js标签
		3.可以使用vue的语法做功能了
		4.把vue提供的数据填充到标签里面
		 */
		
		var vm = new Vue({
			el:'#app',
			data:{
				msg:'Hello Vue'
			}
		})
	</script>
</body>
</html>
~~~



el：元素的挂载位置(值可以是CSS选择器或者是DOM元素)

data：模型数据(值是一个对象)

插值表达式：

​		表达式计算

​		填充数据到HTML中

### Vue列表显示

```html
<body>
    <div id="app">
        <ul>
            <li v-for="item in books">{{item}}</li>
        </ul>
    </div>
    <script>
        var vm = new Vue({
            el: '#app',
            data: {
                books: ['计算机组成原理','Java核心技术','Java编程思想','大学英语','大学物理']
            }
        })
    </script>
</body>
```

使用v-for指令遍历数组。

### vue简单计数器

```html
<body>
    <div id="app">
        <h3>当前计数值{{count}}</h3>
        <button @click="addCount">加</button>
        <button @click="delcount">减</button>
    </div>
    <script>
        var vm = new new Vue({
            el: '#app',
            data: {
                count: 0
            },
            methods: {
                addCount() {
                    this.count++
                },
                delcount() {
                    this.count--
                }
            },
        })
    </script>
</body>
```

methods是与el、data同级的一个属性，用于定义方法。

## 模板语法

### 插值表达式

- 文本：数据绑定最常见的形式就是使用 `{{对象}}`（双大括号）的文本插值

  ~~~html
  <div id="app">
    <p>{{ message }}</p>
  </div>
  ~~~

- Html：使用v-html指令用于输出html代码

  ~~~html
  <div id="app">
      <div v-html="message"></div>
  </div>
      
  <script>
  new Vue({
    el: '#app',
    data: {
      message: '<h1>测试文字</h1>'
    }
  })
  </script>
  ~~~

- 属性：HTML属性中的值应使用v-bind指令

  ~~~html
  <div id="app">
    <label for="r1">修改颜色</label><input type="checkbox" v-model="use" id="r1">
    <br><br>
    <div v-bind:class="{'class1': use}">
      v-bind:class 指令
    </div>
  </div>
      
  <script>
  new Vue({
      el: '#app',
    data:{
        use: false
    }
  });
  </script>
  ~~~

- 表达式：支持JavaScript表达式

  ~~~html
  <div id="app">
      {{5+5}}<br>
      {{ ok ? 'YES' : 'NO' }}<br>
      {{ message.split('').reverse().join('') }}
      <div v-bind:id="'list-' + id">测试文字</div>
  </div>
      
  <script>
  new Vue({
    el: '#app',
    data: {
      ok: true,
      message: 'hello world',
      id : 1
    }
  })
  </script>
  ~~~

### 指令

[官方API文档](https://cn.vuejs.org/v2/api/#指令)

指令就是带有v-前缀的特殊属性。

#### v-once

元素与组件只会渲染一次，不再会随着数据变化而变化.

```html
<h2 v-once>{{firstName}} {{lastName}}</h2>
```

<img src="https://lemenk-img-aliyun.oss-cn-beijing.aliyuncs.com/img/2020-06-05-15-17-04.gif" alt="2020-06-05-15-17-04" style="zoom:75%;" />

#### v-text 

用于修改文本内容，会完全覆盖。

~~~html
<span v-text="msg"></span>
~~~

作用与<a href="#3.1 插值表达式">插值表达式</a>一样;

但是`v-text`是没有闪烁问题的；

#### v-cloak

用于解决插值表达式闪烁问题。

~~~html
  <meta charset="UTF-8">
  <title>Document</title>
  <style type="text/css">
  <!--设置v-cloak样式-->
  [v-cloak]{
    display: none;
  }
  </style>
</head>
<body>
  <div id="app">
    <div v-cloak>{{msg}}</div>
  </div>
  <script type="text/javascript" src="js/vue.js"></script>
  <script type="text/javascript">
    var vm = new Vue({
      el: '#app',
      data: {
        msg: 'Hello Vue'
      }
    });
  </script>
</body>
</html>
~~~

**背后的原理**：先通过样式隐藏内容，然后在内存中进行值的替换，替换好之后再显示最终的结果。

#### v-html：代码片段

用于将HTML片段填充到标签中，但是可能有安全问题

~~~html
<div v-html="msg1"></div>

msg1: '<h1>这是一个HTML</h1>'
~~~

#### v-pre

用于显示原始信息，跳过编译过程

~~~html
<div v-pre>{{msg}}</div>

msg: 'Hello Vue'
~~~

#### v-bind：事件单向绑定

- 绑定属性

  ```html
  <body>
      <div id="app">
          <a v-bind:href="baiduLink">百度</a><br>
          <!-- v-bind简写方式 -->
          <a :href="baiduLink">百度</a>
      </div>
      <script type="text/javascript">
          var vm = new Vue({
              el: '#app',
              data: {
                  baiduLink: 'http://www.baidu.com'
              }
          });
      </script>
  </body>
  ```

- 绑定CSS

  ~~~html
  <head>
      <meta charset="UTF-8">
      <meta name="viewport" content="width=device-width, initial-scale=1.0">
      <title>7.v-bind绑定CSS</title>
      <script src="./js/vue.js"></script>
      <style>
          .active {
              color: red;
          }
  
          .size {
              font-size: 35px;
          }
  
          .h2-border {
              border: 1px solid red;
          }
      </style>
  </head>
  
  <body>
      <div id="app">
          <!-- 方式1：直接通过{}绑定一个CSS属性类。可以绑定多个值，也可以与普通类同时存在 -->
          <h2 :class="{'active':isActive}">Hello Vue</h2>
          <!-- 方式2：和普通类同时存在，并不冲突 -->
          <h2 class="h2-border" :class="{'active':isActive, 'size': big}">Hello Vue</h2>
          <!-- 方式3：如果过于复杂，可以放到一个data中 -->
          <h2 class="h2-border" :class="classes">Hello Vue</h2>
  
  
      </div>
      <script type="text/javascript">
          var vm = new Vue({
              el: '#app',
              data: {
                  isActive: true,
                  size: true
              }
          });
      </script>
  </body>
  ~~~

#### v-model：事件双向绑定

Vue中使用v-model来实现表单元素和数据的双向绑定。

~~~html
<body>
    <div id="app">
        <input type="text" name="name" v-model="name">
        <h3>{{name}}</h3>
    </div>
    <script>
        var vm = new Vue({
            el: '#app',
            data: {
                name: '张三'
            }
        })
    </script>
</body>
~~~

当我们输入表单中的数据时，data中也会随之改变，data被改变后，插值运算符中的显示内容也会随之改变。

- v-model绑定select标签

  ~~~html
  <body>
      <div id="app">
          <select name="" v-model="mySelect">
              <option value="java">Java从入门到精通</option>
              <option value="mysql">MySQL从入门到精通</option>
              <option value="python">Python从入门到精通</option>
          </select>
      </div>
      <script>
          var vm = new Vue({
              el: '#app',
              data: {
                  mySelect: 'java'
              }
          })
      </script>
  </body>
  ~~~

#### 条件判断：v-if

包括v-if、v-else-if、v-else。

这三个指令与JS中的条件判断if、else if、else类似。

v-if可以根据表达式中的值渲染或者销毁元素和组件。

~~~html
<body>
    <div id="app">
        {{score}}
        <div v-if="score >= 85">优秀</div>
        <div v-else-if="score >= 70">良好</div>
        <div v-else-if="score >= 60">及格</div>
        <div v-else>不及格</div>
        <button @click="delScore">减分</button>
    </div>
</body>

<script>
    var vm = new Vue({
        el: '#app',
        data: {
            score: 100
        },
        methods: {
            delScore() {
                this.score -= 10
            }
        }

    })
</script>
~~~

当前页面将只会显示一个符合条件的div。

<img src="https://lemenk-img-aliyun.oss-cn-beijing.aliyuncs.com/img/image-20200606162444758.png" alt="image-20200606162444758" style="zoom:80%;" />

#### 条件判断：v-show

~~~html
<body>
    <div id="app">
        <p>{{score}}</p>
        <div v-show="score >= 90">优秀</div>
        <div v-show="score >= 75 && score < 90">良好</div>
        <div v-show="score >= 60 && score < 75">及格</div>
        <div v-show="score < 60">不及格</div>
        <button @click="delScore">减分</button>
    </div>
</body>

<script>
    var vm = new Vue({
        el: '#app',
        data: {
            score: 100
        },
        methods: {
            delScore() {
                this.score -= 10
            }
        }

    })
</script>
~~~

页面展示：

<img src="https://lemenk-img-aliyun.oss-cn-beijing.aliyuncs.com/img/image-20200606162238379.png" alt="image-20200606162238379" style="zoom:80%;" />

**条件判断小结：**

v-if是完全不创建其余DOM元素，而v-show则是创建了所有DOM元素，仅仅是使用`display:none`隐藏了该元素。

当需要频繁的显示、隐藏一些内容时，使用v-show。当我们仅有一次切换，某些v-if场景根本不会显示出来的时候，用v-if。

#### 循环遍历：v-for

- 遍历数组

  ~~~html
  <body>
      <div id="app">
          <ul>
              <li v-for="item in books">{{item}}</li>
              <hr>
              <!-- 获取索引值 -->
              <li v-for="(item, index) in books">{{index + ':' + item}}</li>
          </ul>
      </div>
  </body>
  
  <script>
      var vm = new Vue({
          el: '#app',
          data: {
              books: ['计算机组成原理',
                  'Java核心技术',
                  'Java编程思想',
                  '大学英语',
                  '大学物理'
              ]
          }
      })
  </script>
  ~~~

  <img src="https://lemenk-img-aliyun.oss-cn-beijing.aliyuncs.com/img/image-20200606163154136.png" alt="image-20200606163154136" style="zoom:80%;" />

#### 事件监听：v-on

~~~html
<body>
    <div id="app">
        <div>{{num}}</div>
        <div>
            <button v-on:click="num++">点击</button>
            <!-- v-on缩写为@ -->
            <button @click="num++">点击1</button>
            <br>
            <!-- 标准写法 -->
            <button @click="handle">点击2</button>
        </div>
    </div>
</body>
<script>
    var vm = new Vue({
        el: '#app',
        data: {
            num: 0
        },
        methods: {
            handle() {
                // 这里的this是Vue的实例对象+
                console.log(this === vm)
                //   在函数中 想要使用data里面的数据 一定要加this 
                this.num++;
            }

        }

    });
</script>
~~~



### 计算属性及其getter和setter方法

计算属性是一个属性，而不是方法。虽然写法是方法，但是我们使用的时候是直接视为一个属性去操作的，使用方式和data一致。

计算属性中使用到的data中的数据只要发生了变化，计算属性就会重新计算。如果两次获取计算属性的时候，里面使用到的属性没有发生变化，计算属性会直接使用之前缓存的值。

~~~html
<body>
    <div id="app">
        <input type="text" v-model="firstName">
        <input type="text" v-model="lastName">
        <h2>{{fullName}}</h2>
    </div>
</body>
<script>
    let app = new Vue({
        el: '#app',
        data: {
            firstName: '张',
            lastName: '狗子'
        },
        computed: {
            fullName() {
                // 写法是个方法，但是使用的时候是作为属性使用的，和data一致。
                return this.firstName + this.lastName
            }
        }
    })
</script>
~~~

每一个计算属性都包含get方法和set方法。

~~~html
<body>
  <div id="app">
    <input type="text" v-model="firstName">
    <input type="text" v-model="lastName">
    <input type="text" v-model="fullName">
  </div>
</body>
<script>
  let app = new Vue({
    el: '#app',
    data: {
      firstName: '张',
      lastName: '狗子'
    },
    computed: {
      fullName: {
        get() {
          console.log('调用了get方法')
          return this.firstName+' '+this.lastName
        },
        set(val) {
          console.log('调用了set方法')
          const names = val.split(' ')
          this.firstName = names[0]
          this.lastName = names[1]
        }
      }
    }
  })
</script>
~~~

### 参数

参数在指令后以冒号指明。

~~~html
<div id="app">
    <pre><a v-bind:href="url">菜鸟教程</a></pre>
</div>
    
<script>
new Vue({
  el: '#app',
  data: {
    url: 'http://www.runoob.com'
  }
})
</script>
~~~

v-bind 指令被用来响应地更新 HTML 属性。href 是参数，告知 v-bind 指令将该元素的 href 属性与表达式 url 的值绑定。

~~~html
<a v-on:click="doSomething">
~~~

**修饰符**：是以半角句号 **.** 指明的特殊后缀，用于指出一个指令应该以特殊方式绑定。

~~~html
<form v-on:submit.prevent="onSubmit"></form>
~~~

`.prevent` 修饰符告诉`v-on`指令对于触发的事件调用 `event.preventDefault()`：

### 用户输入

~~~html
<!--双向数据绑定-->
<div id="app">
    <p>{{ message }}</p>
    <input v-model="message">
</div>
    
<script>
new Vue({
  el: '#app',
  data: {
    message: 'Runoob!'
  }
})
</script>
~~~

在 input 输入框中我们可以使用 v-model 指令来实现双向数据绑定。

`v-model` 指令用来在 `input`、`select`、`textarea`、`checkbox`、`radio` 等表单控件元素上创建双向数据绑定，根据表单上的值，自动更新绑定的元素的值。

~~~html
<!--字符串反转-->
<div id="app">
    <p>{{ message }}</p>
    <button v-on:click="reverseMessage">反转字符串</button>
</div>
    
<script>
new Vue({
  el: '#app',
  data: {
    message: 'Runoob!'
  },
  methods: {
    reverseMessage: function () {
      this.message = this.message.split('').reverse().join('')
    }
  }
})
</script>
~~~

按钮的事件我们可以使用 v-on 监听事件，并对用户的输入进行响应。

### 监听器

如果我们需要监听某个值的变化，当这个值变化是，进行一些操作，可以使用监听器。

监听器的属性名称是**watch**，和data平级。

监听器中的数据是方法，接收两个参数，分别为改变后的值和改变前的值。

~~~html
<body>
    <div id="app">
        <input type="text" v-model="firstName">
        <input type="text" v-model="lastName">
        <h2>{{fullName}}</h2>
    </div>
</body>
<script>
    let app = new Vue({
        el: '#app',
        data: {
            firstName: '张',
            lastName: '华',
            fullName: ''
        },
        watch: {
            firstName: function (newVal, oldVal) {
                this.fullName = this.firstName + this.lastName
            },
            lastName(newVal, oldVal) {
                this.fullName = this.firstName + this.lastName
            }
        }
    })
</script>
~~~

### 过滤器

Vue.js 允许自定义过滤器，被用作一些常见的文本格式化。

```html
<!-- 在两个大括号中 -->
{{ message | capitalize }}

<!-- 在 v-bind 指令中 -->
<div v-bind:id="rawId | formatId"></div>
```

过滤器函数接受表达式的值作为第一个参数。

~~~html
<!--实例-->
<div id="app">
  {{ message | capitalize }}
</div>
    
<script>
new Vue({
  el: '#app',
  data: {
    message: 'runoob'
  },
  filters: {
    capitalize: function (value) {
      if (!value) return ''
      value = value.toString()
      return value.charAt(0).toUpperCase() + value.slice(1)
    }
  }
})
</script>
~~~

过滤器可以串联：

```html
{{ message | filterA | filterB }}
```

过滤器是 JavaScript 函数，因此可以接受参数：

```html
{{ message | filterA('arg1', arg2) }}
```

这里，message 是第一个参数，字符串 'arg1' 将传给过滤器作为第二个参数， arg2 表达式的值将被求值然后传给过滤器作为第三个参数。

# 组件化开发

## Vue的组件化思想

组件化是Vue的一个重要思想

它提供了一种抽象，让我们可以开发出一个个独立可复用的小组件来构建我们的应用。

任何的应用都可以被抽象成一颗组件树。

<img src="https://lemenk-img-aliyun.oss-cn-beijing.aliyuncs.com/img/components.png" alt="components" style="zoom:80%;" />

## 组件的使用

- 创建组件构造器
- 注册组件
- 使用组件

```html
<body>
    <div id="app">
        <my-component1></my-component1>
        <my-component2></my-component2>
        <my-component3></my-component3>
        <my-component4></my-component4>
    </div>
</body>
<template id="temp">
    <div>
        <p>我是p标签</p>
        <h1>我是h1标签</h1>
        <div style="color: red;">哈哈哈</div>
    </div>
</template>

<script>
    var num = 10
    /* 
    1.使用Vue.extend来注册组件
    变量名采用小驼峰规则。
    但是使用组件时全部改成小写，中间用-连接
    */
    Vue.component('myComponent1', Vue.extend({
        // template就是组件要展示的内容，可以是html标签
        template: '<h3>这是用extend注册的组件</h3>'
    }))

    /**
     * 2.不使用extend去注册组件
     */
    Vue.component('myComponent2', {
        // template就是组件要展示的内容，可以是html标签
        template: '<div><h3>这是不用extend注册的组件</h3><h3>我是第二个h3</h3></div>'
    })
    // 不论使用哪一种注册方式，template属性只能有一个，并且有且仅有一个根节点。
    Vue.component('myComponent3', {
        // template就是组件要展示的内容，可以是html标签
        template: `<div><h3>组件中使用外部变量num:${num}</h3></div>`
    })

    // 3.使用template
    Vue.component('myComponent4', {
        template: '#temp'
    })

    var vm = new Vue({
        el: '#app',
        data: {

        }
    })
</script>
```

## 私有组件

```html
<body>
    <div id="app1">
        <my-component></my-component>
        <my-comp></my-comp>
    </div>
    <div id="app2">
        <my-component></my-component>
    </div>
</body>

<script>
    /* 全局组件 */
    Vue.component('my-component', Vue.extend({
        // template就是组件要展示的内容，可以是html标签
        template: '<h3>这是用extend注册的组件</h3>'
    }))

    /* 私有组件 */
    var comp = Vue.extend({
        // template就是组件要展示的内容，可以是html标签
        template: '<h3>这是私有组件</h3>'
    })

    var vm = new Vue({
        el: '#app1',
        components:{
            'my-comp': comp
        }
    })
    var vm = new Vue({
        el: '#app2'
    })
</script>
```

`components`：关键字，与`el`、`data`等关键字同级，用于注册组件。

**一般开发中都使用私有组件。**

## 父组件与子组件

在组件树，组件和组件之间存在层级关系的，这就是父组件与子组件。

组件中也有`components`关键字，同样是使用`components`将子组件注册到父组件。

~~~html
<body>
    <div id="app">
        <parent-com></parent-com>
    </div>
</body>

<script>
    // 1.创建一个子组件
    let childCom = Vue.extend({
        template: `
      <div>我是子组件内容。</div>
    `
    })
    // 2.创建一个父组件，并注册子组件
    let parentCom = Vue.extend({
        template: `
      <div>
        <h1>我是父组件内容</h1>
        <child-com></child-com>
      </div>
    `,
        components: {
            childCom
        }
    })
    let app = new Vue({
        el: '#app',
        components: {
            parentCom
        }
    })
</script>
~~~

## 组件的数据

组件也有个`data`、`methods`、`filters`等属性。其中除`data`属性之外，其他属性的使用方式与Vue一致。

在组件中，`data`属性必须使用方法的形式定义，且返回一个对象。

~~~html
<body>
    <div id="app">
        <my-com></my-com>
    </div>
</body>

<script>
    let myCom = Vue.extend({
        template: `<div>我是组件{{msg}}</div></div>`,
        data() {
            return {
                msg: '  我是子组件的msg  '
            }
        }
    })
    let app = new Vue({
        el: '#app',
        data: {
            msg: '哈哈哈'
        },
        components: {
            myCom
        }
    })
</script>
~~~

可以看到在组件中，`data`是以方法的形式书写的。而且不能取到Vue的data属性的值。

## 组件中的通信

### 父组件向子组件传值

~~~html
<body>
    <div id="app">
        <!-- 第一步，用绑定的方式，将父组件的数据绑定给子组件 -->
        <my-com :msg="msg"></my-com>
    </div>
</body>

<template id="myTemp">
    <div>
        <span>当前数量：{{count}}</span>
        <div>{{msg}}</div>
    </div>
</template>

<script>
    let myCom = Vue.extend({
        template: '#myTemp',
        data() {
            return {
                count: 10
            }
        },
        props: [
            // 第二步，使用props接收.
            'msg'
        ]
    })
    let app = new Vue({
        el: '#app',
        data: {
            msg: '我是父组件的msg'
        },
        components: {
            myCom
        }
    })
</script>
~~~

### 父组件向子组件传递方法

~~~html
<body>
    <div id="app">
        <!-- 第一步，在子组件上，使用@符号为该组件绑定一个事件 -->
        <my-com @alert-msg="alertMsg"></my-com>
    </div>
</body>
<template id="myTemp">
    <div>
        <button @click="handlerMethod">点我</button>
    </div>
</template>
<script>
    let myCom = Vue.extend({
        template: '#myTemp',
        methods: {
            handlerMethod() {
                // 第一个参数表示要调用的方法。
                // 第二个参数往后，就是我们需要传递的参数
                this.$emit('alert-msg', '被调用')
            }
        }
    })
    let app = new Vue({
        el: '#app',
        data: {
            msg: '我是父组件的msg'
        },
        methods: {
            alertMsg(msg) {
                alert(msg)
            }
        },
        components: {
            myCom
        }
    })
</script>
~~~



### 父组件调用自组件方法

~~~html
<body>
    <div id="app">
        <button @click="countAdd">点我</button>
        <!-- 第一步，给子组件绑定一个ref -->
        <my-com ref="myComp"></my-com>
    </div>
</body>
<template id="myTemp">
    <div>
        {{count}}
    </div>
</template>
<script>
    let myCom = Vue.extend({
        template: '#myTemp',
        data() {
            return {
                count: 1
            }
        },
        methods: {
            addCount() {
                this.count++
            }
        }
    })
    let app = new Vue({
        el: '#app',
        methods: {
            countAdd() {
                // 第二步，在父组件中使用this.$refs.id就行了
                console.log(this.$refs.myComp.count)
                this.$refs.myComp.addCount()
            }
        },
        components: {
            myCom
        }
    })
</script>
~~~

# 模块化开发

# Webpack

## Webpack基础

官网：webpack是一个现代的JavaScript应用的**静态模块打包工具**。

### 安装

- 安装最新版`node.js`：https://nodejs.org/zh-cn/download/

- 查看当前node版本：`node -v`

  ![](https://lemenk-img-aliyun.oss-cn-beijing.aliyuncs.com/img/image-20200608171351949.png)

- **全局安装webpack**：（但官网不推荐）

  ~~~shell
  npm install --global webpack@3.6.0
  ~~~

  若安装webpack4+版本，必须安装webpack-cli。

  ~~~shell
  cnpm install --global webpack-cli
  ~~~

- **局部安装webpack**：

  ~~~shell
  //先cd进入对应的开发目录，再执行以下命令
  cnpm install --save-dev webpack@3.6.0
  ~~~

- 为什么要安装局部webpack？

  因为全局安装版本可能与当前项目webpack版本不一致，所以为了统一开发环境，必须使用局部安装所需版本的webpack。

### 基础案例

- 项目目录：

  <img src="https://lemenk-img-aliyun.oss-cn-beijing.aliyuncs.com/img/image-20200608170450316.png" alt="image-20200608170450316" style="zoom:80%;" />

  - dist文件夹：用于存放之后打包的文件。

    - bundle.js：是webpack处理了项目直接文件依赖后生成的一个js文件，index.html中引入此文件。

  - src文件夹：用于存放我们写的源文件。

    - main.js：项目的入口文件。

      ~~~js
      // 1.使用commonjs的模块化规范
      const {
          add,
          mul
      } = require('./mathUtils.js')
      
      console.log(add(20, 30));
      console.log(mul(20, 30));
      
      // 2.使用ES6的模块化的规范
      import {
          name,
          age,
          height
      } from "./info";
      
      console.log(name);
      console.log(age);
      console.log(height);
      ~~~

    - mathUtils.js：定义了一些数学工具函数，可以在其他地方引用，并且使用。

      ~~~js
      // 两数相加
      function add(num1, num2) {
        return num1 + num2
      }
      
      // 两数相乘
      function mul(num1, num2) {
        return num1 * num2
      }
      
      //导出两个函数
      module.exports = {
        add,
        mul
      }
      ~~~

    - info：定义了一些用户信息。

      ~~~html
      // 两数相加
      function add(num1, num2) {
        return num1 + num2
      }
      
      // 两数相乘
      function mul(num1, num2) {
        return num1 * num2
      }
      
      //导出两个函数
      module.exports = {
        add,
        mul
      }
      ~~~

  - index.html：浏览器打开展示的首页html。

    ~~~html
    <!DOCTYPE html>
    <html lang="en">
    
    <head>
        <meta charset="UTF-8">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <title>首页</title>
    </head>
    
    <body>
        <script src="./dist/bundle.js"></script>
    </body>
    
    </html>
    ~~~

- 打包命令：

  ~~~shell
  //在对应的项目目录中
  webpack ./src/main.js ./dist/bundle.js
  ~~~

  <img src="https://lemenk-img-aliyun.oss-cn-beijing.aliyuncs.com/img/image-20200608172451080.png" alt="image-20200608172451080" style="zoom:80%;" />

- 运行结果：

  <img src="https://lemenk-img-aliyun.oss-cn-beijing.aliyuncs.com/img/image-20200609131819638.png" alt="image-20200609131819638" style="zoom:80%;" />

### 使用webpack配置打包环境

为什么要配置webpack的打包环境？

因为每次进行打包时都需要写入口文件和打包文件路径，很麻烦，所以配置webpack打包环境可以简化打包操作。

- 编写基础案例的代码

- 执行以下命令，初始化webpack环境

  ~~~shell
  cnpm init
  ~~~

  <img src="https://lemenk-img-aliyun.oss-cn-beijing.aliyuncs.com/img/image-20200609132536498.png" alt="image-20200609132536498" style="zoom:80%;" />

  可得到package.json文件：

  <img src="https://lemenk-img-aliyun.oss-cn-beijing.aliyuncs.com/img/image-20200609132940116.png" alt="image-20200609132940116" style="zoom:80%;" />

- 在项目根目录下新建`webpack.config.js`文件，用来进行写webpack配置内容。

  ~~~js
  // 导入node的path模块
  const path = require('path')
  
  module.exports = {
      entry: './src/main.js',
      //输出路径应该分开写
      output: {
          path: path.resolve(__dirname, 'dist'),
          filename: 'bundle.js'
      },
  }
  ~~~

- 再直接使用`webpack`命令进行打包。

  <img src="https://lemenk-img-aliyun.oss-cn-beijing.aliyuncs.com/img/image-20200609133305921.png" alt="image-20200609133305921" style="zoom:80%;" />

- 局部安装webpack

  ~~~shell
  cnpm install --save-dev webpack@3.6.0
  ~~~

  <img src="https://lemenk-img-aliyun.oss-cn-beijing.aliyuncs.com/img/image-20200609194906744.png" alt="image-20200609194906744" style="zoom:80%;" />

  生成`node_modules`文件夹，修改了`package.json`文件夹。

  <img src="https://lemenk-img-aliyun.oss-cn-beijing.aliyuncs.com/img/image-20200609195100527.png" alt="image-20200609195100527" style="zoom:80%;" />

### loader

#### 什么是loader？

在之前都是使用`webpack`来打包代码的，并且会自动处理`js`文件依赖问题。

但是在开发中还有另外一些格式的文件，例如`CSS`、图片，也包括一些高级的将`ES6`转成`ES5`代码，将`TypeScript`转成`ES5`代码，将`scss`、`less`转成`css`，将`.jsx`、`.vue`文件转成`js`文件等等。

对于`webpack`来说并没有打包这些文件的功能，引体`webpack`提供了`loader`拓展功能。

#### 如何使用loader？

1. 使用npm安装所需要使用的loader
2. 在webpack.config.js中的modules关键字下进行配置

官网详细教程：https://www.webpackjs.com/loaders/

#### loader使用案例

##### 处理CSS文件

- 目录结构：

  ![image-20200609225748077](https://lemenk-img-aliyun.oss-cn-beijing.aliyuncs.com/img/image-20200609225748077.png)

- loader配置：

  ~~~shell
  cnpm install --save-dev css-loader
  ~~~

  ![image-20200609230021210](https://lemenk-img-aliyun.oss-cn-beijing.aliyuncs.com/img/image-20200609230021210.png)

  ~~~js
  // 导入node的path模块
  const path = require('path')
  
  module.exports = {
    entry: './src/main.js',
    //输出路径应该分开写
    output: {
      path: path.resolve(__dirname, 'dist'),
      filename: 'bundle.js'
    },
    //loader配置
    module: {
      rules: [
      {
        test: /\.css$/,
        use: ["style-loader", 'css-loader']
      }
      ]
    }
  }
  ~~~

##### 处理图片文件

若要在项目中使用图片文件，则需要添加一些关于文件的配置。

由于文件一般是通过`url`方式引入的，则需要使用`url-loader`。

- loader配置

  ~~~shell
  cnpm install --save-dev url-loader
  ~~~

  ~~~js
  rules: [
      {
          test: /\.(png|jpg|gif|jpeg)$/,
          use: [{
            loader: 'url-loader',
            options: {
              limit: 300000
            }
          }]
        }
      ]
  ~~~

  `options`字段中，`limit: 300000`是配置限制文件的大小。

  若是文件小于该大小，则使用`url-loader`；若是大于该文件，则使用`file-loader`。

- 当文件大于该限制时，会报错：

  <img src="https://lemenk-img-aliyun.oss-cn-beijing.aliyuncs.com/img/image-20200618201549275.png" alt="image-20200618201549275" style="zoom:80%;" />

  需要下载`file-loader`。

  ~~~shell
  cnpm install --save-dev file-loader
  ~~~

  同时需要修改`webpack.config.js`文件夹，配置公共路径。

  ~~~js
  output: {
      path: path.resolve(__dirname, 'dist'),
      filename: 'bundle.js',
      publicPath: 'dist/'
    },
  ~~~

  `publicPath: 'dist/'`：是指所有关于URL路径的都会自动加上`dist/`。

  这是因为，大于`url-loader`限制的文件，将通过文件的方式打包至`dist`路径下。

- 通常开发中，`limit`常常设置为`8kb`。

- 处理文件路径和重命名

  通常在开发中，可能不同文件夹中的图片可能同名，为了防止在打包时覆盖，所以需要对文件进行重命名。配置方式如下：

  ~~~js
  options: {
      limit: 300000,
      name: 'img/[name].[hash:8].[ext]'
  }
  ~~~

  `[name]`：是指原文件名称。

  `[hash:8]`：是为了防止重复添加8位的`hash`值。

  `[ext]`：是指原文件的格式。

  ![image-20200618205718661](https://lemenk-img-aliyun.oss-cn-beijing.aliyuncs.com/img/image-20200618205718661.png)

### babel

**ES6语法处理**：在之前的`webpack`打包的`js`文件，写的`ES6`语法并没有转成`ES5`，那么就意味着可能一些对`ES6`还不支持的浏览器没有办法很好的运行我们的代码。

因此就需要将`ES6`语法转化成`ES5`。使用[babel](https://www.webpackjs.com/loaders/babel-loader/)插件。

~~~shell
cnpm install --save-dev babel-loader@7 babel-core babel-preset-es2015
~~~

~~~js
module: {
  rules: [
    {
      test: /\.js$/,
      exclude: /(node_modules|bower_components)/,
      use: {
        loader: 'babel-loader',
        options: {
          presets: ['es2015']
        }
      }
    }
  ]
}
~~~

再重新打包，可以看到`bundle.js`中变成了`ES5`的语法。

<img src="https://lemenk-img-aliyun.oss-cn-beijing.aliyuncs.com/img/image-20200621145632933.png" alt="image-20200621145632933" style="zoom:80%;" />



## Webpack配置Vue

### 简单配置使用

1. npm引入vue

   ~~~shell
   cnpm install vue --save
   ~~~

2. 在main.js中引入Vue

   ~~~js
   import Vue from 'vue'
   
   const app = new Vue({
       el: '#app',
       data: {
           msg: 'Hello Webpack'
       }
   });
   ~~~

3. 在index.html中打印该msg

   ~~~html
   <body>
       <div id="app">
           <h2>{{msg}}</h2>
       </div>
       
       <script src="./dist/bundle.js"></script>
   </body>
   ~~~

4. 运行该项目，但是浏览器中并未打印改内容，而且console中还会报错。

   ![image-20200621154622347](https://lemenk-img-aliyun.oss-cn-beijing.aliyuncs.com/img/image-20200621154622347.png)

   这是因为vue默认使用的是runtime-only，但是该版本中不包含template，因此无法构建。[官网说明](https://cn.vuejs.org/v2/guide/installation.html#%E5%AF%B9%E4%B8%8D%E5%90%8C%E6%9E%84%E5%BB%BA%E7%89%88%E6%9C%AC%E7%9A%84%E8%A7%A3%E9%87%8A)

   **解决方法：**在webpack.config.js文件中的module下添加配置。

   ~~~js
   resolve: {
       // alias: 别名
       extensions: ['.js', '.css', '.vue'],
       alias: {
         'vue$': 'vue/dist/vue.esm.js'
       }
   }
   ~~~

5. 再运行该程序，则正常运行。

   ![image-20200621155359600](https://lemenk-img-aliyun.oss-cn-beijing.aliyuncs.com/img/image-20200621155359600.png)

### Vue组件化开发引入

在通常的开发中，一般不会在`index.html`中书写过多的代码，而是通过`vue`文件进行引入。

- 安装vue-loader等依赖。

  ~~~shell
  cnpm install vue-loader vue-template-compiler --save-dev
  ~~~

  配置vue-loader：

  ~~~js
  {
      test: /\.vue$/,
      use: ['vue-loader']
  }
  ~~~

- 创建`vue`文件夹，并创建`vue`入口文件`App.vue`文件。

  ~~~vue
  <template>
    <div>
      <h2 class="title">{{message}}</h2>
      <button @click="btnClick">按钮</button>
      <h2>{{name}}</h2>
    </div>
  </template>
  
  <script>
  
    export default {
      name: "App",
      data() {
        return {
          message: 'Hello Webpack',
          name: 'coderwhy'
        }
      },
      methods: {
        btnClick() {
  
        }
      }
    }
  </script>
  
  <style scoped>
    .title {
      color: green;
    }
  </style>
  ~~~

- 在main.js中引入该vue文件

  ~~~js
  import Vue from 'vue'
  import App from './vue/App'
  
  const app = new Vue({
      el: '#app',
      template: '<App/>',
      components: {
          App
      }
  });
  ~~~

- 修改index.html文件

  ~~~html
  <body>
      <div id="app">
      </div>
      <script src="./dist/bundle.js"></script>
  </body>
  ~~~



## Webpack的Plugin配置

### 添加版权声明

在`webpack.config.js`文件中添加语句：

~~~js
//导入模块
const webpack = require('webpack')

//。。。其他配置

//配置插件
  plugins: [
    //添加版权声明
    new webpack.BannerPlugin('最终版权归Lemenk所有')
  ]
~~~

注：`plugin`属性为二级属性，与`module`同级。

### 打包HTML

为了使`index.html`在发布时打包至`dist`文件夹，所以需要使用该插件。

1. 安装

   ~~~shell
   cnpm install html-webpack-plugin --save-dev
   ~~~

2. 在`webpack.config.js`中进行配置

   ~~~js
   //导入模块
   const HtmlWebpackPlugin = require('html-webpack-plugin')
   
   //其他配置
   
   //配置插件
   plugins: [
     new HtmlWebpackPlugin({
       template: 'index.html'
     })
   ]
   ~~~

   注：请选择插件版本为`3.2.0`。

### JS压缩

该插件可以压缩bundle.js，以减少占用空间。

1. 安装

   ~~~shell
   cnpm install uglifyjs-webpack-plugin@1.1.1 --save-dev
   ~~~

2. 配置

   ~~~js
   //导入模块
   const UglifyjWebpackPlugin = require('uglifyjs-webpack-plugin')
   
   //其他配置
   
   //配置插件
     plugins: [
       new UglifyjWebpackPlugin()
     ]
   ~~~

3. 结果展示

   ![image-20200621184912360](https://lemenk-img-aliyun.oss-cn-beijing.aliyuncs.com/img/image-20200621184912360.png)

   

# Vue CLI

## 基础知识

1. CLI是什么意思？

   - CLI是Command-Line Interface, 翻译为命令行界面, 但是俗称脚手架.
   - Vue CLI是一个官方发布 vue.js 项目脚手架
   - 使用 vue-cli 可以快速搭建Vue开发环境以及对应的webpack配置.

2. 为什么要用CLI？

   在使用Vue.js开发大型应用时，我们需要考虑代码目录结构、项目结构和部署、热加载、代码单元测试等事情。如果每个项目都需要手动完成这些工作，则会影响工作效率。

3. 使用前提：

   Vue CLI使用了webpack模板，所以需要先安装webpack。
   
4. runtime+compiler与runtime-only

   在使用Vue-CLI初始化项目时，会有一个选择是使用runtime+compiler和runtime-only的选项。通过创建项目可以发现两者的区别。

   ![image-20200623144530085](https://lemenk-img-aliyun.oss-cn-beijing.aliyuncs.com/img/image-20200623144530085.png)

   具体可以看另外一篇博文：传送门。

## Vue CLI的使用

### 安装

安装Vue脚手架

~~~shell
npm install -g @vue/cli
~~~

可以使用`vue -V`命令查看当前vue版本（注意V为大写）

![image-20200623170512438](https://lemenk-img-aliyun.oss-cn-beijing.aliyuncs.com/img/image-20200623170512438.png)

当前最新版本为`4.4.5`

拉取CLI2版本的模块t

~~~sh
npm install -g @vue/cli-init
~~~

### 创建Vue CLI2初始项目

~~~sh
vue init webpack my-project
~~~

![image-20200621220740856](https://lemenk-img-aliyun.oss-cn-beijing.aliyuncs.com/img/image-20200621220740856.png)



项目结构：

<img src="https://lemenk-img-aliyun.oss-cn-beijing.aliyuncs.com/img/FastStoneEditor.png" alt="FastStoneEditor" style="zoom:80%;" />



### 创建Vue CLI4初始项目

#### 步骤

~~~sh
vue create project-name
~~~

**第一步**：选择配置

<img src="https://images.lemenk.top/img/image-20200623174235896.png" alt="image-20200623174235896" style="zoom:80%;" />

**第二步**：选择项目使用的组件。**使用空格键选择或反选**

![image-20200623174653869](https://images.lemenk.top/img/image-20200623174653869.png)

**第三步**：选择将配置存放的位置

![image-20200623175540854](https://images.lemenk.top/img/image-20200623175540854.png)

一般性选择第一项，即存放到独立的文件中。

**第四步**：选择是否保存刚才的配置，若选择yes，则需要填写文件名称。这里我设置为`myself-config`。

![image-20200623180008702](https://images.lemenk.top/img/image-20200623180008702.png)

保存后，在下一次创建项目时就会出现该配置。

![image-20200623180415469](https://images.lemenk.top/img/image-20200623180415469.png)

若要删除该配置，则可以进入`C:\Users\username\.vuerc`文件中删除`presets`中的配置。

第五步，在经过一系列自动配置后完成项目的创建。

![image-20200623181045707](https://images.lemenk.top/img/image-20200623181045707.png)

#### 目录结构

![image-20200623182058236](C:%5CUsers%5CLemenk%5CAppData%5CRoaming%5CTypora%5Ctypora-user-images%5Cimage-20200623182058236.png)

#### 配置文件

##### 配置文件去哪里了

很明显可以看到该项目文件夹下没有了cli2中的build和config文件夹。这是因为在lic3中将配置文件隐藏在`node_modules\@vue\cli-service`中。

![image-20200624104215943](C:%5CUsers%5CLemenk%5CAppData%5CRoaming%5CTypora%5Ctypora-user-images%5Cimage-20200624104215943.png)

##### 如何修改配置文件

1. 使用`vue ui`：

   在任意一目录下启动cmd，使用以下命令：

   ~~~sh
   vue ui
   ~~~

   将会启动vue服务器，打开一个本地服务

   <img src="C:%5CUsers%5CLemenk%5CAppData%5CRoaming%5CTypora%5Ctypora-user-images%5Cimage-20200624104539898.png" alt="image-20200624104539898" style="zoom:80%;" />

   在导入下选择项目所在的文件夹，将会进入该项目的配置页面。可以根据需求对一些配置进行修改。

2. 在项目下创建`vue.config.js`文件，并导出配置

   ~~~js
   module.exports = {
   }
   ~~~



# Vue Router

## 网络发展

### 后端路由阶段

后端开发人员完成前后端代码编写，在页面代码中嵌入后端逻辑代码，然后再生成前端代码相应给浏览器。

每个页面都有自己的URL，URL会发送到服务器, 服务器会对该URL进行匹配, 并且最后交给一个Controller进行处理。Controller进行各种处理, 最终生成HTML或者数据, 返回给前端。

常见的有JSP等。

### 前后端分离阶段

Ajax的出现, 有了前后端分离的开发模式。后端只提供API来返回数据, 前端通过Ajax获取数据, 并且可以通过JavaScript将数据渲染到页面中。

这样也有利于移动端程序的开发，可共用一套API即可。

也是目前最多的选择。

### SPA阶段

SPA是指单页面富应用。

静态资源服务器中只有一个html，只改变url，但是页面并不进行整体刷新。



## VueRouter基础使用

### 简单配置

1. 可以在创建项目时选择VueRouter，会自动添加router的配置。
2. 手动创建
   - 安装vue-router
   
     ~~~sh
     npm install vue-router --save
     ~~~
   
   - 在`src`目录下创建`router`文件，并在`router`文件夹中创建`index.js`
   
     ![image-20200625215345197](https://images.lemenk.top/img/image-20200625215345197.png)
   
   - 在`src/router/index.js`文件中编写`router`配置
   
     ~~~js
     import Vue from 'vue'
     import Router from 'vue-router'
     import HelloWorld from '@/components/HelloWorld'
     
     //1.安装router插件
     Vue.use(Router)
     
     //2.创建VueRouter对象，并导出
     export default new Router({
       //配置路由和组件之间的映射关系
       routes: [
         {
           path: '/',
           name: 'HelloWorld',
           component: HelloWorld
         }
       ]
     })
     ~~~
   
   - 将router挂载到vue实例中
   
     ~~~js
     import Vue from 'vue'
     import App from './App'
     import router from './router'
     
     Vue.config.productionTip = false
     
     /* eslint-disable no-new */
     new Vue({
       el: '#app',
       //挂载router
       router,
       render: h => h(App)
     })
     ~~~
   
     

### 使用步骤

1. 创建组件：在`vue`项目中，一般一个`component`组件就是一个页面。

   在`src/components/`目录下创建两个vue文件，`Home.vue`和`About.vue`。

   **Home.vue**：

   ~~~vue
   <template>
     <div>
       <h2>首页</h2>
       <p>我是首页，我是首页</p>
     </div>
   </template>
   
   <script>
   export default {
     name: "Home"
   };
   </script>
   
   <style scoped>
   </style>
   ~~~

   **About.vue**：

   ~~~vue
   <template>
     <div>
       <h2>关于我</h2>
       <p>关于我，关于我</p>
     </div>
   </template>
   
   <script>
   export default {
     name: "About"
   };
   </script>
   
   <style scoped>
   </style>
   
   ~~~

2. 在App.vue中注册两个组件：

   ~~~vue
   <template>
     <div id="app">
       <router-link to="/home">首页</router-link>
       <router-link to="/about">关于</router-link>
       <router-view></router-view>
     </div>
   </template>
   
   <script>
   export default {
     name: "App"
   };
   </script>
   
   <style>
   </style>
   
   ~~~

   

3. 在`src/router/index.js`文件中配置路由

   ~~~js
   import Vue from 'vue'
   import Router from 'vue-router'
   import Home from '../components/Home'
   import About from '../components/About'
   
   //1.安装router插件
   Vue.use(Router)
   
   //2.创建VueRouter对象，并导出
   export default new Router({
     //配置路由和组件之间的映射关系
     routes: [{
         path: '/',
         redirect: '/home'
       },
       {
         path: '/home',
         component: Home
       },
       {
         path: '/about',
         component: About
       }
     ],
     mode: 'history'
   })
   
   ~~~

4. 运行项目，既可以看到结果：

   ![Video_2020-06-26_004809](http://images.lemenk.top/img/Video_2020-06-26_004809.gif)

### 详细配置

#### mode

在`src/router/index.js`文件的`routes`配置中，有一项mode配置：

![image-20200626010934971](C:%5CUsers%5CLemenk%5CAppData%5CRoaming%5CTypora%5Ctypora-user-images%5Cimage-20200626010934971.png)

该选项是选择选择路由的模式。具体可以看里往外一篇博客。传送门。

#### router-link

`router-link`标签用于App.vue文件中，作用相当于a标签。

**四个属性：**

- to：用于指定跳转的路径。

- tag：tag可以指定<router-link>之后渲染成什么组件。

  比如：

  ~~~html
  <router-link to='/home' tag='li'>
  ~~~

  将会被渲染成一个<li>元素, 而不是<a>。

- replace：replace不会留下history记录, 所以指定replace的情况下, 后退键返回不能返回到上一个页面中。

- active-class：当<router-link>对应的路由匹配成功时, 会自动给当前元素设置一个router-link-active的class, 设置active-class可以修改默认的名称。

#### router-view

该标签会根据当前的路径, 动态渲染出不同的组件。

### 路由代码跳转

在某些情况下，页面跳转可能需要执行对应的JS代码，所以就需要使用代码跳转的方式。

~~~vue
<template>
  <div id="app">
    <button @click="linkToHome">首页</button>
    <button @click="linkToAbout">关于我</button>
    <router-view></router-view>
  </div>
</template>

<script>
export default {
  name: "App",
  methods: {
    linkToHome() {
      this.$router.push('/home')
    },
    linkToAbout() {
      this.$router.push('/about')
    }
  },
};
</script>

<style>
</style>

~~~

### 动态路由

在某种需求下，一个页面的`path`路径可能是不确定的，比如我们进入用户界面时，希望是`/user/aaaa`或`/user/bbbb`。

除了有前面的`/user`之外，后面还跟上了用户的ID

这种`path`和`Component`的匹配关系，我们称之为**动态路由**(也是路由传递数据的一种方式)。

**步骤：**

1. 创建`User.vue`文件：

   ~~~vue
   <template>
     <div>
       <h2>用户页面</h2>
       <p>我是用户信息</p>
       <!--方法一：计算属性，需要在computed中定义-->
       <p>我的用户名是{{userId}}</p>
       <!--方法二：直接使用插值表达式-->
       <p>我的用户名是：{{$route.params.userId}}</p>
     </div>
   </template>
   
   <script>
   export default {
     name: "User",
     computed: {
       userId() {
         return this.$route.params.userId
       }
     },
   };
   </script>
   
   <style scoped>
   </style>
   ~~~

2. 在`src/router/index.js`文件中配置路由：

   ~~~js
   {
       path: '/user/:userId',
       component: User
   }
   ~~~

   `:userId`即为动态路由最后的参数

3. 在`App.vue`中使用该组件：

   这里要使用`v-bind`指令，用于绑定`userId`。

   此处应有两种写法：

   ~~~vue
   <!-- 方法一：在data中需要定义该属性 -->
   <router-link :to="'/user/'+userId">用户1</router-link>
   
   data() {
       return {
         userId: 'zhangsan'
       }
     }
   ~~~

   ~~~vue
   <!-- 方法二用es6的模板字符串语法 -->
   <router-link :to="`/user/${userId}`">用户2</router-link>
   ~~~

4. 测试结果：

   ![image-20200626150403783](C:%5CUsers%5CLemenk%5CAppData%5CRoaming%5CTypora%5Ctypora-user-images%5Cimage-20200626150403783.png)

### 路由懒加载

当打包构建应用时，Javascript 包会变得非常大，影响页面加载。如果我们能把不同路由对应的组件分割成不同的代码块，然后当路由被访问的时候才加载对应组件，这样就更加高效了。

路由懒加载的主要作用：

- 将路由对应的组件打包成一个个的js代码块
- 只有在这个路由被访问到的时候, 才加载对应的组件

使用方式：即组件在被使用时才加载。

~~~js
/* 普通模式 */
/* import Home from '../components/Home'
import About from '../components/About'
import User from '../components/User' */
/*替换为： */
/* 懒加载模式 */
const Home = () => import('../components/Home')
const About = () => import('../components/About')
const User = () => import('../components/User')
~~~

### 路由嵌套

比如在`home`页面中, 我们希望通过`/home/news`和`/home/message`访问一些内容。一个路径映射一个组件, 访问这两个路径也会分别渲染两个组件。

![image-20200626154241103](C:%5CUsers%5CLemenk%5CAppData%5CRoaming%5CTypora%5Ctypora-user-images%5Cimage-20200626154241103.png)

1. 定义两个组件：

   ~~~vue
   //HomeMessage.vue
   <template>
     <div>
       <ul>
         <li>消息1</li>
         <li>消息2</li>
         <li>消息3</li>
         <li>消息4</li>
         <li>消息5</li>
       </ul>
     </div>
   </template>
   
   <script>
   export default {
     name: "HomeMessage"
   };
   </script>
   
   <style scoped>
   </style>
   ~~~

   ~~~vue
   //HomeNews.vue
   <template>
     <div>
       <ul>
         <li>新闻1</li>
         <li>新闻2</li>
         <li>新闻3</li>
         <li>新闻4</li>
         <li>新闻5</li>
       </ul>
     </div>
   </template>
   
   <script>
   export default {
     name: "HomeNews"
   };
   </script>
   
   <style scoped>
   </style>
   ~~~

2. 在index.js中嵌套路由

   ~~~js
   /* 懒加载模式 */
   const Home = () => import('../components/Home')
   const HomeMessage = () => import('../components/HomeMessage')
   const HomeNews = () => import('../components/HomeNews')
   
   /*其他配置*/
   
   {
       path: '/home',
       component: Home,
       /* 路由嵌套 */
       children: [
         {
           path: '',
           redirect: 'news'
         },
         {
           path: 'news',
           component: HomeNews
         },
         {
           path: 'message',
           component: HomeMessage
         }
       ]
   }
   ~~~

3. 在Home.vue中使用该组件

   ~~~vue
   <router-link to="/home/news">新闻</router-link>
       <router-link to="/home/message">消息</router-link>
       <router-view></router-view>
   ~~~

4. 测试结果：

   <img src="C:%5CUsers%5CLemenk%5CAppData%5CRoaming%5CTypora%5Ctypora-user-images%5Cimage-20200626160949160.png" alt="image-20200626160949160" style="zoom:80%;" />



### 传递参数

传递参数主要有两种类型: params和query，此前使用的是param。

1. param方式：
   - 配置路由格式: /router/:id
   - 传递的方式: 在path后面跟上对应的值
   - 传递后形成的路径: /router/123, /router/abc
2. query的方式：
   - 配置路由格式: /router, 也就是普通配置
   - 传递的方式: 对象中使用query的key作为传递方式
   - 传递后形成的路径: /router?id=123, /router?id=abc

**query方式简单使用：**

1. 创建Profile.vue

   ~~~vue
   <template>
     <div>
       <h2>个人档案</h2>
       <p>这是我的个人信息</p>
       <p>{{$route.query.name}}</p>
       <p>{{$route.query.age}}</p>
       <p>{{$route.query.sex}}</p>
     </div>
   </template>
   
   <script>
   export default {
     name: "Profile"
   };
   </script>
   
   <style scoped>
   </style>
   
   ~~~

2. 在`index.js`中配置路由

   ~~~js
   {
   	path: '/profile',
   	component: Profile
   }
   ~~~

3. 在App.vue中使用该组件

   ~~~vue
   <!--方法一：-->
   <router-link
         :to="{
         path:'/profile',
         query:{name: '张三',age: '18', sex: '男'}
       }"
       >档案</router-link>
   ~~~

   ~~~vue
   <!--方法二：-->
   <button @click="linkToProfile">档案</button>
   
   methods: {
   	linkToProfile(){
       this.$router.push({
         path: '/profile',
         query:{
           name: '李四',
           age: '20', 
           sex: '女'}
       })
     }
   }
   ~~~

   

4. 结果展示

   ![image-20200626165504440](C:%5CUsers%5CLemenk%5CAppData%5CRoaming%5CTypora%5Ctypora-user-images%5Cimage-20200626165504440.png)

### 导航守卫

在一个SPA应用中, 可以监听路由跳转来修改网页的标题。

~~~vue
router.beforeEach((to, from, next) => {
  document.title = to.matched[0].meta.title
  next()
})
~~~

三个参数的解析：

- to: 即将要进入的目标的路由对象。
- from: 当前导航即将要离开的路由对象。
- next: 调用该方法后, 才能进入下一个钩子。

并且需要给每个路由加上`mate`属性：

~~~vue
meta: {
	title: '标题'
}
~~~

### keep-alive

需求：在此前的demo中，若点击首页消息列后，进入我的或者档案页面后，再回到首页将会发现首页又回到新闻页。这是因为此前组件内的状态未被保存，组件时被再次创建的。若需要保存组件状态，则需要使用`keep-alive`。

`keep-alive` 是`Vue`内置的一个组件，可以使被包含的组件保留状态，或避免重新渲染。

- `include`字符串或正则表达，只有匹配的组件会被缓存
- `exclude`字符串或正则表达式，任何匹配的组件都不会被缓存

`router-view` 也是一个组件，如果直接被包在 `keep-alive` 里面，所有路径匹配到的视图组件都会被缓存。

~~~vue
<keep-alive>
	<router-view />
</keep-alive>
~~~

对上面的需求提供解决方案：

1. 删除index.js中对于首页子组件的默认配置。

   ~~~js
   {
       path: '/home',
       component: Home,
       meta: {
         title: '首页'
       },
       /* 路由嵌套 */
       children: [{
           path: 'news',
           component: HomeNews
         },
         {
           path: 'message',
           component: HomeMessage
         }
       ]
     }
   ~~~

2. 修改Home.vue文件

   ~~~vue
   <script>
   export default {
     name: "Home",
     data() {
       return {
         path: ''
       }
     },
     //当前组件活跃时自动调用
     activated() {
       this.$router.push(this.path);
     },
     //组件内导航守卫
     beforeRouteLeave (to, from, next) {
       this.path = this.$route.path;
       next()
     }
   };
   </script>
   ~~~

此时页面就可以完成以上的需求。

注：`activated()`是一个函数，再当前组件活跃时会自动被调用。`beforeRouteLeave`函数是组件内导航守卫，用于组件离开之前。

这里的使用方式是：在离开当前组件之前先将当前组件的`path`传给定义在`data`中的`path`，然后在当前组件活跃时再将`path`值`push`到路由中。