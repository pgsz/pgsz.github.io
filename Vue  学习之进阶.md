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

