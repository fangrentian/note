# 给 Function扩展原型方法

```js
var functionPrototype = {
	/**
	 * @author: ares
	 * @date: 2019/6/25 16:59
	 * @description: 前置函数
	 * @param {function} fn
	 * @returns {Function}
	 */
	before: function (fn) {
		var self = this;
		return function () {
			fn.apply(this, arguments);
			return self.apply(this, arguments);
		}
	},
	/**
	 * @author: ares
	 * @date: 2019/6/25 17:03
	 * @description: 后置函数
	 * @param {function} fn
	 * @returns {Function}
	 */
	after: function (fn) {
		var self = this;
		return function () {
			var result = self.apply(this, arguments);
			fn.apply(this, arguments);
			return result;
		}
	},
	/**
	 * @author: ares
	 * @date: 2019/6/25 17:05
	 * @description: 绑定this
	 * @returns {Function}
	 */
	bind: function () {
		var self = this,
			context = [].shift.call(arguments),
			args = [].slice.call(arguments);
		return function () {
			return self.apply(context, [].concat.call(args, [].slice.call(arguments)));
		}
	}
};
/**
 * @author: ares
 * @date: 2019/6/25 10:16
 * @description: Function.prototype 扩展before方法
 */
typeof Function.prototype.before === 'function' || (Function.prototype.before = functionPrototype.before);
/**
 * @author: ares
 * @date: 2019/6/25 10:17
 * @description: Function.prototype 扩展after方法
 */
typeof Function.prototype.after === 'function' || (Function.prototype.after = functionPrototype.after);
/**
 * @author: ares
 * @date: 2019/6/25 10:51
 * @description: Function.prototype 扩展bind方法
 */
typeof Function.prototype.bind === 'function' || (Function.prototype.bind = functionPrototype.bind);
```

# 给 Object扩展静态方法

```js
var objectMethod = {
	/**
	 * @author: ares
	 * @date: 2019/6/25 15:57
	 * @description: 将所有可枚举属性的值从一个或多个源对象复制到目标对象。它将返回目标对象
	 * @param {object} target   目标对象
	 * @returns {object}
	 */
	assign: function (target) {
		if (Object.prototype.toString.call(target) !== '[object Object]') throw new TypeError('first argument is not an object!');
		for (var i = 1; i < arguments.length; i++) {
			var source = arguments[i];
			if (Object.prototype.toString.call(source) !== '[object Object]') continue;
			for (var key in source) {
				if (Object.prototype.hasOwnProperty.call(source, key)) target[key] = source[key];
			}
		}
		return target;
	}
};
/**
 * @author: ares
 * @date: 2019/6/25 10:00
 * @description: Object.prototype 扩展assign方法
 */
typeof Object.assign === 'function' || (Object.assign = objectMethod.assign);
```

# 给Array扩展原型方法

