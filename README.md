# express

- 文件操作路径和模块路径
  + 文件操作路径
    ```
    //===> 在文件操作的相对路径中，
    //   ./data/a.txt  相对于当前目录
    //   data/a.txt    相对于当前目录
    //   /data/a.txt   绝对路径，当前文件模块所处磁盘根目录
    //   c:/xx/xx...   绝对路径

    // fs.readFile('/data/a.txt', function (err, data) {
    //   if (err) {
    //     console.log(err)
    //     return console.log('读取失败')
    //   }
    //   console.log(data.toString())
    // })

    ```

  + 模块操作路径
    ```
    // 这里如果忽略了. , 则也是磁盘根目录
    require('/data/foo.js')

    //相对路径
    require('./data/foo.js)

    //模块加载的路径中的相对路径不能省略 ./
    ```

- Express
  + 原生的http在某些方面表现不足以应对我们的开发需求，所以我们就需要使用框架来加快我们的开发效率。
  + 框架的目的就是提高效率，让我们的代码更加高度统一

- Express起步
 + 安装
    ```
    npm install --save express
    ```
  + 发标题
    ```
    const express = require('express')
    const app = express()

    app.get('/', (req,res) => res.send('hello world'))
    app.listen(3000, () => console.log('Express app listenling on port 3000!'))
    ```
  + 基本路由 路由其实就是一张表，这个表里面有具体的映射关系

    - get
    ```
    # 当你以GET方法请求/的时候，执行对应的处理函数

    app.get('/',function(req,res){
      res.end('hello world')
    })
    ```
    - post
    ```
    # 当你以post方法请求/的时候，执行对应的处理函数

    app.get('/',function(req,res){
      res.end('Got a post request')
    })
    ```
  + 静态服务
    ```
    # 直接访问里面的public的文件
    app.use(express.static('public'))
    app.use(express.static('files'))
    
    # 必须/static/public资源
    app.use('/static',express.static('public'))
    
    
    app.use('/static', express.static(path.join(__dirname, 'public')))
    ```


- 修改完代码自动重启
  + 可以使用一个第三方命名工具：nodemon来帮我们解决频繁修改代码重启服务器问题
  + nodemon是一个基于Node.js开发的一个第三放命令行工具，我们使用的时候需要独立安装
  ```
  # 在任意目录执行该命令都可以
  # 也就是说，所有需要，--global来安装的包都可以在任意目录执行
  npm install --global nodemon
  ```
  + 安装完毕之后，使用：
  ```
  node app.js

  # 安装完毕之后，使用：
  nodemon app.js
  ```
  + 只要通过nodemon app.js启动的服务，则它会监视你的文件变化，当文件发生变化的时候，自动帮你重启服务器

- 在Express中配置使用art-template模板引擎
  + 安装
  ```
  npm install --save art-template
  npm install --save express-art-template
  ```

  + 配置
  ```
  app.engine('art', require('express-art-template'))
  ```

  + 使用
  ```
  app.get('/', function(req,res){
    //express默认会去项目中的views目录找index.html
    res.render('index.html', {
      title: 'hello world'
    })
  })
  ```
  + 如果希望修改默认views视图渲染存储目录，可以：
  ```
  //注意第一个参数views千万不要传错
  app.set('views' , 目录路径)
  ```

- 在Express获取表单POST请求体数据
  + 在Express中没有内置获取表单POST请求体的API，这里我们需要使用一个第三方的包body-parse(express里面的middleware中间件)
  + 安装
  ```
  npm install --save body-parser
  ```
  + 配置
  ```
  var express = require('express')

  // 0.引包
  var bodyParser = require('body-parser')

  var app = express()

  //配置body-parser
  //只要加入这个配置，则在req请求对象上会多出来一个属性：body
  //也就是说你就可以直接通过req.body来获取表单POST请求体数据了


   // parse application/x-www-form-urlencoded
   app.use(bodyParser.urlencoded({ extended: false }))

   // parse application/json
   app.use(bodyParser.json())

   app.use(function(req,res){
    res.setHeader('Content-Type': 'text/plain')
    res.write('you posted: \n')

    //可以通过req.body来获取表单请求体数据
    res.end(JSON.stringify(req.body, null , 2))
  })
  ```

