# Vue 的学习总结

### Vue - 渐进式JavaScript框架



##### 介绍

* vue的中文网   https://cn.vuejs.org/v2/guide/
* vue的github  https://github.com/vuejs/vue
* 两大最核心功能: 
  * 1.响应式的数据绑定系统 
  * 2.组件系统



##### 库和框架的区别

* https://zhuanlan.zhihu.com/p/26078359?group_id=830801800406917120

* 库(Libray):本质上是一些函数的集合.每次调用函数.实现一个特定的功能,接着把控制权交给使用者

  * 代表:jQuery    核心:DOM操作,封装DOM操作,简化DOM操作

* 框架(Framework):是一套完整的解决方案,使用框架的时候,想要把代码放到框架合适的地方,框架会在合适的时机调用你的代码

  * 框架规定了自己的编程方式,是一套完整的解决方案
  * 使用框架的时候,由框架控制一切,我们只需要按照规则写代码

* 主要区别:

  * You call Libray,  Framework calls you

  * 核心点:  谁起到主导作用

    * 框架控制整个流程的是框架

    * 使用库,由开发人员决定如何调用库中提供的方法

      

##### MVVM的介绍

* http://www.cnblogs.com/indream/p/3602348.html
* MVC
  * M: Model数据模型(专门用来操作数据)
  * V:View视图(页面)
  * C:Controller控制器(处理业务逻辑)
* MVVM 
  * MVVM ==> M / V  / VM
  * M: model数据模型
  * V:view视图
  * VM:view model视图模型
  * ![](D:\study\个人知识总结\pgsz.github.io\img\mvvm.png)
* MVVM通过数据双向绑定,让数据自动的双向同步
  * V(修改数据) -> M
  * M(修改数据) ->V
  * 数据是核心
  * Model和View并无直接关联,而是通过ViewModel来进行联系的,Model和ViewModel之间有着双向数据绑定的联系,因此Model中数据改变时会触发View层的刷新,View中由于用户交互操作而改变的数据也会在Model中同步
* 学习Vue要转变思路,不要想着怎么操作DOM,而是如何操作数据 !!!



##### 起步学习Vue

* 安装:  npm install vue

* 导包: 

  ```html
  <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>  
  或者 <script src="https://cdn.jsdelivr.net/npm/vue"></script>
  ```

* 使用

  * 先在data中声明数据,再使用数据
  * 只有data中的数据才是响应式的,动态添加进来的数据默认为非响应式

  ```html
  <div id="app">
      {{msg}}  {{content}}
  </div>
  <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
  <script>
      // 使用vue
      let app = new Vue({
          // 选择器  可以是css选择器 建议写id  不能是 body 和 html
          el: '#app',
          // 数据
          data: {
              msg: '今天是个好日子',
              content: 'hello world'
          }
      })
  </script>
  ```



##### 双向数据绑定

* 将DOM与Vue实例的data数据绑定到一起,彼此之间相互影响 
  * 数据的改变会引起DOM的变化
  * DOM的改变也会引起数据的变化
*  http://www.cnblogs.com/kidney/p/6052935.html       https://segmentfault.com/a/1190000006599500
* 原理: object.defineProperty 中的 get 和 set 方法
  * getter 和 setter :访问器
  * 作用: 指定读取或设置对象属性值的时候,进行操作
* https://cn.vuejs.org/v2/guide/reactivity.html
* https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object/defineProperty



##### 指令

- 解释:指令是带有 v- 前缀的特殊属性

- v-text

  - 解释:更新DOM对象的textContent;识别文本,无法识别标签

    ```html
    <h1 v-text="msg"></h1>     简写:<h1>{{msg}}</h1>  
    ```

- v-html

  - 解释:更新DOM对象的innerHTML;识别文本与标签

  - ```html
    <h1 v-html="msg"></h1>
    ```