```js
var arrayPrototype = {
	/**
	 * @author: ares
	 * @date: 2019/6/26 15:06
	 * @description: 浅复制数组的一部分到同一数组中的另一个位置，并返回它，不会改变原数组的长度
	 * @param {number} target         复制序列到该位置, 如果是负数，target 将从末尾开始计算
	 * @param {number} start          开始复制元素的起始位置。如果是负数，start 将从末尾开始计算。如果 start 被忽略，copyWithin 将会从0开始复制
	 * @param {number} end            开始复制元素的结束位置。copyWithin 将会拷贝到该位置，但不包括 end 这个位置的元素。如果是负数， end 将从末尾开始计算
	 *                                 如果 end 被忽略，copyWithin 方法将会一直复制至数组结尾
	 * @returns {*}
	 */
	copyWithin: function (target, start, end) {
		if (this == null) throw new TypeError('this is null or not defined');
		var _obj = Object(this),
			len = _obj.length >>> 0,
			relativeTarget = target >> 0,
			to = relativeTarget < 0 ? Math.max(len + relativeTarget, 0) : Math.min(relativeTarget, len),

			relativeStart = start >> 0,
			from = relativeStart < 0 ? Math.max(len + relativeStart, 0) : Math.min(relativeStart, len),

			end = arguments[2],
			relativeEnd = end === undefined ? len : end >> 0,
			final = relativeEnd < 0 ? Math.max(len + relativeEnd, 0) : Math.min(relativeEnd, len),

			count = Math.min(final - from, len - to),
			direction = 1;

		if (from < to && to < (from + count)) {
			direction = -1;
			from += count - 1;
			to += count - 1;
		}
		while (count > 0) {
			if (from in _obj) {
				_obj[to] = _obj[from];
			} else {
				delete _obj[to];
			}
			from += direction;
			to += direction;
			count--;
		}
		return _obj;
	},
	/**
	 * @author: ares
	 * @date: 2019/6/26 15:14
	 * @description: 测试一个数组内的所有元素是否都能通过某个指定函数的测试
	 * @param {function} callbackfn
	 * @param {*} thisArg
	 * @returns {boolean}
	 */
	every: function (callbackfn, thisArg) {
		if (this == null) throw new TypeError('this is null or not defined');
		if (typeof callbackfn !== 'function') throw new TypeError();
		var context = arguments.length > 1 ? thisArg : this,
			index = 0,
			_obj = Object(this),
			len = _obj.length >>> 0;
		while (index < len) {
			var item;
			if (index in _obj) {
				item = _obj[index];
				var testResult = callbackfn.call(context, item, index, _obj);
				if (!testResult) return false;
			}
			index++;
		}
		return true;
	},
	/**
	 * @author: ares
	 * @date: 2019/6/26 15:16
	 * @description: 用一个固定值填充一个数组中从起始索引到终止索引内的全部元素
	 * @param {*} value           用来填充数组元素的值
	 * @param {number} start      起始索引，默认值为0
	 * @param {number} end        终止索引，默认值为 this.length
	 * @returns {*}
	 */
	fill: function (value, start, end) {
		if (this == null) throw new TypeError('this is null or not defined');
		var _obj = Object(this),
			len = _obj.length >>> 0,
			start = arguments[1],
			relativeStart = start >> 0,
			incept = relativeStart < 0 ? Math.max(len + relativeStart, 0) : Math.min(relativeStart, len),

			end = arguments[2],
			relativeEnd = end === undefined ? len : end >> 0,

			final = relativeEnd < 0 ? Math.max(len + relativeEnd, 0) : Math.min(relativeEnd, len);
		while (incept < final) {
			_obj[incept] = value;
			incept++;
		}
		return _obj;
	},
	/**
	 * @author: ares
	 * @date: 2019/6/26 15:19
	 * @description: 创建一个新数组, 其包含通过所提供函数实现的测试的所有元素
	 * @param {function} callbackfn
	 * @param {*} thisArg
	 * @returns {Array}
	 */
	filter: function (callbackfn, thisArg) {
		if (this == null) throw new TypeError('this is null or not defined');
		if (typeof callbackfn !== 'function') throw new TypeError();
		var context = arguments.length > 1 ? thisArg : this,
			_obj = Object(this),
			len = _obj.length >>> 0,
			res = new Array(len),
			resIdx = 0,
			idx = -1;
		while (++idx !== len) {
			if (idx in _obj) {
				if (callbackfn.call(context, _obj[idx], idx, _obj)) res[resIdx++] = _obj[idx];
			}
		}
		res.length = resIdx;
		return res;
	},
	/**
	 * @author: ares
	 * @date: 2019/6/26 15:20
	 * @description: 返回数组中满足提供的测试函数的第一个元素的值。否则返回 undefined
	 * @param {function} callbackfn
	 * @param {*} thisArg
	 * @returns {*}
	 */
	find: function (callbackfn, thisArg) {
		if (this == null) throw new TypeError('"this" is null or not defined');
		if (typeof callbackfn !== 'function') throw new TypeError();
		var context = arguments.length > 1 ? thisArg : this,
			_obj = Object(this),
			len = _obj.length >>> 0,
			index = 0;
		while (index < len) {
			var item = _obj[index];
			if (callbackfn.call(context, item, index, _obj)) return item;
			index++;
		}
		return undefined;
	},
	/**
	 * @author: ares
	 * @date: 2019/6/26 15:21
	 * @description: 返回数组中满足提供的测试函数的第一个元素的索引。否则返回-1
	 * @param {function} callbackfn
	 * @param {*} thisArg
	 * @returns {number}
	 */
	findIndex: function (callbackfn, thisArg) {
		if (this == null) throw new TypeError('this is null or not defined');
		if (typeof callbackfn !== 'function') throw new TypeError();
		var context = arguments.length > 1 ? thisArg : this,
			_obj = Object(this),
			len = _obj.length >>> 0,
			index = 0;
		while (index < len) {
			var item = _obj[index];
			if (callbackfn.call(context, item, index, _obj)) return index;
			index++;
		}
		return -1;
	},
	/**
	 * @author: ares
	 * @date: 2019/6/26 15:22
	 * @description: 对数组的每个元素执行一次提供的函数
	 * @param {function} callbackfn
	 * @param {*} thisArg
	 */
	forEach: function (callbackfn, thisArg) {
		if (this == null) throw new TypeError('this is null or not defined');
		if (typeof callbackfn !== 'function') throw new TypeError();
		var context = arguments.length > 1 ? thisArg : this,
			_obj = Object(this),
			len = _obj.length >>> 0,
			index = 0;
		while (index < len) {
			var item;
			if (index in _obj) {
				item = _obj[index];
				callbackfn.call(context, item, index, _obj);
			}
			index++;
		}
	},
	/**
	 * @author: ares
	 * @date: 2019/6/26 15:23
	 * @description: 判断一个数组是否包含一个指定的值，根据情况，如果包含则返回 true，否则返回false。
	 * @param {*} valueToFind        需要查找的元素值
	 * @param {number} fromIndex     可选参数,从fromIndex 索引处开始查找 valueToFind。
	 * @returns {boolean}
	 */
	includes: function (valueToFind, fromIndex) {
		if (this == null) throw new TypeError('this is null or not defined');
		var _obj = Object(this),
			len = _obj.length >>> 0,
			_idx = fromIndex | 0,
			index = Math.max(_idx >= 0 ? _idx : len - Math.abs(_idx), 0);
		if (len === 0) return false;

		function sameValueZero(x, y) {
			return x === y || (typeof x === 'number' && typeof y === 'number' && isNaN(x) && isNaN(y));
		}

		while (index < len) {
			if (sameValueZero(_obj[index], valueToFind)) return true;
			index++;
		}
		return false;
	},
	/**
	 * @author: ares
	 * @date: 2019/6/26 15:25
	 * @description: 方法返回在数组中可以找到一个给定元素的第一个索引，如果不存在，则返回-1
	 * @param {*} member             要查找的元素
	 * @param {number} fromIndex     开始查找的位置
	 */
	indexOf: (function (Object, max, min) {
		return function indexOf(member, fromIndex) {
			if (this == null) throw new TypeError('this is null or not defined');
			var _obj = Object(this),
				len = _obj.length >>> 0,
				index = min(fromIndex | 0, len);
			if (index < 0) {
				index = max(0, len + index);
			} else if (index >= len) {
				return -1;
			}
			if (member === void 0) {
				for (; index !== len; ++index) if (_obj[index] === void 0 && index in _obj) return index; // undefined
			} else if (member !== member) {
				for (; index !== len; ++index) if (_obj[index] !== _obj[index]) return index; // NaN
			} else for (; index !== len; ++index) if (_obj[index] === member) return index; // all else

			return -1;
		};
	})(Object, Math.max, Math.min),
	/**
	 * @author: ares
	 * @date: 2019/6/26 15:28
	 * @description: 返回指定元素（也即有效的 JavaScript 值或变量）在数组中的最后一个的索引，如果不存在则返回 -1。从数组的后面向前查找，从 fromIndex 处开始
	 * @param {*} member                被查找的元素
	 * @param {number} fromIndex        可选参数,从此位置开始逆向查找
	 * @returns {*}
	 */
	lastIndexOf: function (member, fromIndex) {
		if (this == null) throw new TypeError('this is null or not defined');
		var _obj = Object(this),
			len = _obj.length >>> 0,
			_idx = len - 1,
			index;
		if (len === 0) return -1;
		if (arguments.length > 1) {
			_idx = Number(arguments[1]);
			if (_idx != _idx) _idx = 0;
		} else if (_idx != 0 && _idx != (1 / 0) && _idx != -(1 / 0)) {
			_idx = (_idx > 0 || -1) * Math.floor(Math.abs(_idx));
		}
		for (index = _idx >= 0 ? Math.min(_idx, len - 1) : len - Math.abs(_idx); index >= 0; index--) {
			if (index in _obj && _obj[index] === member) return index;
		}
		return -1;
	},
	/**
	 * @author: ares
	 * @date: 2019/6/26 15:29
	 * @description: 创建一个新数组，其结果是该数组中的每个元素都调用一个提供的函数后返回的结果
	 * @param {function} callbackfn
	 * @param {*} thisArg
	 * @returns {Array}
	 */
	map: function (callbackfn, thisArg) {
		if (this == null) throw new TypeError('this is null or not defined');
		if (typeof callbackfn !== 'function') throw new TypeError();
		var context = arguments.length > 1 ? thisArg : this,
			_obj = Object(this),
			len = _obj.length >>> 0,
			res = new Array(len),
			index = 0;
		while (index < len) {
			var kValue, mappedValue;
			if (index in _obj) {
				kValue = _obj[index];
				mappedValue = callbackfn.call(context, item, index, _obj);
				res[index] = mappedValue;
			}
			index++;
		}
		return res;
	},
	/**
	 * @author: ares
	 * @date: 2019/6/26 15:30
	 * @description: 对数组中的每个元素执行一个由您提供的reducer函数(升序执行)，将其结果汇总为单个返回值
	 * @param {function} callbackfn
	 * @param {*} initialValue  作为第一次调用 callback函数时的第一个参数的值。 如果没有提供初始值，则将使用数组中的第一个元素。
	 * @returns {*}
	 */
	reduce: function (callbackfn, initialValue) {
		if (this == null) throw new TypeError('this is null or not defined');
		if (typeof callbackfn !== 'function') throw new TypeError();
		var _obj = Object(this),
			len = _obj.length >>> 0,
			index = 0,
			value;

		if (arguments.length > 1) {
			value = arguments[1];
		} else {
			while (index < len && !(index in _obj)) index++;
			if (index >= len) throw new TypeError('Reduce of empty array with no initial value');
			value = _obj[index++];
		}
		while (index < len) {
			if (index in _obj) value = callbackfn(value, _obj[index], index, _obj);
			index++;
		}
		return value;
	},
	/**
	 * @author: ares
	 * @date: 2019/6/26 15:32
	 * @description: 接受一个函数作为累加器（accumulator）和数组的每个值（从右到左）将其减少为单个值
	 * @param {function} callbackfn
	 * @param {*} initialValue          可作为第一次调用回调 callback 的第一个参数
	 * @returns {*}
	 */
	reduceRight: function (callbackfn, initialValue) {
		if (this == null) throw new TypeError('this is null or not defined');
		if (typeof callbackfn !== 'function') throw new TypeError();
		var _obj = Object(this),
			len = _obj.length >>> 0,
			index = len - 1,
			value;
		if (arguments.length >= 2) {
			value = arguments[1];
		} else {
			while (index >= 0 && !(index in _obj)) index--;
			if (index < 0) throw new TypeError('Reduce of empty array with no initial value');
			value = _obj[index--];
		}
		for (; index >= 0; index--) {
			if (index in _obj) value = callbackfn(value, _obj[index], index, _obj);
		}
		return value;
	},
	/**
	 * @author: ares
	 * @date: 2019/6/26 15:34
	 * @description: 测试是否至少有一个元素可以通过被提供的函数方法。该方法返回一个Boolean类型的值
	 * @param {function} callbackfn
	 * @param {*} thisArg
	 * @returns {boolean}
	 */
	some: function (callbackfn, thisArg) {
		if (this == null) throw new TypeError('this is null or not defined');
		if (typeof callbackfn !== 'function') throw new TypeError();
		var context = arguments.length > 1 ? thisArg : this,
			_obj = Object(this),
			len = _obj.length >>> 0;
		for (var i = 0; i < len; i++) {
			if (i in _obj && callbackfn.call(context, _obj[i], i, _obj)) return true;
		}
		return false;
	}
};
/**
 * @author: ares
 * @date: 2019/6/26 9:27
 * @description: Array.prototype.copyWithin
 */
typeof Array.prototype.copyWithin === 'function' || (Array.prototype.copyWithin = arrayPrototype.copyWithin);
/**
 * @author: ares
 * @date: 2019/6/26 9:31
 * @description: Array.prototype.every
 */
typeof Array.prototype.every === 'function' || (Array.prototype.every = arrayPrototype.every);
/**
 * @author: ares
 * @date: 2019/6/26 9:33
 * @description: Array.prototype.fill
 */
typeof Array.prototype.fill === 'function' || (Object.defineProperty(Array.prototype, 'fill', {
	value: arrayPrototype.fill
}));
/**
 * @author: ares
 * @date: 2019/6/26 9:35
 * @description: Array.prototype.filter
 */
typeof Array.prototype.filter === 'function' || (Array.prototype.filter = arrayPrototype.filter);
/**
 * @author: ares
 * @date: 2019/6/26 9:37
 * @description: Array.prototype.find
 */
typeof Array.prototype.find === 'function' || (Object.defineProperty(Array.prototype, 'find', {
	value: arrayPrototype.find,
	configurable: true,
	writable: true
}));
/**
 * @author: ares
 * @date: 2019/6/26 9:40
 * @description: Array.prototype.findIndex
 */
typeof Array.prototype.findIndex === 'function' || (Object.defineProperty(Array.prototype, 'findIndex', {
	value: arrayPrototype.findIndex,
	configurable: true,
	writable: true
}));
/**
 * @author: ares
 * @date: 2019/6/26 10:03
 * @description: Array.prototype.forEach
 */
typeof Array.prototype.forEach === 'function' || (Array.prototype.forEach = arrayPrototype.forEach);
/**
 * @author: ares
 * @date: 2019/6/26 10:05
 * @description: Array.prototype.includes
 */
typeof Array.prototype.includes === 'function' || (Object.defineProperty(Array.prototype, 'includes', {
	value: arrayPrototype.includes
}));
/**
 * @author: ares
 * @date: 2019/6/26 10:08
 * @description: Array.prototype.indexOf
 */
typeof Array.prototype.indexOf === 'function' || (Array.prototype.indexOf = arrayPrototype.indexOf);
/**
 * @author: ares
 * @date: 2019/6/26 10:11
 * @description: Array.prototype.lastIndexOf
 */
typeof Array.prototype.lastIndexOf === 'function' || (Array.prototype.lastIndexOf = arrayPrototype.lastIndexOf);
/**
 * @author: ares
 * @date: 2019/6/25 11:20
 * @description: Array.prototype.map
 */
typeof Array.prototype.map === 'function' || (Array.prototype.map = arrayPrototype.map);
/**
 * @author: ares
 * @date: 2019/6/26 10:14
 * @description: Array.prototype.reduce
 */
typeof Array.prototype.reduce === 'function' || (Object.defineProperty(Array.prototype, 'reduce', {
	value: arrayPrototype.reduce
}));
/**
 * @author: ares
 * @date: 2019/6/26 10:16
 * @description: Array.prototype.reduceRight
 */
typeof Array.prototype.reduceRight === 'function' || (Array.prototype.reduceRight = arrayPrototype.reduceRight);
/**
 * @author: ares
 * @date: 2019/6/26 10:18
 * @description: Array.prototype.some
 */
typeof Array.prototype.some === 'function' || (Array.prototype.some = arrayPrototype.some);
```

