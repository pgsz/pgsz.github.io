# 01-fs文件模块读写模式

## 1.1-readFile读取文件

```javascript
// 导入文件
const fs = require('fs');

//读取文件
/*
  第一个参数: 文件路径
  第二个参数: 编码格式 (可选参数,默认为二进制)
  第三个参数: 读取回调函数(异步操作)
           err:如果读取成功,err为null, 否则读取失败(一般文件路劲错误或者找不到文件夹)
           data:读取的数据
*/

fs.readFile('./data/aaaa.txt','utf8',(err,data)=>{
    if(err){
        //读取失败:抛出异常
        //抛出异常，node停止工作告诉你错误原因：方便调试
        throw err;
    }else{
        console.log(data);
    };
})

//同步读取文件(了解即可，几乎不用,一般在异步的api后面加上Sync就是同步)
let data = fs.readFileSync('./data/aaa.txt','utf-8');
console.log(data);
//阻塞线程
console.log('2222');
```

## 1.2-writFeile写入文件

```javascript
// 导入文件
const fs = require('fs');

// 写入文件
// 第一个参数: 文件路劲;
// 第二个参数: 要写入的数据
// 第三个参数: 文件编码 默认utf-8
// 第四个参数: 异步回调函数
fs.writeFile('./data/bbb.txt','加油,fighting',(err)=>{
    if(err){
        throw err;
    } else {
        console.log('读写成功');
    }
})
```


# 拓展之异步与同步的区别

```javascript
/* js从上往下解析代码流程
     1:判断是同步还是异步
         同步:立即执行
         异步:不执行,放入事件队列
     2:继续往下执行
     3:如果页面同步代码全部执行完毕之后,再执行异步代码
*/

/* 异步操作:
    1:不会阻塞线程(性能高)
    2:没有顺序(随机)
    3:有回调函数
*/

    var t = true;
    //这是一个异步操作：不管写在那里，都是等页面所有同步执行完毕之后再执行
    window.setTimeout(function(){
            t = false;
        },1000);

    while(t){
    };
    alert('end');

    /*js从上往下解析代码
        //第一行：同步
        var t = true;
        //第二行:异步  不执行，放入事件队列
        //第三行：同步
        while(t){}; //死循环
        //第四行：同步
        alert('end')
    */

    function fn(){
        console.log(1111);
        setTimeout(function(){
            console.log('定时器');  
        });
        console.log(2222);
        return 10;
    };
    console.log(fn());  // 1111  2222  10  定时器

/*同步操作
    1:会阻塞线程(性能低)
    2:按照顺序执行
    3:没有回调,但有返回值
*/

/*实际开发:
    1:取决于需求,大多数异步
    2:优先异步,性能高
*/
```

# http模块搭建服务器

## 1.1-http搭建服务器

```javascript
//1.导入模块
const http = require('http');

//2.创建服务器

/* 参数：回调函数
    req: request 客户端请求对象
    res: response 服务端响应对象
 */
let server = http.createServer((req, res) => {
    //获取客户端请求路径
    console.log(req.url);

});

//3.启动服务器:

/*第一个参数：端口号  一般1000-60000之间
第二个参数：ip地址  默认不写 就是本地ip 
第三个参数：成功回调
 */
server.listen(3000, () => {
    console.log('服务器开启成功');
});
```

## 1.2-响应客户端请求
```javascript
//1.导入模块
const http = require('http');
//2.创建服务器
/* 
req:请求对象 负责处理客户端请求
res:响应对象 负责响应客户端请求
*/
let server =  http.createServer((req,res)=>{
    //客户端每发送一个请求，这个函数都会执行一次（执行多次）
    console.log(req.url);
    //响应客户端数据
    res.end('hello world');
});

//3.启动服务器
server.listen(3000,()=>{
    console.log('服务器开启成功');
});
```