- v-on

  - 作用:绑定事件

  - 语法: 

    ```html
    v-on:click='doThis'   简写 @click='doThis'
    ```

  - 修饰符

    ```html
    .stop - 调用 event.stopPropagation()。
    .prevent - 调用 event.preventDefault()。
    .capture - 添加事件侦听器时使用 capture 模式。
    .self - 只当事件是从侦听器绑定的元素本身触发时才触发回调。
    .{keyCode | keyAlias} - 只当事件是从特定键触发时才触发回调。
    .native - 监听组件根元素的原生事件。
    .once - 只触发一次回调。
    .left - (2.2.0) 只当点击鼠标左键时触发。
    .right - (2.2.0) 只当点击鼠标右键时触发。
    .middle - (2.2.0) 只当点击鼠标中键时触发。
    .passive - (2.3.0) 以 { passive: true } 模式添加侦听器
    ```

- v-model	

  - 作用:在表单元素或组件上创建双向数据绑定

  - 限制:

    ```html
    <input>   <select>   <textarea>   components
    ```

  - 修饰符

    ```html
    .lazy - 取代 input 监听 change 事件
    .number - 输入字符串转为有效的数字
    .trim - 输入首尾空格过滤
    ```

  ```html
  <input type="text" placeholder="请输入内容" v-model='msg'>
  <p @click='changeValue'> msg is : {{msg}} </p>
  ```

- v-for

  - 作用:基于数据多次渲染元素或模板快

  - ```html
    <div v-for="item in items">{{ item.text }}</div>
    <div v-for="(item, index) in items"></div>
    <div v-for="(val, key) in object"></div>
    <div v-for="(val, key, index) in object"></div>
    ```

- key属性

  - 推荐:使用 v-for 的时候提供 key 属性,以获得性能的提升

  - 说明:使用key,Vue会基于key的变化重新排序元素顺序,并且会移除key不存在的元素

  - ```html
    <div v-for="item in items" :key="item.id"> <!-- 内容 --></di>
    ```

  - https://www.zhihu.com/question/61064119/answer/183717717  或  https://cn.vuejs.org/v2/guide/list.html#key

- v-bind

  - 作用:动态地绑定一个或多个特性，或一个组件 prop 到表达式。当表达式的值改变时,将其产生的连带影响,响应式的作用于DOM

  - 语法:    v-bind:title="msg"   简写    :title="msg"

  - ```html
    <img v-bind:src=" it.male ? './images/boy.png' : './images/girl.png'" alt="">
    <li><a href="#/all" :class="{selected : chooseString == 'All'}">All</a></li>
    <li v-for='(item,index) in weatherLister' :key='item.date' :style="{'transition-delay':index*300 + 'ms'}" ></li>
    ```

- v-if 和 v-show

  - 条件渲染
  - v-if , v-else, v-else-if
  - v-if : 根据表达式的值的真假条件,销毁或重建元素
  - v-show : 根据表达式值的真假,切换display的CSS属性
  - 频繁切换某个元素的显示与隐藏时,使用v-show,性能消耗少

- 提升性能 v-pre

  - 跳过这个元素和它的子元素的编译过程。可以用来显示原始 Mustache 标签。跳过大量没有指令的节点会加快编译。

  - ```html
    <span v-pre>{{ this will not be compiled }}</span>
    ```

- 提升性能 v-once

  - 只渲染元素和组件**一次**。随后的重新渲染，元素/组件及其所有的子节点将被视为静态内容并跳过。这可以用于优化更新性能。

  - ```html
    <span v-once>This will never change: {{msg}}</span>
    ```

- v-cloak

  - 这个指令保持在元素上直到关联实例结束编译。和 CSS 规则如 `[v-cloak] { display: none }` 一起用时，这个指令可以隐藏未编译的 Mustache 标签直到实例准备完毕。

    - ```css
      [v-cloak] {display: none;}
      ```

    - ```html
      <div v-cloak>{{ message }}</div>
      ```


###### 自定义指令

- 作用:进行DOM操作

- 使用场景:对纯DOM元素进行底层操作.  比如:文本框获得焦点

- 两种指令   1.全局指令  2.局部指令

- https://juejin.im/entry/58b7c5d8ac502e006cfee34a