# 给Array扩展静态方法

```js
var arrayMethod = {
	/**
	 * @author: ares
	 * @date: 2019/6/26 14:59
	 * @description: 从一个类似数组或可迭代对象中创建一个新的，浅拷贝的数组实例
	 */
	from: (function () {
		var toStr = Object.prototype.toString;
		var isCallable = function (fn) {
			return typeof fn === 'function' || toStr.call(fn) === '[object Function]';
		};
		var toInteger = function (value) {
			var number = Number(value);
			if (isNaN(number)) {
				return 0;
			}
			if (number === 0 || !isFinite(number)) {
				return number;
			}
			return (number > 0 ? 1 : -1) * Math.floor(Math.abs(number));
		};
		var maxSafeInteger = Math.pow(2, 53) - 1;
		var toLength = function (value) {
			var len = toInteger(value);
			return Math.min(Math.max(len, 0), maxSafeInteger);
		};

		// The length property of the from method is 1.
		return function from(arrayLike/*, mapFn, thisArg */) {
			// 1. Let C be the this value.
			var C = this;

			// 2. Let items be ToObject(arrayLike).
			var items = Object(arrayLike);

			// 3. ReturnIfAbrupt(items).
			if (arrayLike == null) {
				throw new TypeError('Array.from requires an array-like object - not null or undefined');
			}

			// 4. If mapfn is undefined, then let mapping be false.
			var mapFn = arguments.length > 1 ? arguments[1] : void undefined;
			var T;
			if (typeof mapFn !== 'undefined') {
				// 5. else
				// 5. a If IsCallable(mapfn) is false, throw a TypeError exception.
				if (!isCallable(mapFn)) {
					throw new TypeError('Array.from: when provided, the second argument must be a function');
				}

				// 5. b. If thisArg was supplied, let T be thisArg; else let T be undefined.
				if (arguments.length > 2) {
					T = arguments[2];
				}
			}

			// 10. Let lenValue be Get(items, "length").
			// 11. Let len be ToLength(lenValue).
			var len = toLength(items.length);

			// 13. If IsConstructor(C) is true, then
			// 13. a. Let A be the result of calling the [[Construct]] internal method
			// of C with an argument list containing the single item len.
			// 14. a. Else, Let A be ArrayCreate(len).
			var A = isCallable(C) ? Object(new C(len)) : new Array(len);

			// 16. Let k be 0.
			var k = 0;
			// 17. Repeat, while k < len… (also steps a - h)
			var kValue;
			while (k < len) {
				kValue = items[k];
				if (mapFn) {
					A[k] = typeof T === 'undefined' ? mapFn(kValue, k) : mapFn.call(T, kValue, k);
				} else {
					A[k] = kValue;
				}
				k += 1;
			}
			// 18. Let putStatus be Put(A, "length", len, true).
			A.length = len;
			// 20. Return A.
			return A;
		};
	}()),
	/**
	 * @author: ares
	 * @date: 2019/6/26 15:00
	 * @description: 确定传递的值是否是一个 Array
	 * @param {*} arg
	 * @returns {boolean}
	 */
	isArray: function (arg) {
		return Object.prototype.toString.call(arg) === "[object Array]";
	},
	/**
	 * @author: ares
	 * @date: 2019/6/26 15:02
	 * @description: 创建一个具有可变数量参数的新数组实例，而不考虑参数的数量或类型
	 */
	of: function () {
		return Array.prototype.slice.call(arguments);
	},
};
/**
 * @author: ares
 * @date: 2019/6/26 10:19
 * @description: Array.from
 */
typeof Array.from === 'function' || (Array.from = arrayMethod.from);
/**
 * @author: ares
 * @date: 2019/6/26 10:19
 * @description: Array.isArray
 */
typeof Array.isArray === 'function' || (Array.isArray = arrayMethod.isArray);
/**
 * @author: ares
 * @date: 2019/6/26 10:20
 * @description: Array.of
 */
typeof Array.of === 'function' || (Array.of = arrayMethod.of);
```

