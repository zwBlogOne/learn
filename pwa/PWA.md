#### PWA （渐进式/Web/application）

* __只能在http://localhost或者https下运行__

* 持续关注PWA，积极研究其中应用场景，谷歌公司将现有的技术，结合起来推广的一整套的方案

- 慢慢收服你（用户）
- 离线浏览web应用，生成桌面应用，顶部通知（页面都可以不存在），预缓存（在你页面没有启动以前，请求资源保存到浏览器）（真正访问的时候，非常快，请求本地），骨架屏，App shell(利用缓存机制保存css+html+js等等)， 分享

- 以上所有讲的东西，请大家把自己当产品经理来学习，以后再新技术上，chrome推出PWA但是一直没火起来。
- 手机端 chrome 55以上才支持这些所有
  - IOS11 safari 支持
- 英文看支持率 caniuse     中文就是lavas官网可以看到



#### service worker

* 本质就是浏览器开启的一个线程（类似webworker），该线程可以拦截请求，发请求（类似node服务器）



#### SW生命周期

![sw-lifecycle](C:\Users\heima\Desktop\全栈4期\22-VueJS-第14天-自定义指令、PWA、服务端渲染\4-源代码\assets\sw-lifecycle.png)

- **install**：Service Worker 安装成功后被触发的事件，在事件处理函数中可以添加需要缓存的文件（预先缓存资源）
- **activate**：当 Service Worker 安装完成后并进入激活状态，会触发 activate 事件。通过监听 activate 事件你可以做一些预处理，如对旧版本的更新、对无用缓存的清理等。（检查更新资源）
- **message**：Service Worker 运行于独立 context 中，无法直接访问当前页面主线程的 DOM 等信息，但是通过 postMessage API，可以实现他们之间的消息传递，这样主线程就可以接受 Service Worker 的指令操作 DOM。（浏览器与serviceworker通信的事件）



#### 数据存储

* H5中的CacheAPI

* 使用CacheAPI获取指定缓存的内容

  - ```js
    
    // 创建响应对象 开始
    var debug = {hello: "world"};
    var blob = new Blob([JSON.stringify(debug, null, 2)],
      {type : 'application/json'});
    var init = { "status" : 200 , "statusText" : "SuperSmashingGreat!" };
    var myResponse = new Response(blob,init);
    // 创建响应对象 结束
    caches.open('key1').then(function(cachedRequests) { 
        cachedRequests.put('/def113',myResponse)
      });
    ```



  #### DB比较

![1530352193027](C:\Users\heima\Desktop\全栈4期\22-VueJS-第14天-自定义指令、PWA、服务端渲染\4-源代码\assets\1530352193027.png)

#### 缓存precaching

* workbox-sw 一个工具库
  * 预缓存静态资源```workbox.precaching.precacheAndRoute(['xxx'])```
  * 动态缓存动态资源```workbox.routing.registerRoute(fn)```
  * 缓存算法策略```workbox.strategies```
    * 缓存优先 ```cacheFirst()```
    * 网络优先 ```networkFirst()```
    * 仅缓存```networkOnly()```
    * 仅网络```networkOnly()```
    * Stale While Revalidate(缓存优先并更新缓存) ```staleWhileRevalidate()```

#### 更新问题

* 在service worker 中会缓存 service worker的具体行为
* 让register-worker 每次请求新的service worker  就可以更改 缓存内容与缓存策略
* ![1537279494286](C:\Users\heima\Desktop\全栈4期\22-VueJS-第14天-自定义指令、PWA、服务端渲染\4-源代码\assets\1537279494286.png)



#### 兴趣补充

* 顶部通知 notification对象
* 分享 navigator.share