- ```js
   // 注册一个全局自定义指令 `v-focus`
      // 第一个参数:指令名称  第二个参数:配置对象,指定指令的钩子函数
      Vue.directive('focus', {
          // 当被绑定的元素插入到 DOM 中时……
          update: function (el) {
              // 聚焦元素
              el.focus()
          }
      })
  ```

  ```html
  <input v-focus>   //模板中任何元素使用
  ```




##### 计算属性

* 说明:计算属性是基于他们的依赖进行缓存的,只有在他的依赖发生改变时才会重新求值

* 注意:Mustache语法（{{}}）中不要放入太多的逻辑，否则会让模板过重、难以理解和维护

* computed中的属性不能与data中的属性同名,否则会报错

* http://www.cnblogs.com/kidney/p/7384835.html?utm_source=debugrun&utm_medium=referral

* 计算属性的setter

  ```js
  computed: {
      checkAll: {   // 顶部全选
        get() {   // 取值
          let arr = this.todoLists.filter(val => {   // 遍历这个数组
            return val.flag   // 返回选中的个数
          })
          return (this.todoLists.length == arr.length)   // 返回作比较的结果
        },
        set(res) {   // 赋值
          this.todoLists.filter(val => {
            val.flag = res
          })
        }
      }
    }
  ```

###### 侦听器

* 语法: watch(){  数据名(新值,旧值){} }     data中的数据

* 作用: 在数据改变的时候,要执行自定义的逻辑

* 注意: 如果侦听的是复杂数据类型(数组,对象), 侦听器接收到的参数,跟data中侦听的数据其实是同一个

* ```js
    getData() {
        // 发送请求,获取数据
        this.$http
          .get(`site/goods/getgoodsinfo/${this.$route.params.id}`)
          .then(res => {
            window.console.log(res);
            this.goodsinfo = res.data.message.goodsinfo;
            this.hotgoodslist = res.data.message.hotgoodslist;
            this.imglist = res.data.message.imglist;
          });
      }
    },
    watch: {   // 侦听器
      $route(newVal) {
        console.log(this.$route == newValue);   // newValue和this.$route是一个东西
        this.getData()
      }
    },
    created() {  // 生命周期函数,实例化之后
      window.scrollTo(0, 0);  // 回到顶部
      this.getData()
    }
  ```

  



##### 进入/离开 & 列表过渡

* 在 CSS 过渡和动画中自动应用 class
* 可以配合使用第三方 CSS 动画库，如 Animate.css
* 在过渡钩子函数中使用 JavaScript 直接操作 DOM
* 可以配合使用第三方 JavaScript 动画库，如 Velocity.js

###### 一. 单元素动画

* 当我们使用transition 这个标签包裹需要动画的元素时(name属性 设置的跟选择器一致)
*  Vue会自动的添加跟移除动画的类名 从而有动画效果,
* 元素显示跟隐藏的类名是不同的:进入 enter, 离开 leave

###### 二. 列表过渡

* 使用<transition-group>组件,会以一个真实的元素呈现,默认为一个<span>,可以通过 tag 特性更换为其他元素
* 内部元素总是需要提供唯一的 key 属性值



##### axios (插件)

* 安装: cnpm i axios --save    https://unpkg.com/axios/dist/axios.min.js
* axios的github: https://github.com/axios/axios
* 以Promise为基础的HTTP客户端，适用于：浏览器和node.js
* 独立的axios库,封装ajax,用来发送请求,异步获取数据

  * axios结合vue使用想要考虑this的指向问题(箭头函数)
  * 一般会设置 Vue.prototype.$http = axios 用起来和 Vue-resource


###### axios 与 vue-resource的区别

* vue-resource可以发 jsonp.  axios不支持jsonp,为了保证自身的轻量级,建议导入其他库来实现

* JSONP:只能发get,数据量小,不安全,兼容性好,只支持script标签

* CORS:get,post都可以,数据量大,更安全,兼容行差一些

