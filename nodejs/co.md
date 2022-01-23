# 作用

基于 **Generator** 和 **Promise** 的流程控制, 让你愉快的书写非阻塞代码.

# co(fn*).then( val => )

```js
co(function* () {
	return yield Promise.resolve(true);
}).then(function (val) {
	console.log(val);
}, function (err) {
	console.error(err.stack);
});

// 数组方式
co(function* () {
	var res = yield [
		Promise.resolve(1),
		Promise.resolve(2),
		Promise.resolve(3),
	];
	console.log(res); // => [1, 2, 3]
}).catch(onerror);

// 对象方式
co(function* () {
	var res = yield {
		1: Promise.resolve(1),
		2: Promise.resolve(2),
	};
	console.log(res); // => { 1: 1, 2: 2 }
}).catch(onerror);
```

# var fn = co.wrap(fn*)

```js
var fn = co.wrap(function* (val) {
    return yield Promise.resolve(val);
});
 
fn(true).then(function (val) {
 
});
```