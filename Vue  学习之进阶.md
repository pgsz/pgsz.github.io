## Vue  学习之进阶



##### router-link 之详解

* 说明:在Vue项目中,如果要新开一个窗口,使用 v-link 在过去是可以的; 但在2.0出来之后, v-link 指令就被新组件 **<router-link to='指向要链接的组件'>** 代替了.  **默认渲染成带有正确链接的 <a> 标签**, 可以通过配置 **tag属性** 生成别的标签. 除此之外,当目标路由成功激活时,链接元素**自动设置一个表示激活的css类名.**

* <router-link> 比起写死的 <a> 好一些：

  * 无论是 HTML5 history 模式还是 hash 模式，它的表现行为一致，所以，当你要切换路由模式，或者在 IE9 降级使用 hash 模式，无须作任何变动。
  * 在 HTML5 history 模式下，`router-link` 会守卫点击事件，让浏览器不再重新加载页面。
  * 当你在 HTML5 history 模式下使用 `base` 选项之后，所有的 `to` 属性都不需要写 (基路径) 了。

* 注意:  <router-link> 不支持 target="_blank", 是一个开发单页应用的组件.  所以,想要打开新窗口还是要使用 a 标签

* 如果要引入路由相关信息,一定要引入 vue-router 插件

* <router-link>  组件的属性有： to， replace， append , tag , active-class , exact , event , exact-active-class,

  * to (必选参数)： 类型 string/location

    * 表示目标路由的链接。当被点击后，内部会立刻把 to 的值传到 router.push() ,使用该值可以是一个字符串， 也可以是动态绑定的描述目标位置的对象

    * ```js
      <router-link to="home">Home</router-link>
      <!-- 渲染结果 -->
      <a href="home">Home</a>
       
      <!-- 使用 v-bind 的 JS 表达式 -->
      <router-link v-bind:to="'home'">Home</router-link>
       
      <!-- 不写 v-bind 也可以，就像绑定别的属性一样 -->
      <router-link :to="'home'">Home</router-link>
       
      <!-- 同上 -->
      <router-link :to="{ path: 'home' }">Home</router-link>
       
      <!-- 命名的路由 -->
      <router-link :to="{ name: 'user', params: { userId: 123 }}">User</router-link>
       
      <!-- 带查询参数，下面的结果为 /register?plan=private -->
      <router-link :to="{ path: 'register', query: { plan: 'private' }}">Register</router-link>
      ```

  * tag: 类型：string    默认值： “a“

    * 如果想要 <rounter-link> 渲染成某种标签，例如 <li> 。可以使用 tag prop类指向何种标签， 同样它还是会监听点击，触发导航

      * ```js
        <router-link :to="'index'" tag="li" event="mouseover">Home
        ```

    * 如果此时想要在这个li标签中添加a标签，可以不为a标签设置 href 属性即可。

      * ```js
        <router-link :to="{name:'Home'}" tag="li"> <a>Home</a> </router-link>
        ```

      * 此时，<a> 作为真实的链接（它会获取正确的 href），而 ’激活时的CSS类名‘ 则设置到外层的 <li>

  * replace: 类型： boolean   默认值： false

    * 设置 replace 属性的话，当点击时，会调用 router.replace() 而不是router.push(),于是导航后不会留下 history 记录。

    * ```js
      <router-link :to={path:'/abc'} replace> </router-link>
      ```

  * append: 类型： boolean  默认值： false

    * 设置 append 属性后，则在当前（相对）路径前添加基路径。例如，我们从 /a 导航到一个相对路径 b，如果没有配置 append，则路径为 /b，如果配了，则为 /a/b

    * ```js
      <router-link :to="{ path: 'relative/path'}" append></router-link>
      ```

  * active-class： 类型： string    默认值： ’router-link-active‘

    * 设置链接激活时使用的CSS类名。默认值可以通过路由的构造选项 linkActiveClass 来全局配置

    * ```js
      <router-link :to="{path:'/about'}" active-class="activeClass"> about </router-link>
      // 默认值可以通过路由的构造选项 linkActiveClass 来全局配置
      export default new Router({
        mode:'history',
        linkActiveClass:'is-active',
        routes: [
          {
            path:'/about',
            component:about
          }
      ]
      })
      ```

  * exact-active-class： 类型： string    默认值： ’router-exact-active

    * 配置当链接被精确匹配的时候应该激活的 class。注意默认值也是可以通过路由构造函数选项  linkExactActiveClass 进行全局配置的.

  * exact :  类型： boolean  默认值： false

    * "是否激活" 默认类名的依据是 inclusive match (全包含匹配)。 举个例子，如果当前的路径是 /a 开头的，那么 <router-link to="/a"> 也会被设置 CSS 类名。

    * 按照这个规则，每个路由都会激活`<router-link to="/">`！想要链接使用 "exact 匹配模式"，则使用 `exact` 属性：

    * ```js
      <li><router-link to="/">全局匹配</router-link></li>
      <li><router-link to="/" exact>严格匹配</router-link></li>
      ```

      * 简单点说，第一个的话，如果地址是/aa,或/aa/bb，……都会匹配成功； 但加上exact，只有当地址是/时被匹配，其他都不会匹配成功

  * event: 类型： string | Array（string）    默认值： click

    * 声明可以用来触发导航的事件

    * ```js
      <router-link to="/document" event="mouseover">document</router-link>
      ```

    * 如果我们不加event，那么默认情况下是当我们点击document的时候，跳转到相应的页面，但当我们加上event的时候，就可以改变触发导航的事件，比如鼠标移入事件