- 怎么选中长度不同的单词
    ```
      <li>me1</li>
      <li>hello1</li>
      <li>default1</li>
      <li>mistakes1</li>
      <li>you1</li>
    ```
  + mac上是shift + option + 右方向键 
  + 注意不是command + shift + 右方向键
- 怎么选中对不齐的标签里面的长度不同的单词
   ```
     <li>me1</li>
    <li>hello1</li>
  <li>default1</li>
     <li>mistakes1</li>
  <li>you1</li>
   ```
   + 第一步是选中<li>这个标签，
   + ====> command + d 
   + ====> 松开command + d,按右方向键
   + ====> 再按住shift + option + 右方向键

- callback是不是相当于函数的自调用
  + 并不是。函数也是一种数据类型，既可以当做参数进行传递，也可以当做方法的返回值。

- 异步编程
  + 不成立的情况：
    ```
      // function add(x,y){
      //   console.log(1) => 1
      //   setTimeout(function(){
      //     console.log(2) => 2
      //     var ret = x + y
      //     return ret =>这里不可能拿到这个返回值
      //   },1000)
      //   console.log(3) => 3
            //到这里执行就结束了，不会等到前面的定时器，所以直接就返回了默认值undefined
      // }
      // console.log(add(10,20))  => undefined

      // 执行结果：
      // 1
      // 3
      // undefined
      // 2

    ``` 
  + 不成立的情况：
    ```
    function add(x,y){
       var ret
       console.log(1)
      setTimeout(function(){
        console.log(2)
        return x+y
       },1000)
      console.log(3)
     return ret
    }
    console.log(add(10,20))
    ```
- 回调函数
  ```
        function add(x,y,callback){

        //callback就是回调函数
        //var x = 10
        //var y = 20
        //var callback = function(ret){console.log(ret)}


        console.log(1)
        setTimeout(function(){
          var ret = x + y
          callback(ret)  ==> 获取回调函数中的返回值

        },1000)
      }

      add(10,20,function(ret){
         //现在拿到这个结果可以做任何操作 
      })
  ```
+ 基于原生XMLHttpRequest封装get方法
   ```
   function get(url, callback){
      var oReq = new XMLHttpRequest()
      oReq.onload = function(){
        callback(oReq.responseText)
      }
      oReq.open('get', url, true)
      oReq.send()
    }

    get('data.json', function(data){
      console.log(data)
    })
   ```
+ javascript天生不支持模块化
  + require
  + exports
  + Node.js才有的
- 在浏览器中也可以像在Node中的模块一样来进行编程
  + require.js 第三方库  AMD
  + sea.js     第三方库  CMD
+ 在Node这个环境中对JavaScript进行了特殊的模块化支持CommonJS
  + 无论是CommonJS,AMD,CMD,UMD,EcmaScript 6 Modules官方规范，都是为了解决js的模块化问题
  + 开发人员为了在不同的环境下使用不同的Javascript模块化解决方案，就弄了很多种第三方库
  + 在2015年时候，EcmaScript就发布了官方的标准，这样子语言天生就支持了
  + 虽然标准已经发布了，但是很多javascript运行时还不支持，Node也是只在8.5的版本后才对Ecmascript 6 module进行了支持
  + 学习vue的时候会去学习
+ 目前前端都是使用新技术，然后利用编译器工具打包可以在低版本的浏览器运行
  + 使用新技术的目的是为了提高效率，增加可维护性
  + less编译器 ==> css
  + Ecmascropt 6 ===> 编译器 ====> ECMASCRIPT 5
+ package.json和package-lock.json
  + npm5以前是不会有package-lock.json这个文件的
  + npm5以后才加入了这个文件
  + 当你安装或者更新package-lock.json这个文件
    + npm5以后的版本安装包不需要加--save参数
    + 当你安装包的时候会自动创建或者更新package-lock.json这个文件
    + package-lock.json这个文件会保存node_modules中所有包的信息（版本，下载地址）
    + 这样的话重新npm install的时候速度就会提升
  + package-lock.json这个文件的另外一个作用是锁定版本号，防止自动升级