## 1.3-根据不同的需求响应不同的html页面
```Javascript
//1.导入模块
const http = require('http');
const fs = require('fs');

//2.创建服务器
let server =  http.createServer((req,res)=>{
    //需求：  如果请求路径是 / 返回首页html  如果是login返回登录html

    //1.获取客户端请求路径
    let url = req.url;

    //2.处理请求
    //一般如果响应的是文件，不需要设置响应头，直接返回二进制数据。浏览器会自动识别数据类型并且加载
    if(url == '/'){
        //响应首页
        fs.readFile('./data/index.html',(err,data)=>{
            if(err){
                throw err;
            }else{
                console.log(data);
                //3，将读取好的文件数据响应给客户端
                res.end(data);
            };
        });
    }else if(url == '/login'){
        //响应登录页
        fs.readFile('./data/login.html',(err,data)=>{
            if(err){
                throw err;
            }else{
                //3，将读取好的文件数据响应给客户端
                res.end(data);
            };
        });
    }else{
        res.end('404 not found');
    }
});

//3.开启服务器
server.listen(3000,()=>{
    console.log('服务器开启成功');
});
```

# nodejs中的路径问题
```javascript
//1.导入模块：路径模块
const path = require('path');
/*  前端相对路劲./  :相对于当前文件夹所在的文件路径
    nodejs相对路劲./   :相当于执行node命令所在路劲
    解决方案: 服务端不使用相对路劲,而是使用绝对路径
    在nodejs每一个文件夹都有两个默认的全局属性,这个属性获取绝对路径
        __dirname:获取当前js文件所在文件夹绝对路径
        __filname:获取当前js文件的节点路径
*/

console.log(__dirname);    console.log(__filename)
//一般使用__dirname来拼接读取文件的绝对路径
// let path = __dirname + './data/aaa/bbb.txt';

/*使用path模块join方法拼接文件路径于使用 + 号拼接的好处
    1:可以自动根据当前系统的路径分割符来识别路径
    2:如果路径分割符错误,path模块会自动帮我们修正
*/

let path = path.join(__dirname,'data','aaa','bbb.txt')

fs.readFile(path,'utf-8',(err,data)=>{
    if(err){
        throw err;
    }else{
        console.log(data);
    }
})
```


# WEB开发的三个特点

```txt
1.HTML中所有的外部资源路径都会变成网络请求
2.HTML中的路径只是一个资源标识符（由后台决定,接口文档）
3.HTML中所有的静态资源路径（css html 图片 音视频 文件）会与服务端文件路径保持一致
    __dirname + req.url = 服务端文件真实绝对路径
```

# nodejs接收get请求参数
```javascript
//1.导入模块
const http = require('http');
//导入url模块：解析get请求参数
const url = require('url');

//2.创建服务器
let server = http.createServer((req,res)=>{
    console.log(req.url);
    //了解即可：如果请求路径有中文，浏览器会自动进行URI编码
    console.log(decodeURI(req.url));
    // //使用原生的字符串分隔来获取参数
    // let arr = req.url.split('?');
    // console.log(arr);

    /* 1.使用url模块来解析get请求参数
    第一个参数：要解析的url（完整的url）
    第二个参数：布尔类型  true:得到是参数是对象（推荐）  false：得到参数是字符串
    返回值：对象（将完整url的每一个部分作为对象的属性）
    */
    let urlObjc = url.parse(req.url,true);
    console.log(urlObjc);
    //2.获取请求路径pathName
    let pathName = urlObjc.pathname;
    console.log(pathName);
    //3.获取get请求参数
    let query = urlObjc.query;
    console.log(query);

    //解析好的参数响应给客户端
    //服务器不能直接返回返回js对象（服务器具有跨平台性），需要转成json对象
    res.end(JSON.stringify(query));
});

//3.开启服务器
server.listen(3000,()=>{
    console.log('success');
});
```