------

##### 导航守卫

* '导航' 表示路由正在发生改变
* vue-router 提供的导航守卫主要用来通过跳转或取消的方式守卫导航. 有多种机会植入路由导航过程中:  全局的, 单个路由独享的, 或者组件级的.
* 注意: 参数查询的改变并不会触发进入/离开的导航守卫. 此时可以通过观察 $router 对象来应对这些变化, 或者使用 beforeRouterUpdate 的组件内守卫.
* 应用场景: 帮我们解决一些在组件加载之前,可以做一些操作,尤其是异步操作. 这样可以避免组件加载完毕后,却没有数据的尴尬. 当然,不应该在组件加载之前做太多的异步操作,会导致页面白屏时间过长,用户体验很差.,慎用. 
  * 登录拦截: 没有登录信息,无法访问其他页面,跳转到登录页
* 守卫队列
  * 守卫两大种类: 前置守卫,  后置守卫
  * 前置守卫: 
    * 全局的前置守卫: beforeEach   beforeResolve
    * 路由独享的守卫: beforeEnter
    * 组件内的守卫: beforeRouteEnter   beforeRouteUpdate   beforeRouteLeave
  * 后置守卫
    * 全局的后置守卫: afterEach

###### 全局前置守卫

* 可以使用 router.beforeEach 注册一个或多个全局前置守卫

* 当一个导航触发时，全局前置守卫按照创建顺序调用。守卫是异步解析执行，此时导航在所有守卫 resolve 完之前一直处于 **等待中**. (所有不宜有太多的异步加载)

* ```js
  const router = new VueRouter({ ... })
  router.beforeEach((to, from, next) => {
      // to: 即将进入的路由对象   from 即将离开的路由对象
      if(to.path == '/login'){  
          next()    // 前往登录页
      } else {   // 不是登录页
          if(window.sessionStorage.getItem('tokenNight')){
              next()    // 有 tokenNight 则可以进入
          } else {
              next('/login')  // 否则前往登录页进行登录
          }
      }
    })
  ```

* 每个守卫方法有三个参数

  * to:  Route  :即将要进入的目标路由对象
  * from: Route: 当前当行正要离开的路由
  * next: Function:  一定要调用该方法来 resolve 这个钩子。执行效果依赖 next 方法的调用参数。
    * next(): 进行管道中的下一个钩子。如果全部钩子执行完了，则导航的状态就是 confirmed (确认的).
    * next(false): 中断当前的导航。如果浏览器的 URL 改变了 (可能是用户手动或者浏览器后退按钮)，那么 URL 地址会重置到 from 路由对应的地址。
    * next('/') 或者 next({ path: '/' }): 跳转到一个不同的地址。当前的导航被中断，然后进行一个新的导航。你可以向 next 传递任意位置对象，且允许设置诸如 replace: true、name: 'home' 之类的选项以及任何用在 router-link 的 to prop 或 router.push 中的选项.
    * next(error): (2.4.0+) 如果传入 next 的参数是一个 Error 实例，则导航会被终止且该错误会被传递给 router.onError() 注册过的回调
    * 要确保调用 next 方法,否则钩子就不会被 resolved

###### 全局解析守卫

* 可以用 router.beforeResolve 注册全局守卫, 与 router.beforeEach 类似. 区别在于:  在导航被确认前, 同时在所有组件内守卫和异步路由组件被解析之后, 才被调用