+ 在Express中怎么判断页面不存在的情况
  + express对没有设定的请求路径，默认会返回 can not get XXX
  + 如果想要定制这个404，需要通过中间件来配置
  + 只需要在自己的路由之后增加一个
      ```
      app.use(function(req,res){
        //所有未处理的请求路径都会跑到这里
        //404
      })
      ```
- 回调函数
   + 异步编程
   + 如果需要得到一个函数内部异步操作的结果，这个时候必须通过回调函数来获取
   + 在调用的位置传递一个函数进来
   + 在封装的函数内部调用传递进来的函数
- find,findIndex,forEach
   + 都是数组的遍历方法，都是对函数作为参数的一种运用

##  ES6-babel
   + 第一：在项目根目录下面创建一个`.babelrc`
   + 进入这个文件写入以下内容：
      ```json
        {
          "preset":[

          ]
        }
      ```
  + 第二：安装专门的转码规则
    ```bash
      # ES2015转码规则
      npm install --save-dev babel-preset-es2015

      # react转码规则
      npm install --save-dev babel-preset-react

    ```
      + --save和--save-dev
        - 通过`--save`参数安装的包，是将依赖项目保存到package.json文件中的dependencies选项中
        -  通过`--save-dev`参数安装的包，是将依赖项保存到package.json文件中的devDependencies选项中
        - 无论是`--save`或是`--save-dev`安装的包，通过执行 `npm install`都会将对应的依赖包安装进来。
        - 但是在开发阶段会有一些仅仅用来辅助开发的一些第三方包或是工具，然后最终上线运行（到了生产环境），这些开发依赖项就不再需要了，就可以通过`npm install --production`命令仅仅安装`dependencies`中的依赖项，
  + 第三：将`.babelrc`文件中修改为一下内容：
      ```json
        {
          "presets":[
            "es2015"
          ]
        }

      ```
  + 第四：
    - babel-cli : 命令行转码
    - babel-node: babel-cli工具自带一个babel-node命令，提供一个支持ES6的REPL环境
    - babel-register：实时转码，所以只适合在开发环境中使用
    - babel-core：如果某些代码需要调用Babel的API进行转码，就要使用babel-core模块
  + babel-cli：一种方式是全局安装：`npm install -g babel-cli(可以通过`npm root -g`查看全局包的安装目录)`
    只要全局安装了`babel-cli`，则会在命令行中多出一个命令：`babel`
## Express
  + hello world
     + 1.先引入包
       ```
       const express = require('express')
       ```
      + 2.调用express()方法，得到一个app实例接口对象
        ```
        const app = express()
        ```
      + 3.通过app设置对应的路径对应的请求处理函数
        ```
          app.get('/login',(req,res) => {
            //end用力结束响应的同时发送响应数据
            res.end('hello login')
          })
        ```
      + 4.开启监听，启动服务器
       ```
        app.listen(3000,() => {
          console.log('服务已启动，请访问：http://127.0.0.1:3000/')
        })
       ```
  + 基本路由
     + 根据不同的请求路径分发到具体的请求处理函数
  + 处理静态资源
  + 中间件
        + 中间件是用来处理http请求的一个具体的环节（可能是要执行某个具体的处理函数）
        + 中间件一般是通过修改req或者res对象来为后续的处理提供遍历的使用
      + 中间件分类
        + `use(function(req,res,next){})`不关心请求方法和请求路径，没有路由规则，任何请求都会进入该中间件
        + `use('请求路径' , function(req,res,next){})`不关心请求方法，只关心请求路径的中间件
        + `get('请求路径', function(req,res,next){})`具体路由规则中间件
        + `post('请求路径', function(req,res,next){})`
      + 中间件的作用：
        + 在http中，没有请求就没有响应，服务端不可能主动个客户端发请求，就是一问一答的形式
        + 对于一次请求来说，只能响应一次，如果发送了多次响应，则只有第一次生效
        + 假如有请求进入了中间件，调用的next会执行下一个能匹配的中间件
      + 中间件处理404
        + 一定是加在中间件的最后一个，因为404是所有的中间件的网址配置不到的页面，也就是在`app.listen`前面
        + 添加的代码为
          ```
            app.use((req,res,next) => {
              res.end('404')
            })
          ```