# 给String扩展原型方法

```js
var stringPrototype = {
	/**
	 * @author: ares
	 * @date: 2019/6/26 12:51
	 * @description: 判断当前字符串是否是以另外一个给定的子字符串结尾
	 * @param {string} search     要搜索的子字符串
	 * @param {number} this_len   可选参数,作为被操作字符串的长度。默认值为被操作字符串的length
	 * @returns {boolean}
	 */
	endsWith: function (search, this_len) {
		if (this_len === undefined || this_len > this.length) this_len = this.length;
		return this.substring(this_len - search.length, this_len) === search;
	},
	/**
	 * @author: ares
	 * @date: 2019/6/26 12:49
	 * @description: 判断当前字符串是否以另外一个给定的子字符串开头
	 * @param {string} search   要搜索的子字符串
	 * @param {number} pos      可选参数,从哪个索引位置开始搜寻子字符串，默认值为0
	 * @returns {boolean}
	 */
	startsWith: function (search, pos) {
		pos = !pos || pos < 0 ? 0 : +pos;
		return this.substring(pos, pos + search.length) === search;
	},
	/**
	 * @author: ares
	 * @date: 2019/6/26 12:47
	 * @description: 判断一个字符串是否包含在另一个字符串中
	 * @param {string} search  要搜索的字符串
	 * @param {number} start   可选参数,从哪个索引位置开始搜寻子字符串，默认值为0
	 * @returns {boolean}
	 */
	includes: function (search, start) {
		if (typeof start !== 'number') start = 0;
		if (start + search.length > this.length) {
			return false;
		} else {
			return this.indexOf(search, start) !== -1;
		}
	},
	/**
	 * @author: ares
	 * @date: 2019/6/26 12:46
	 * @description: 字符串后置填充,使字符串长度达到指定长度
	 * @param {number} targetLength   字符串预期长度
	 * @param {string} padString      用来填充的字符串
	 * @returns {string}
	 */
	padEnd: function (targetLength, padString) {
		targetLength = targetLength >> 0; //floor if number or convert non-number to 0;
		padString = String((typeof padString !== 'undefined' ? padString : ''));
		if (this.length > targetLength) {
			return String(this);
		} else {
			targetLength = targetLength - this.length;
			if (targetLength > padString.length) {
				padString += padString.repeat(targetLength / padString.length); //append to original to ensure we are longer than needed
			}
			return String(this) + padString.slice(0, targetLength);
		}
	},
	/**
	 * @author: ares
	 * @date: 2019/6/26 11:00
	 * @description: 字符串前置填充,使字符串长度达到指定长度
	 * @param {number} targetLength   字符串预期长度
	 * @param {string} padString      用来填充的字符串
	 * @returns {string}
	 */
	padStart: function (targetLength, padString) {
		targetLength = targetLength >> 0; //floor if number or convert non-number to 0;
		padString = String((typeof padString !== 'undefined' ? padString : ' '));
		if (this.length > targetLength) {
			return String(this);
		} else {
			targetLength = targetLength - this.length;
			if (targetLength > padString.length) {
				padString += padString.repeat(targetLength / padString.length); //append to original to ensure we are longer than needed
			}
			return padString.slice(0, targetLength) + String(this);
		}
	},
	/**
	 * @author: ares
	 * @date: 2019/6/26 10:54
	 * @description: 重复字符串
	 * @param {number} count  重复次数
	 * @returns {string}
	 */
	repeat: function (count) {
		if (this == null) throw new TypeError('can\'t convert this to object');
		var str = '' + this;
		count = +count;
		if (count != count) count = 0;
		if (count < 0) throw new RangeError('repeat count must be non-negative');
		if (count == Infinity) throw new RangeError('repeat count must be less than infinity');
		count = Math.floor(count);
		if (str.length == 0 || count == 0) return '';
		// 确保 count 是一个 31 位的整数。这样我们就可以使用如下优化的算法。
		// 当前（2014年8月），绝大多数浏览器都不能支持 1 << 28 长的字符串，所以：
		if (str.length * count >= 1 << 28) throw new RangeError('repeat count must not overflow maximum string size');
		var rpt = '';
		for (; ;) {
			if ((count & 1) == 1) rpt += str;
			count >>>= 1;
			if (count == 0) break;
			str += str;
		}
		return rpt;
	},
	/**
	 * @author: ares
	 * @date: 2019/6/26 10:50
	 * @description: 从一个字符串的两端删除空白字符
	 */
	trim: function () {
		return this.replace(/^[\s\uFEFF\xA0]+|[\s\uFEFF\xA0]+$/g, '');
	},
	/**
	 * @author: ares
	 * @date: 2019/6/26 10:52
	 * @description: 从一个字符串的左端删除空白字符
	 */
	trimLeft: function () {
		return this.replace(/^[\s\uFEFF\xA0]+/g, '');
	},
	/**
	 * @author: ares
	 * @date: 2019/6/26 10:53
	 * @description: 从一个字符串的右端删除空白字符
	 */
	trimRight: function () {
		return this.replace(/[\s\uFEFF\xA0]+$/g, '');
	},
};
/**
 * @author: ares
 * @date: 2019/6/26 10:36
 * @description: String.prototype.endsWith
 */
typeof String.prototype.endsWith === 'function' || (String.prototype.endsWith = stringPrototype.endsWith);
/**
 * @author: ares
 * @date: 2019/6/26 10:44
 * @description: String.prototype.startsWith
 */
typeof String.prototype.startsWith === 'function' || (String.prototype.startsWith = stringPrototype.startsWith);
/**
 * @author: ares
 * @date: 2019/6/26 10:37
 * @description: String.prototype.includes
 */
typeof String.prototype.includes === 'function' || (String.prototype.includes = stringPrototype.includes);
/**
 * @author: ares
 * @date: 2019/6/26 10:39
 * @description: String.prototype.padEnd
 */
typeof String.prototype.padEnd === 'function' || (String.prototype.padEnd = stringPrototype.padEnd);
/**
 * @author: ares
 * @date: 2019/6/26 10:41
 * @description: String.prototype.padStart
 */
typeof String.prototype.padStart === 'function' || (String.prototype.padStart = stringPrototype.padStart);
/**
 * @author: ares
 * @date: 2019/6/26 10:43
 * @description: String.prototype.repeat
 */
typeof String.prototype.repeat === 'function' || (String.prototype.repeat = stringPrototype.repeat);
/**
 * @author: ares
 * @date: 2019/6/26 10:46
 * @description: String.prototype.trim
 */
typeof String.prototype.trim === 'function' || (String.prototype.trim = stringPrototype.trim);
/**
 * @author: ares
 * @date: 2019/6/26 10:53
 * @description: String.prototype.trimLeft
 */
typeof String.prototype.trimLeft === 'function' || (String.prototype.trimLeft = stringPrototype.trimLeft);
/**
 * @author: ares
 * @date: 2019/6/26 10:54
 * @description: String.prototype.trimRight
 */
typeof String.prototype.trimRight === 'function' || (String.prototype.trimRight = stringPrototype.trimRight);
```