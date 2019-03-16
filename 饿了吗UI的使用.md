# 饿了吗UI的使用

##### 介绍

* 基于Vue(当前最流行的插件)
* 网址:  http://element-cn.eleme.io/#/zh-CN/guide/desig



##### 安装:  

* cnpm i element-ui -S   (推荐使用 npm 的方式安装，它能更好地和 [webpack](https://webpack.js.org/) 打包工具配合使用。)

* CDN  (目前可以通过 [unpkg.com/element-ui](https://unpkg.com/element-ui/) 获取到最新版本的资源，在页面上引入 js 和 css 文件即可开始使用。)

  * ```js
    <!-- 引入样式 -->
    <link rel="stylesheet" href="https://unpkg.com/element-ui/lib/theme-chalk/index.css">
    <!-- 引入组件库 -->
    <script src="https://unpkg.com/element-ui/lib/index.js"></script>
    ```

* 完整引入:  在 main.js 中写入一下内容

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



##### 具体使用

* 全部都是以  el- 开头 
* 购物车的添加数量 -- InputNumber 计数器
* 主页的轮播图区域 -- Carousel 走马灯
* 登陆页面 -- Form表单   (正则匹配 + 样式 + 表单验证)  +  消息提示  
* 主页基本样式: Container 布局容器 + Layout 布局 



##### 注意

* @keyup.native.enter:  饿了吗UI的 el-input 组件上, @keyup.enter 事件无法直接使用; 需要添加 .native