# nodejs接收post请求参数
```javascript
//1.导入模块
const http = require('http');

//querystring：解析post参数
const querystring = require('querystring');

//2.创建服务器
let server = http.createServer((req,res)=>{
    //post请求不是放到url中，而是放到请求体 不能使用解析get请求参数来解析post参数

    /*post请求特点
    1.可以发送很大数据 ，需要很多次发送
    2.浏览器会将post数据切成很多份一点一点发送
    */
   //接收post请求参数流程
   //1.给req注册一个data事件，表示开始准备接收post数据
   var postData = '';
   req.on('data',(chuck)=>{
        //当客户端每发送一次数据流，这个函数会走一次，服务端需要主动将这些数据拼接起来
        //具体多少次，取决于客户端带宽
        postData += chuck;
   });

   //2.给req注册一个end事件。当客户端post数据全部发送完毕之后，这个方法会走一次
   req.on('end',()=>{
       console.log(postData);
        //3.使用querystring模块解析接收完成的post参数数据
       let postObjc = querystring.parse(postData);
       console.log(postObjc);
       //把解析好的post请求参数响应给客户端
       res.end(JSON.stringify(postObjc)); 
   });
    console.log(req.url);
});

//3.开启服务器
server.listen(3000,()=>{
    console.log('success');
});
```

# 服务端项目开发流程

     * 第三方: jquery bootstrap art-template
     * npm/cnpm使用步骤
        * 1:初始化: cnpm init -y
        * 2: 下载第三方模块: cnpm i jquery bootstrap art-template --save 

## WEB开发的本质

    * 请求: 客户端
    * 处理: 服务端(增删改查)
    * 响应: 服务端

## 服务端开发流程

    * 1: 搭建服务器

    * 2: 设置路由 (接口文档)
         * 请求
            * 路径
            * 请求方法
            * 请求参数
         * 响应: 响应的数据

    * 2.1 托管静态资源路由

    * 2.2 业务逻辑路由 (动态路由)
        * / : 显示首页
            * 服务端 重定向技术
        * /heroList : 获取首页英雄列表
            * 服务端 json数据
        * /heroAdd : 添加英雄
            * 服务端 接收post数据存入数据库
        * /heroInfo : 查看英雄
            & 服务端 接收get请求参数id,查询数据
        * /heroDelete : 删除英雄
            * 服务端 接收get请求参数id,删除英雄
                * 重定向技术刷新首页
        
        * 3: 处理路由
            * 1: 获取请求参数
            * 2: 处理: 增删改查
            * 3: 响应

## 客户端开发流程

    * 1.首页
        * 1: 发送ajax请求获取英雄列表,
        * 2: 使用模板引擎渲染服务端返回的数据
            * 1: 导入第三方
            * 2: 写模板
            * 3: 调用template的API来进行解析替换渲染
    
    * 2.添加英雄
        * ajax发送post表单数据

    * 3.查看英雄
        * 页面间传值,利用location的search属性

    * 4.删除英雄
        * 服务端重定向



# nodejs的commonjs规范
    * 1. nodejs中每一个js文件都是一个模块,他们遵循一种规范,commonJS规范
          a: 导入模快使用: `require()`
          b: 导出模块使用: `module.exports`
    * 2. 在nodejs中,每一个js文件都是一个独立的作用域,外部无法访问.要想访问需要使用'module.exports`导出

# nodejs中三种模式
    * 1. 内置模式: const fs = require('fs');
            * 无需下载,直接使用
    
    * 2. 第三方模块: const jquery = require('jquery');
            * 需要下载
    
    * 3. 自定义js模块: const mokuai = require('mokuai');
            * 自定义模块使用文件路径导入,可以使用相对路径 (nodejs底层为自动帮我们拼接)
    

