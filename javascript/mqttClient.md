引用 **paho-mqtt.js**插件

# 配置示例

```js
var options = {
	hostname: '10.58.100.35',  //必须参数
	port: 61623,               //必须参数
	path: '/',
	clientId: 'aaaaaaa',
	timeout: 5,
	keepAlive: 100,
	cleanSession: false,
	ssl: false,
	reconnect: true,
	user: {
		name: 'admin',
		password: '123456'
	}
};
```

# mqtt客户端初始化

```js
/**
 * @author: ares
 * @date: 2019/4/19 16:43
 * @description: mqtt客户端初始化
 * @param {object}options  配置
 * @constructor
 */
function MqttClientInit(options) {
	//实例化mqttclient
	if (typeof options.path == 'string' && options.path.length > 0) {
		return new Paho.Client(options.hostname, Number(options.port), options.path, options.clientId);
	} else {
		return new Paho.Client(options.hostname, Number(options.port), options.clientId);
	}
}
```

# mqtt客户端

```js
/**
 * @author: ares
 * @date: 2019/4/19 16:43
 * @description: mqtt客户端
 * @param {object}client   客户端实例
 * @param {object}options  配置
 * @constructor
 */
function MqttClientDeclaration(client, options) {
	this.connected = false;
	this.client = client;

	this.options = {
		invocationContext: {
			host: options.hostname,
			port: options.port,
			path: options.path,
			clientId: options.clientId,
		},
		timeout: Number(options.timeout) || 5,
		keepAliveInterval: Number(options.keepAlive) || 60,
		cleanSession: typeof options.cleanSession == 'boolean' ? options.cleanSession : false,
		useSSL: typeof options.ssl == 'boolean' ? options.ssl : false,
		reconnect: typeof options.reconnect == 'boolean' ? options.reconnect : true
	};
	this.options.onSuccess = typeof options.onConnect == 'function' ? options.onConnect : this.onConnect.bind(this);
	this.options.onFailure = typeof options.onFail == 'function' ? options.onFail : this.onFailure;
	if (options.user) {
		this.options.userName = options.user.name;
		this.options.password = options.user.password;
	}

	this.client.onConnectionLost = typeof options.onConnectionLost == 'function' ? options.onConnectionLost : this.onConnectionLost.bind(this);
	this.client.onMessageArrived = typeof options.onMessage == 'function' ? options.onMessage : this.onMessage;
}
```

# 扩展方法

```js
    /**
 * @author: ares
 * @date: 2019/4/21 10:05
 * @description: 建立连接
 */
MqttClientDeclaration.prototype.connect = function () {
	this.client.connect(this.options);
}
/**
 * @author: ares
 * @date: 2019/4/19 13:40
 * @description: 断开连接
 */
MqttClientDeclaration.prototype.disconnect = function () {
	this.client.disconnect();
}
/**
 * @author: ares
 * @date: 2019/4/19 14:14
 * @description: 连接成功
 * @param {object}event
 */
MqttClientDeclaration.prototype.onConnect = function (event) {
	this.connected = true;
	console.log('>> 连接成功 <<', event);
}
/**
 * @author: ares
 * @date: 2019/4/19 14:15
 * @description: 连接失败
 * @param {object}event
 */
MqttClientDeclaration.prototype.onFailure = function (event) {
	console.error('>> 连接失败 <<', event)
}
/**
 * @author: ares
 * @date: 2019/4/19 14:15
 * @description: 连接断开
 * @param {object}event
 */
MqttClientDeclaration.prototype.onConnectionLost = function (event) {
	this.connected = false;
	console.error('>> 连接断开 <<', event);
}
/**
 * @author: ares
 * @date: 2019/4/19 16:40
 * @description: 收到消息
 */
MqttClientDeclaration.prototype.onMessage = function () {
	console.error('>> 没有实现收到消息处理函数 <<');
}
/**
 * @author: ares
 * @date: 2019/4/19 13:36
 * @description: 发布消息
 * @param {string}topic        主题
 * @param {string}msg          消息体
 * @param {number}qos          质量等级0|1|2
 * @param {boolean}retain      保留消息
 */
MqttClientDeclaration.prototype.publish = function (topic, msg, qos, retain) {
	console.log('>> 发布 <<', topic)
	var message = new Paho.Message(msg);
	message.destinationName = topic;
	message.qos = /^([0-2])$/.test(qos) ? Number(qos) : 0;
	message.retained = !!retain;
	this.client.send(message);
}
/**
 * @author: ares
 * @date: 2019/4/19 13:38
 * @description: 订阅
 * @param {string}topic        主题
 * @param {number}qos          质量等级0|1|2
 */
MqttClientDeclaration.prototype.subscribe = function (topic, qos) {
	console.log('>> 订阅 <<', topic)
	this.client.subscribe(topic, {qos: /^([0-2])$/.test(qos) ? Number(qos) : 0});
}
/**
 * @author: ares
 * @date: 2019/4/19 13:38
 * @description:  取消订阅
 * @param {string}topic        主题
 */
MqttClientDeclaration.prototype.unsubscribe = function (topic) {
	console.log('>> 取消订阅 <<', topic)
	this.client.unsubscribe(topic, {
		onSuccess: this.unsubscribeSuccess,
		onFailure: this.unsubscribeFailure,
		invocationContext: {topic: topic}
	});
}
/**
 * @author: ares
 * @date: 2019/4/19 13:39
 * @description: 取消订阅成功
 * @param {object}event
 */
MqttClientDeclaration.prototype.unsubscribeSuccess = function (event) {
	console.log('>> 取消订阅主题"' + event.invocationContext.topic + '"成功 <<')
}
/**
 * @author: ares
 * @date: 2019/4/19 13:39
 * @description: 取消订阅失败
 * @param {object}event
 */
MqttClientDeclaration.prototype.unsubscribeFailure = function (event) {
	console.log('>> 取消订阅主题"' + event.invocationContext.topic + '"失败 <<')
}
```