* ```js
  // 在浏览器中使用，直接引入js文件使用下面的GET/POST请求方式即可
  // 1 引入 axios.js
  // 2 直接调用axios提供的API发送请求
  created: function () {
    axios.get(url)
      .then(function(resp) {})
  }
  ---
  // 配合 webpack 使用方式如下：
  import Vue from 'vue'
  import axios from 'axios'
  // 将 axios 添加到 Vue.prototype 中
  Vue.prototype.$axios = axios
  ---
  // 在组件中使用：
  methods: {
    getData() {
      this.$axios.get('url')
        .then(res => {})
        .catch(err => {})
    }
  }
  ---
  // API使用方式：
  axios.get(url[, config])
  axios.post(url[, data[, config]])
  axios(url[, config])
  axios(config)
  ```


###### Get请求

* ```js
  // url中带有query参数
  axios.get('https://autumnfish.cn/search?keywords=${this.searchValue}')
    .then(function (response) {
      console.log(response);
    })
    .catch(function (error) {
      console.log(error);
    });
  // url和参数分离，使用对象
  axios.get('https://autumnfish.cn/search', {
    params: {
      keywords:${this.searchValue}
    }
  })
  ```

###### Post请求

* 不同环境中处理POST请求  https://github.com/axios/axios#using-applicationx-www-form-urlencoded-format
* 默认情况下，axios 会将JS对象序列化为JSON对象.

###### 请求配置

* axios.defaults.baseURL = 'http://111.230.232.110:8899/';
  * 设置一个'baseURL',便于为 axios 实例的方法传递相对URL




##### 组件

* 组件系统是 Vue 的另一个重要概念，因为它是一种抽象，允许我们使用小型、独立和通常可复用的组件构建大型应用。仔细想想，几乎任意类型的应用界面都可以抽象为一个组件树
* 创建组件的两种方式:  1.全局组件  2.局部组件

###### 全局组件

* 全局组件在所有的vue实例中都可以使用

* 注意: 先注册组件,再初始化根实例

* ```html
  <script>
  // 1 注册全局组件  
  Vue.component('my-component', {
    // template 只能有一个根元素
    template: '<p>A custom component!</p>',
    // 组件中的 `data` 必须是函数 并且函数的返回值必须是对象
    data() {
      return {
        msg: '注意：组件的data必须是一个函数！！！'
      }
    }
  })
  </script>
  // 2 使用：以自定义元素的方式
  <div id="example">
    <my-component></my-component>
  </div>
  // =====> 渲染结果
  <div id="example">
    <p>A custom component!</p>
  </div>
  // 3 template属性的值可以是：
    - 1 模板字符串
    - 2 模板id  template: '#tpl'
  <script type="text/x-template" id="tpl">
    <p>A custom component!</p>
  </script>
  <script>
  // 根实例(最顶级的Vue实例,想要设置el
  let example = new Vue({
  	el: '#example'
  })
  </script>
  ```

###### 局部组件

* 局部组件,是在某一个具体的vue实例中定义的,只能在这个vue实例中使用

* ```js
  var Child = {
    template: '<div>A custom component!</div>'
  }
  
  new Vue({
    // 注意：此处为 components
    components: {
      // <my-component> 将只在当前vue实例中使用
      // my-component 为组件名 值为配置对象 
      'my-component': {
        template: ``,
        data () { return { } },
        props : []
      }
    }
  })
  ```



##### 路由