# Express框架学习

    * express中的一切都是中间件

    * 1.托管静态资源t
       * 第一个参数：路径前缀
       * 第二个参数：要托管的文件夹绝对路径 
       * 'app.use('/resource', express.static(__dirname+'/resource'));'

    * 2. 中间件
        * express最核心的技术
            * a. 中间件的本质: 就是一个函数 (req,res,next){
                req: 请求对象  res: 响应对象  next:下一个中间件
            }

            * b. express 三种方式使用中间件
                * app.use(pathname,中间件)
                    * 如果pathname不写,所有的请求都会进入到这个中间件
                    * 如果写了pathname,以pathname路径开头的请求都会进入这个中间件
                * app.get(pathname,中间件): 请求路径为pathname的get请求都会进入到这个中间件
                * app.post(pathname,中间件): 请求路径为pathname的post请求都会进入到这个中间件
            
            * c. express处理网络请求流程
                * c.1: 依次从上往下匹配路径,如果可以匹配则进入中间件
                * c.2: 如果中间件中执行了next(),则继续往下匹配
                * c.3: 如果使用的中间件都无法匹配,express有一个默认兜底的中间件,会自动返回404

            * d. 可以给req和res动态添加属性


    * 3. 中间件使用步骤固定
        * 1. 进官网找文档
        * 2. 复制 + 粘贴
            * a. 安装 : 'cnpm i body-parser`
            * b. 使用 : `var bodyParser = require('body-parser')`  `app.use(bodyParser.urlencoded({ extended: false }))`

    * 4. 路由中间件
        * 使用路由中间件:
            * 1. 在router.js中: const express = require('express');
            * 2. 在router.js中: let app = express();
            * 3. 在router.js中: module.exports = app;
            * 4. 在index.js中使用路由中间件: app.use(require('./router.js))




# 第三方模块mongoose的简单介绍

### mongoose的官网: http://mongoosejs.com/
    * mongoose是基于mongodb封装在异步环境工作的对象模型工具
        * 异步环境: 提高性能
        * 对象模型工具: 一中全新的ORM思维去操作数据库
    * 安装: mongoose
        * npm/cnpm install/i mongoose
```javascript
//1,导入mongoose
const mongoose = require('mongoose');

//2.连接数据库  test：要连接的数据库名字    如果在则连接 不在则创建
mongoose.connect('mongodb://localhost:27017/test', {useNewUrlParser: true});

//3.创建表（mysql：table  mongodb：集合collection  mongoose：模型Model）
/* 第一个参数：表的名字  第二个参数：表中的数据结构 */
const Cat = mongoose.model('Cat', { name: String,age:Number });

//4.创建行（mysql：行row  mongodb:文档document  mongoose:Entyty实体）
const kitty = new Cat({ name: '张三',age:18 });

//操作数据库：增加数据到数据库
kitty.save().then(() => console.log('meow'));
```

## 数据库结构介绍
    * 数据库服务(mySql,mongodb: 一个数据库服务可以有很多个数据库)
        * 数据库: CQManager (一个数据库下可以有很多张表)
            * 表: cats(一个表中可以有很多个数据 mySql:表 table; mongodb: 集合 collections; mongoos: 模型 Model)
                * 记录: (一个记录可以有很多个列, mySql: 记录; mongodb: 文档document; mongoose: 实体 Entity)
                    * 数据:
## mongoose使用介绍
```javascript
// 导入模板
const mongoose = require('mongoose');

//链接数据库 class就是数据库的名称
mongoose.connect('mongodb://127.0.0.1:27017/class');

// 创建Model
/* 
第一个参数：模型名称（table表名）
第二个参数：模型中的数据结构(table表中数据结构)
返回值：model对象，负责table的增删改查操作
*/
let classModel = mongoose.model('students',{name:String,age:Number})
```

## mongoose增删改查的的基本操作
```javascript
/***********          增加操作            ***********/
/* 
第一个参数：要插入的数据  
    如果只插入一条数据：则传单个对象
    如果要插入多条数据：则传数组
第二参数：回调函数
    * err: 如果为null，表示插入成功。 否则插入失败
*/

student.create([{name:'张三',age:18},{name:'李四',age:20}],(err)=>{
    if(err){
        console.log('插入失败' + err);
    }else{
        console.log('插入成功');   
    };
});

/***********          删除操作            ***********/
/*删除满足条件的第一个数据
第一个参数：查询条件
第二个参数：回调函数 err：错误信息 
 */
student.deleteOne({name:'张三'},(err)=>{
    if(err){
        console.log('删除失败');
    }else{
        console.log('删除成功');  
    };
});

//删除满足条件所有数据
student.deleteMany({name:'张三'},(err)=>{
    if(err){
        console.log('删除失败');
    }else{
        console.log('删除成功');  
    };
});

/*******          修改数据               **********/
/*修改满足条件第一条数据
第一个参数：查询条件
第二个参数：要修改的数据
第三个参数：回调函数 
 */
student.updateOne({name:'李四'},{name:'王五',age:88},(err)=>{
    if(err){
        console.log('修改失败');
    }else{
        console.log('修改成功');
    }
});

//修改满足条件所有数据
student.updateMany({name:'李四'},{name:'王五',age:88},(err)=>{
    if(err){
        console.log('修改失败');
    }else{
        console.log('修改成功');
    }
});

/*******          查询数据               **********/
/*查询满足条件所有数据
第一个参数：查询条件
            如果省略,则返回所有数据
第二个参数：查询回调  err：错误信息  res：查询结果 
 */
student.find({name:'王五'},(err,res)=>{
    if(err){
        console.log('查询失败');   
    }else{
        console.log(res);       
    }
});

//只想查询满足条件第一个数据  student.findOne()
//通过数据唯一ID来查询  findByIdAndDelete findByIdAndCreate findByIdAndUpdate findByIdAndFind
student.findById('5c6aa1953753d11b9824b93c',(err,res)=>{
    if(err){
        console.log(err);
    }else{
        console.log(res);
    }
});
```

## 用crawler爬取网页数据
    * 安装: npm/cnpm install/i crawler --save
```javascript
// 导入文件
const Crawler = require('crawler');
// 创建抓包对象:抓取数据
var c = new Crawler({
    maxConnections : 10,
    // This will be called for each crawled page
    // 抓取数据之后会进入这个回调函数
    callback : function (error, res, done) {
        if(error){
            console.log(error);
        }else{
            // crawler会将抓取到的网页数据赋值给jQuery的$对象
            var $ = res.$;
            // $ is Cheerio by default
            //a lean implementation of core jQuery designed specifically for the server
            // 只需要使用jQuery语法解析网页即可
            // 抓取整个网页结构
            // console.log($("html").text());
            console.log($('#hero_list tr').length)
           $('#hero_list tr').each((index,ele)=>{ 
            // 获取英雄名字
            // console.log($(ele).find('td>.hero-icon>.class-icon>a').attr('title'))
            let heroName = $(ele).find('td>.hero-icon>.class-icon>a').attr('title');
            console.log(heroName);
            // 获取英雄技能
            // console.log($(ele).find('td>.joymewiki-tooltips>.joymewiki-tooltips-sub>.tt_skill>.havepassive>a').text())
            let heroSkill = $(ele).find('td>.joymewiki-tooltips>.joymewiki-tooltips-sub>.tt_skill>.havepassive>a').text();
            console.log(heroSkill)
            // 获取英雄图标
            // console.log($(ele).find('td>.hero-icon >a>img').attr('src'))
            let heroIcon = $(ele).find('td>.hero-icon >a>img').attr('src');
            console.log(heroIcon)
           })
        }
        done();
    }
});
// 开始抓包
// Queue just one URL, with default callback
c.queue('http://wiki.joyme.com/cq/%E5%89%91%E5%A3%AB');
```

## 用crawler爬取网页文件数据
```javascript
//导入原生文件模块
var fs = require('fs');
//1.导入模块
const Crawler = require('Crawler');
 
//2.创建抓包对象
var c = new Crawler({
    encoding:null,
    jQuery:false,// set false to suppress warning message.
    callback:function(err, res, done){
        if(err){
            console.error(err.stack);
        }else{
            //自动帮我们把抓取到的文件写入到服务器本地
            fs.createWriteStream(res.options.filename).write(res.body);
        }
        
        done();
    }
});
 
//3.开始抓包
c.queue({
    uri:"http://p6.qhimg.com/dr/72__/t019d01918ef9f45d4e.png",
    filename:"./resource/images/hero111.png"//本地文件路径
});
```

## 用crawler同时爬取文本和文件
```javascript
//1.导入模块
var Crawler = require("crawler");
//导入M层
const heroModel = require('./heroModel.js');
//2.创建抓包对象:抓取数据
var Datec = new Crawler({
    maxConnections: 10,
    // This will be called for each crawled page
    //抓取到数据之后会进入这个回调函数
    callback: function (error, res, done) {
        if (error) {
            console.log(error);
        } else {
            //crawler会将抓取到的网页数据赋值给jquery的$对象
            var $ = res.$;
            // $ is Cheerio by default
            //a lean implementation of core jQuery designed specifically for the server
            //只需要使用jquery语法解析网页即可
            // console.log($("html").text());
            //tbody有可能代码没写，浏览器自动生成，不要用tbody去解析页面
            console.log($('#hero_list tr').length);
            //使用jqeury语法解析页面，得到你想要抓取的数据
            //思路：将抓取的数据存入数组中，然后使用mongoose添加到数据库
            //1.声明空数组
            let heroArr = [];
            $('#hero_list tr').each((index, element) => {
                //1.获取英雄名字
                let heroName = $(element).find('td>.hero-icon .class-icon>a').attr('title');
                console.log(heroName);
                //2.获取英雄技能
                let heroSkill = $(element).find('.mwiki-hide .tt_skill .name a').text();
                console.log(heroSkill);
                //3.获取英雄图标
                let heroIcon = $(element).find('td>div>a>img').attr('src');
                console.log(heroIcon);
                //获取到图片路径之后，使用FileC抓取文件
                //图片名字格式改为：  /resource/images/英雄名称.png
                let imgPath = './resource/images/' + heroName + '.png'//拼接文件绝对路径
                Filec.queue({
                    uri: heroIcon,
                    filename: imgPath //本地文件路径
                });
                //2.将英雄对象添加到数组中
                heroArr.push({
                    name:heroName,
                    skill:heroSkill,
                    icon:'/resource/images/' + heroName + '.png'//url路径
                });
            });
            //3.将数组添加到数据库
            //抓取到的第一个元素是表头，需要去掉
            heroArr.shift();
            console.log(heroArr);
            heroModel.create(heroArr,(err)=>{
                if(err){
                    console.log('抓包入库失败');
                }else{
                    console.log('success');
                }
            });         
        }
        done();
    }
});
//3.开始抓包
// Queue just one URL, with default callback
Datec.queue('http://wiki.joyme.com/cq/%E5%89%91%E5%A3%AB');
//2.创建抓包对象:抓取文件
const fs = require('fs');
var Filec = new Crawler({
    encoding: null,
    jQuery: false, // set false to suppress warning message.
    callback: function (err, res, done) {
        if (err) {
            console.error(err.stack);
        } else {
            //自动帮我们把抓取到的文件写入到服务器本地
            fs.createWriteStream(res.options.filename).write(res.body);
        }
        done();
    }
});
/****** M层：负责处理数据（增删改查）  *******/
//1.导入模块
const mongoose = require('mongoose');
//2.连接数据库  CQManager:数据库名字
mongoose.connect('mongodb://127.0.0.1/CQManager',{useNewUrlParser:true});
//3.创建Model  heros：表的名字  第二个参数：表中存储的数据的结构
let heroModel = mongoose.model('heros',{name:String,skill:String,icon:String});
//4.导出M层:导出Model，因为mongoose中数据库增删改查由model负责
module.exports = heroModel;
```