###### 组件内的守卫

```js
const Foo = {
  template: `...`,
  beforeRouteEnter (to, from, next) {
    // 在渲染该组件的对应路由被 confirm 前调用
    // 不！能！获取组件实例 `this`
    // 因为当守卫执行前，组件实例还没被创建
    // 支持给 next 传递回调的唯一守卫. 其他两个 this 已经可用,所有不支持传递回调,因为没必要了.
  },
  beforeRouteUpdate (to, from, next) {
    // 在当前路由改变，但是该组件被复用时调用
    // 举例来说，对于一个带有动态参数的路径 /foo/:id，在 /foo/1 和 /foo/2 之间跳转的时候，
    // 由于会渲染同样的 Foo 组件，因此组件实例会被复用。而这个钩子就会在这个情况下被调用。
    // 可以访问组件实例 `this`
  },
  beforeRouteLeave (to, from, next) {
    // 导航离开该组件的对应路由时调用
    // 可以访问组件实例 `this`
    // 通常用来禁止用户在还未保存修改前突然离开
    //  导航离开该组件的对应路由时调用，我们用它来禁止用户离开，比如还未保存草稿，或者在用户离开前，将setInterval销毁，防止离开之后，定时器还在调用。
      const answer = window.confirm('Do you really want to leave? you have unsaved changes!')
      if (answer) {
          next()
      } else {
          next(false)  // 可以通过 next(false)取消
      }
   }
}
```

###### 全局后置钩子

* 和前置守卫不同:  不会接受 next 函数,也不会改变导航本身

* ```js
  router.afterEach((to, from) => {
    // ...
  })
  ```

###### 完整的导航解析流程

* ```text
  // 官网
  1: 导航被触发。
  2: 在失活的组件里调用离开守卫。
  3: 调用全局的 beforeEach 守卫。
  4: 在重用的组件里调用 beforeRouteUpdate 守卫 (2.2+)。
  5: 在路由配置里调用 beforeEnter。
  6: 解析异步路由组件。
  7: 在被激活的组件里调用 beforeRouteEnter。
  8: 调用全局的 beforeResolve 守卫 (2.5+)。
  9: 导航被确认。
  10:调用全局的 afterEach 钩子。
  11:触发 DOM 更新。
  12:用创建好的实例调用 beforeRouteEnter 守卫中传给 next 的回调函数。
  ```

* 导航守卫的执行顺序:  beforeRouteLeave < beforeEach < beforeRouteUpdate < beforeEnter < beforeRoureEnter < beforeResolve < afterEach

###### 解惑

* https://baijiahao.baidu.com/s?id=1612908077665784077&wfr=spider&for=pc

* 导航守卫中 next 的用处:  使导航守卫队列继续往下迭代.
* 为什么 afterEach 守卫没有 next: afterEach根本不在导航守卫队列内,

 没有迭代的 next.

* beforeEach 是否可以叠加:  beforeEach 是可以叠加的, 所有的全局前置守卫按顺序存放在 beforeHooks 的数组里面
* 路由的跳转经历了哪几部分:  路由跳转的核心方法是 transitionTo, 在跳转过程中经历了一次 confirmTransition.

------

##### 触发钩子的完整顺序

* https://juejin.im/post/5b41bdef6fb9a04fe63765f1

* 将路由导航, keep-alive, 和组件生命周期钩子结合起来,触发顺序. 假设是从a组件离开,第一次进入b组件
  1.  beforeRouteLeave: 路由组件的组件离开路由前钩子,可取消路由离开
  2.  beforeEach: 路由全局前置守卫,可用于登陆验证,全局路由loading等
  3.  beforeEnter: 路由独享守卫
  4.  beforeRouteEnter: 路由组件的组件进入路由前的钩子
  5.  beforeResolve: 路由全局解析守卫
  6.  afterEach: 路由全局后置钩子
  7.  beforeCreate: 组件生命周期,不能访问this
  8.  created: 组件生命周期,可以访问this,不能访问dom
  9.  beforeMount: 组件生命周期
  10.  deactivated: 离开缓存组件a,或者触发a的 beforeDestroy 和 destroyed组件销毁钩子
  11.  mounted: 访问/操作dom
  12. activated: 进入缓存组件,进入a的嵌套子组件(如果有的话)
  13.  执行 beforeRouteEnter 回调函数next.







