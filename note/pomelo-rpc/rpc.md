## Client
以`mqtt-connection`封装为例

### mqtt-mailbox
+ @param {Number} curId
+ @param {String} id ex:test-server-1
+ @param {String} host
+ @param {Number} port
+ @param {Object} requests
+ @param {Object} timeout   
+ @param {Array} queue
+ @param {Boolean} bufferMsg
+ @param {Number} keepalive
+ @param {Number} interval
+ @param {Number} timeoutValue
+ @param {Number} keepaliveTimer
+ @param {Number} lastPing
+ @param {Number} lastPong
+ @param {Boolean} connected 是否连接
+ @param {Boolean} closed 是否关闭
+ @param {Object} opts
+ @param {String} serverId 