* 路由: 浏览器的哈希值(#hash)与展示视图内容(template)之间的对应规则

* vue中的理由是: hash 和 component 的对用关系(url和组件是一对一的对应关系)

* 安装:  npm i  vue-router      https://unpkg.com/vue-router/dist/vue-router.js

  ```text
  嵌套的路由/视图表
  模块化的、基于组件的路由配置
  路由参数、查询、通配符
  基于 Vue.js 过渡系统的视图过渡效果
  细粒度的导航控制
  带有自动激活的 CSS class 的链接
  HTML5 历史模式或 hash 模式，在 IE9 中自动降级
  自定义的滚动条行为
  ```

###### 基本使用

```html
  <div id="app">
    <h1>Hello App!</h1>
    <p>
      <!-- 使用 router-link 组件来导航. -->
      <!-- 通过传入 `to` 属性指定链接. -->
      <!-- <router-link> 默认会被渲染成一个 `<a>` 标签 -->
      <router-link to="/foo">Go to Foo</router-link>
      <router-link to="/bar">Go to Bar</router-link>
      <router-link to="/son">Go to Son</router-link>
    </p>
    <!-- 路由出口 -->
    <!-- 路由匹配到的组件将渲染在这里 tab-content-->
    <router-view></router-view>
  </div>
<script src="https://unpkg.com/vue/dist/vue.js"></script>
<!-- vue-router 是vue插件 -->
<script src="https://unpkg.com/vue-router/dist/vue-router.js"></script>
<script>
  // 0. 如果使用模块化机制编程，导入Vue和VueRouter，要调用 Vue.use(VueRouter)
  // 1. 定义 (路由) 组件。  可以从其他文件 import 进来  组件的简单写法
  const Foo = {
    // 模板
    template: '<div>foo</div>'
  }
  const Bar = {
    template: '<div>bar</div>'
  }
  const Son = {
    template: '<div>Son</div>'
  }
  // 2. 定义路由 
  // url跟组件的对应关系
  const routes = [{
      path: '/foo',
      component: Foo
    },
    {
      path: '/bar',
      component: Bar
    },
    {
      path: '/son',
      component: Son
    }
  ]
  // 3. 创建 router 实例，然后传 `routes` 配置
  // 你还可以传别的配置参数
  const myrouter = new VueRouter({
    routes // (缩写) 相当于 routes: routes
  })

  // 4. 创建和挂载根实例。  记得要通过 router 配置参数注入路由， 从而让整个应用都有路由功能
  const app = new Vue({
    el:"#app",
    // 告诉页面中的顶级Vue实例，按照这个路由对象的url->组件的关系 渲染内容
    router:myrouter
  })
  // $mount等同于 el：‘选择器’
  // .$mount('#app')
</script>
```

###### 重定向

* ```js
  //  将path 重定向到 redirect
  { path: '/', redirect: '/home' }
  ```

* 路由其他配置
  * 路由导航高亮
    * 当前匹配的导航链接,会自动添加 router-link-exact-active router-link-active 类

###### 动态路由匹配

* https://blog.csdn.net/dkr380205984/article/details/82353221

* ```js
  // 我们经常需要把某种模式匹配到的所有路由，全都映射到同个组件。例如，我们有一个 User 组件，对于所有 ID 各不相同的用户，都要使用这个组件来渲染。
  const User = {
    template: '<div>User</div>'
  }
  
  const router = new VueRouter({
    routes: [
      // 动态路径参数 以冒号开头
      { path: '/user/:id', component: User }
    ]
  })
  // 一个“路径参数”使用冒号 : 标记。当匹配到一个路由时，参数值会被设置到 this.$route.params，可以在每个组件内使用。于是，我们可以更新 User 的模板，输出当前用户的 ID：
  const User = {
    template: '<div>User {{ $route.params.id }}</div>'
  }
  ```


###### VueRouter嵌套路由

* 路由是可以嵌套的, 既:路由中又包含子路由

* 规则: 父组件中包含 router-view, 在路由规则中使用 children 配置

* ```js
  // 父组件：
   const User = Vue.component('user', {
     template: `
       <div class="user">
         <h2>User Center</h2>
         <router-link to="/user/profile">个人资料</router-link>
         <router-link to="/user/posts">岗位</router-link>
         <!-- 子路由展示在此处 -->
         <router-view></router-view>
       </div>
       `
   })
   // 子组件[简写]
   const UserProfile = {
     template: '<h3>个人资料：张三</h3>'
   }
   const UserPosts = {
     template: '<h3>岗位：FE</h3>'
   }  
   // 路由
   var router =new Router({
     routers : [
       { path: '/user', component: User,
         // 子路由配置：
         children: [
           {
             // 当 /user/profile 匹配成功，
             // UserProfile 会被渲染在 User 的 <router-view> 中
             path: 'profile',   // 注意:此处路径不需要带'/' 相当于 /user/profile 
             component: UserProfile
           },
           {
             // 当 /user/posts 匹配成功
             // UserPosts 会被渲染在 User 的 <router-view> 中
             path: 'posts',
             component: UserPosts
           }
         ]
       }
     ]
   })
  ```


###### 编程式的导航

* 除了使用 `<router-link>` 创建 a 标签来定义导航链接，我们还可以借助 router 的实例方法，通过编写代码来实现
* 声明式: <router-like :to="...">     编程式: router.push(...)   router.replace(...)   router.go(...)
  * 区别: 声明式是a标签,点击跳转  而编程式是代码跳转,结合逻辑进行跳转(进行逻辑判断. 如:登陆跳转)
* router.push(location, onComplete?, onAbort?)
  * 注意: 在Vue实例内部,可以通过 router 访问路由实例,可以调用 **this.​$router.push**



##### vue脚手架

* 说明:框架工具包,给我们封装了很多功能,

* 基于nodejs, 会把我们写的xxxx.vue文件翻译浏览器可以识别的内容

* 安装: cnpm install-g @vue/cli

  * vue create 项目名,根据提示内容直接回车即可

  * ```text
     没有管理员权限
       1: 以管理员身份打开 cmd
       2: 进入目标文件 D盘 d: 回车
       3: 输入cd 空格 复制粘贴文件路径 回车
       4: 输入 vue create 项目名
       5: 回车 直到出现两句话 复制粘贴输入回车
       6: npm run serve
    ```

  * 进入项目目录: cd 项目目录

  * 使用node编译项目: npm run serve 

* 文件结构

  * xxx.vue: 单文件组件  (template: 结构;  script: 逻辑, export.default{data,methods,computed};  style:样式)
  * main.js: 最顶级的Vue对象; 模块的使用,全部在这里导入 import  xxx  from  'xxx'
  * assets: 静态资源
  * public: 基础网页(index.html;  favicon,ico)



##### 过滤器 filter

* 作用:文本数据格式化; (不会对原始数据进行修改,只是返回新数据)
  * 与计算属性的区别: 计算属性修改数据,会改变原始数据
* 过滤器的本质 是一个有参数 有返回值的方法
* 过滤器的两个条件:
  * 过滤器的名称, 处理函数
  * 过滤器中的function 第一个参数已经被规定死了,永远是过滤器管道符前面传递过来的数据
  * 过滤器函数始终以表达式的值作为第一个参数.带引号的参数视为字符串,而不带引号的参数按表达式计算 
* 可以用在两个地方:  {{ }} 和 v-bind 表达式
* 两种过滤器: 1. 全局过滤器  2. 局部过滤器

###### 参数写法

* https://www.jianshu.com/p/ad21df1914c5

* {{ message | filterA | filterB }}
  * message是作为参数传给filterA 函数,而filterA 函数的返回值作为参数传给filterB函数,最终结果显示是由filterB返回的.
* {{ message | filterA('arg1', arg2) }}
  * filterA的第一个参数是message,依次是‘arg1’,arg2
*  {{ 'a','b' | filterB }}
  * 'a'和'b'分别作为参数传给filterB

###### 全局过滤器

* 说明: 通过全局方式创建的过滤器,在任何一个vue实例中都可以使用

* 注意: 使用全局过滤器的时候,需要先创建全局过滤器,再创建Vue实例

* 显示的内容由过滤器的返回值决定

* ```html
  <span>{{item.add_time | formatinTime('YYYY-MM-DD HH:mm:ss')}}</span>
  <script>
  Vue.filter('formatinTime', (val,arg) => {
    return moment(val).format(arg)
  })
  </script>
  ```


###### 局部过滤器

* 说明: 局部过滤器是在某一个vue实例内容创建的,只有当前实例中起作用

* ```js
  {
    data: {},
    // 通过 filters 属性创建局部过滤器
    // 注意：此处为 filters
    filters: {
      filterName: function(value, format) {}
    }
  }
  ```



##### 生命周期钩子函数

* 说明: 一个组件从开始到最后消亡所经历的各种状态,就是一个组件的生命周期 (通俗的说:vue提供的回调函数,在各个关键时刻触发,让开发人员添加自己的逻辑)
* Vue的生命周期函数通常有三类
  * 实力创建时的生命周期函数
  * 实力执行时的生命周期函数
  * 实例销毁时的生命周期函数
* 注意: Vue在执行过程中会自动调用生命周期钩子函数,我们只需调用这些钩子函数即可!  钩子函数的名称都是Vue中规定好了的,不能修改.

###### 钩子函数 - beforeCreate()

* 说明: 在实例初始化之后,数据观测(data observer)和 event/watcher 事件配置之前被调用
* 注意: 此时是无法获取 data 中的数据, methods 中的方法

###### 钩子函数 - created()

* 说明: 在实例创建完成后被立即调用。在这一步，实例已完成以下的配置：数据观测 (data observer)，属性和方法的运算，watch/event 事件回调。然而，挂载阶段还没开始，`$el` 属性目前不可见
* 注意: 这是一个常用的生命周期钩子函数,可以改变 data 中的数据, 调用methods 中的方法,并且修改可以通过vue都响应式绑定体现在页面上,获取computed中的计算属性等等
* 使用场景: 对实例进行预处理. 发送请求, 获取数据,但这个周期函数中没有方法来对实例化过程进行拦截, 如果某些数据必须获取才允许进入页面的话,建议在组件路由钩子beforeRouteEnter中来完成



##### Vue插件的封装

* 插件的分类

  * 添加全局方法或者属性, 如: vue-custom-element
  * 添加全局资源: 指令/过滤器/过渡等, 如: vue-touch
  * 通过全局mixin方法添加一些组件选项, 如: vue-router
  * 添加Vue实例方法,通过把他们添加到 Vue-prototype 上实现
  * 一个库,提供自己的 API, 同时提供上面提到的一个或多个功能,如: vue-router

* 使用插件

  * 通过全局方法 Vue.use( ) 使用插件. 

  * 注意: 需要在你调用 new Vue( ) 启动应用前完成

  * ```js
    // 调用 `MyPlugin.install(Vue)`
    Vue.use(MyPlugin)
    // 也可以传入一个对象
    Vue.use(MyPlugin,{someOption: true})
    
    new Vue({
      //... options
    })
    ```

* 插件开发

  * Vue.js的插件有个公开方法 install.  这个方法第一个参数是 Vue 构造函数,第二个参数是 可选的选项对象

  * 无论采取什么方式,最终目的是在项目中可以使用,借助 install 的Vue参数具体自己进行封装

  * https://juejin.im/post/5b96586de51d450e7d0984a6

  * ```js
    MyPlugin.install = function (Vue, options) {
      // 1. 添加全局方法或属性
      Vue.myGlobalMethod = function () {
        // 逻辑...
      }
      // 2. 添加全局资源
      Vue.directive('my-directive', {
        bind (el, binding, vnode, oldVnode) {
          // 逻辑...
        }
        ...
      })
      // 3. 注入组件
      Vue.mixin({
        created: function () {
          // 逻辑...
        }
        ...
      })
      // 4. 添加实例方法
      Vue.prototype.$myMethod = function (methodOptions) {
        // 逻辑...
      }
    }
    ```



##### 饿了吗UI  (基于vue)

* 安装: cnpm i element-ui -S

  * ```html
    <!-- 引入样式 -->
    <link rel="stylesheet" href="https://unpkg.com/element-ui/lib/theme-chalk/index.css">
    <!-- 引入组件库 -->
    <script src="https://unpkg.com/element-ui/lib/index.js"></script>
    ```

* 完整引入

  * ```js
    import Vue from 'vue';
    import ElementUI from 'element-ui';   // 导入饿了吗ui
    import 'element-ui/lib/theme-chalk/index.css';  //样式文件想要单独引入
    import App from './App.vue';
    
    Vue.use(ElementUI);   // use一下,才能使用
    
    new Vue({   // 实例化顶级对象
      el: '#app',
      render: h => h(App)
    });
    ```

###### 具体使用:

- 全部都是以  el- 开头 
- 购物车的添加数量 -- InputNumber 计数器
- 主页的轮播图区域 -- Carousel 走马灯
- 登陆页面 -- Form表单   (正则匹配 + 样式 + 表单验证)  +  消息提示  
- 主页基本样式: Container 布局容器 + Layout 布局 
-  window.sessionStorage.setItem/getItem/removeItem() 