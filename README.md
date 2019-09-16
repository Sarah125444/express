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



