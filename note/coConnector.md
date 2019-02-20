## coConeector
包含以下组件及服务
1. coSession
    1. sessionService
2. coServer
    1. Server 
        1. filterService
        2. handlerService
3. coPushScheduler
4. coConnection
### SessionService
+ @param {Object} sessions {sid:session}
+ @param {Object} uidMap {uid:sessions}
+ @param {Boolean} singleSession 一个用户是否只能绑定一个session

### Session
+ @param {Number} id ex:1
+ @param {String} frontendId ex:connector-server-1
+ @param {Number} uid
+ @param {Object} settings 用户设置的kv
+ @param {Object} __socket__ socket的引用
+ @param {Object} __sessionService__ SessionService实例
+ @param {Enum} __state__ [0]ST_INITED [1]ST_CLOSED

#### @method closed
做清理工作并通知客户端结束Session,关闭socket  
> duck typing socket,此实例需要实现 send & sendBatch 接口。

### FrontendSession
一个内部session在前端服务器中的傀儡
#### @method on
为FrontendSession和Session加上监听

### Server
调用分为两种情况
1. request.serverType == server.serverType invoke doHandle
2. request.serverType != server.serverType invoke doForward

+ @param {Object} app 上下文
+ @param {Object} globalFilterService
+ @param {Object} filterService
+ @param {Object} handlerService
+ @param {Array} crons
+ @param {Object} jobs
+ @param {Enum} state  
#### @method initFilter
加载Filter信息
#### @method initHandler
加载Handler信息
#### @method loadCronHandlers
加载 app/server/serverType/cron下面的信息
#### @method loadCrons
将config/cron.js下的时间起加到crons下
#### @method scheduleCrons 
用pomelo-scheduler初始化配置的任务
#### @method beforeFilter
调用filterService.beforeFilter过滤



### filterService
+ @param {Array} befores 前置filters
+ @param {Array} afters 后置filters 给server一个在响应request后做清除工作的机会
#### @method before
将filter对象加入before
#### @method after
将filter对象加入after
#### @method beforeFilter
invoke beforeFilter chain
#### @method afterFilter
invoke afterFilter chain
> 配置的filter可以使一个对象，也可以是一个函数。

### handlerService
+ @param {Context} app
+ @param {Object} handlerMap {serverType: Module}
#### @method loadHandlers
加载对应的handler到handlerMap中。
#### @method watchHandlers
监听文件变化，从而支持热更新
> use node fs.watch
#### @method getHandler
获取已加载handler,ex:`connector.entryHandler.entry`,此时获取的对象是entryHandler。
#### @method handle
调用handler下对应的方法
> 其实使用的next有3个参数，最后一个是opts 透传?

### msgRemote
前端服务器通过`msgRemote`将客户端请求发给后端服务器。提供`sys rpc`?
