这是一个基于node的memcached连接库。

源码位置：https://github.com/elbart/node-memcache

client发送get等请求时候都会调用下面的方法：
```js
Client.prototype.query = function(query, type, callback) {
	this.callbacks.push({ type: type, fun: callback });
	this.sends++;
	this.conn.write(query + crlf);
};
```
请求数据的数序和返回数据的顺序是一致的，所以有数据回到client时（且符合一个memcached响应的规范）时，直接从`this.callbacks`取出第一个callback对数据进行处理即可。

## addHandler方法

每次addHandler时会判断tcp是否已经连接上，若连接上则运行已有的所有handler（dispatchHandles）。
