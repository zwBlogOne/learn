#### promise



### koa

- 代码编写上避免了多层的嵌套异步函数调用 async await来解决异步
  - async  await 需要依赖于promise
- 更轻...  减少了内置的中间件  express.static处理静态资源的内置中间件  express.Router() 路由
- 启动步骤
  1. 引入Koa构造函数对象 ```const Koa = require('koa')```
  2. 创建服务器示例对象 ```const app = new Koa();```
  3. 配置中间件 ```app.use(做什么?)```
  4. 监听端口启动服务器 ```app.listen(8888);```
- 做什么? (函数参数说明)
  - context上下文对象: 该对象类似原生http中的 req + res
    - 该对象的req,res属性也存在，就是原生没有包装过的req,res
    - 简单说:  context 对象就是从请求到响应 过程中的一个描述对象
  - next函数:调用下一个中间件
- request(请求对象):  其中包含客户端请求的相关信息
- response(响应对象): 包含响应数据的具体操作



#### request常用属性

- ctx.request.url(ctx.url)
- ctx.request.method(ctx.method)
- ctx.request.headers(ctx.headers)



#### response常用属性

- ctx.response.set(ctx.set)  __函数:参数key,val__
- ctx.response.status(ctx.status)

- ctx.response.body(ctx.body)  



#### 小结

- 以上所有使用的属性,都可以简写 ctx.xxx
- 使用async await的应用场景,如果你出现了异步操作,使用其,  后一个中间件使用了async,前后都使用
- 三主角: __函数前面 async, 内部才能await,要想await能有用,就用promise包裹他__



#### 第三方中间件

- 处理请求体 __koa-bodyparser__
  - 非GET请求，比如说post请求 ，包括表单提交的form内的数据，都能轻松获取
  - ctx.request.body 获取form中的数据

- 处理路由 __koa-router__
  - 获取查询字符串 ctx.query
  - 获取/xxx/:id      ctx.params.id
  - ![52733885202](../../../%E6%A1%8C%E9%9D%A2/NodeJs/06-NodeJS%E8%BF%9B%E9%98%B6-%E7%AC%AC6%E5%A4%A9-%7B%20%E8%81%8A%E5%A4%A9%E5%AE%A4%E6%A1%88%E4%BE%8B%E3%80%81socket.io%20%7D/4-%E6%BA%90%E4%BB%A3%E7%A0%81/assets/1527338852020.png)
- koa-bodyparser是解析请求体数据的，koa-router中可以通过ctx.query||ctx.params获取url上的参数



- 处理静态资源 __koa-static__
- 渲染页面 __koa-art-template__
  - koa与视图通信的对象 __ctx.state__
- session中间件 __koa_session__
  - sign：true   会生成一个关于cookie数据保障不被修改的签名，如果数据改了，但是签名还是之前的状态，就说明数据不安全
  - app.keys 必须要，内部通过该值进行标识或者说计算
  - 操作session   __ctx.session.xxx__

#### 聊天室练习

### koa-socket

#### 核心思想socket-io

- 轮询ajax   __缺点:不停询问服务器，浪费性能__
- 服务器不关闭连接，一次响应，一直保持连接 __缺点:只有服务器向客户端不断输出__
- html5中出来了一个websocket   他是在原来http协议的基础上，去升级当前协议为websocket升级
  - 将原本  先有请求才有响应的机制，更改成了，服务端也可以主动发请求给客户端
  - HTTP 一问一答，  TCP协议，客户端与服务器建立连接以后，就可以自由的通信了
  - 缺点:__有兼容性问题__（IE11）
- socket.io交互方式可能通过websocket/轮询ajax/服务器响应流(保持连接)
  - 1. 服务器可主动发数据到客户端
    2. 客户端向客户端发数据通过服务器

#### 下载包

- 客户端：socket.io-client
- 服务器：koa-socket

#### 操作步骤

- 服务器广播   ```io.broadcast('事件名',{ 数据} );```
- 服务器向客户端||客户端向服务器  ``` socket.emit('事件名',{数据});```
- 客户端接收  ```socket.on('事件名',data=>{} )```
- 服务器接收  ```io.on('事件名',data=>{});```

#### 进阶学习

- 私聊
  - 1. 客户端告诉服务器to谁,及内容
    2. 服务器通过io.to(socket.id)找到目标客户端,再通过emit通信
- 群组聊天
  - 加入群组 ```ctx.socket.join(groupid);```
  - 向群里通信```ctx.socket.broadcast 
  - .to(groupId).emit('xxxx',data)
  - 后续接收还是对应on('xxxx')



#### 总结

- 第三方包的用途  koa-static 处理静态资源
- koa-router  query获取查询字符串  params 获取 /xxx/:id 中的id
- koa-session app.keys必须要写上作为其内部运算的标识
  - 两次setcookie其中一个是cookie标识
  - 一个是数据签名， 保证数据不被修改
- koa-art-template 按文档配置引擎
- koa-bodyparser 解析请求体   ctx.request.body获取数据
- socket.io
  - 客户端需要引入  socket.io-client
  - 服务器： koa-socket
  - 对象之间的关系和结构，都是写多就熟，也并不用很熟，看着文档，会来就行
  - 客户端端与服务器之间都是  on('xxx')  emit('xxx')去对应
    - join加入组
    - to 私聊
    - 事件名,数据    数据可以是对象，如果仅仅是字符串，  ctx.data拿就可以了，是对象ctx.data.xxxx







