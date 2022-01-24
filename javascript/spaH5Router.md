spa应用, 主体使用iframe, 路由使用H5 history, 依赖jquery

```js
(function (root, factory) {
	if (typeof define === 'function' && define.amd) {
		// AMD
		define(['../plugins/jquery-3.2.1.min.js'], factory);
	} else if (typeof exports === 'object') {
		// CommonJS
		exports["h5Router"] = factory(require('../plugins/jquery-3.2.1.min.js'));
	} else {
		// Browser globals
		root["h5Router"] = factory($);
	}

}(this, function ($) {

	function H5router() {
		this.config = {};
	}


	/**
	 * @author: ares
	 * @date: 2020/2/1 18:20
	 * @description: 加载spa iframe实际调用的方法
	 * @param {string}path            要加载文件的路径
	 * @param {function}callback      加载完请求的文档后如果需要额外操作,可使用
	 * @private
	 */
	function _loadIframe(path, callback, historyState) {
		var container = document.getElementById(h5router.config.container),
			oldIframe = document.querySelector('#' + h5router.config.container + ' iframe'),
			newIframe = document.createElement('iframe');
		container.removeChild(oldIframe);
		newIframe.onload = function (event) {
			Object.prototype.toString.call(callback) == "[object Function]" && callback(event, historyState);
		}
		newIframe.src = path;
		container.appendChild(newIframe)
		return h5router;
	}

	H5router.prototype = {
		constructor: H5router,
		/**
		 * @author: ares
		 * @date: 2020/1/30 11:49
		 * @description: 覆盖config
		 * @param {object}option
		 * @returns {H5router}
		 */
		set: function (option) {
			$.extend(true, this.config, option);
			return this;
		},

		/**
		 * @author: ares
		 * @date: 2020/2/1 18:21
		 * @description: h5 onpopstate事件
		 * @param {object}event            onpopstate事件对象
		 * @param {function}callback   加载完请求的文档后如果需要额外操作,可使用
		 */
		onIframePopstate: function (event, callback) {
			var target = event.currentTarget,
				hash = target.location.hash.split('?')[0],
				state = target.history.state,
				path = /\.html$/.test(hash.slice(1)) ? hash.slice(1) : hash.slice(1) + '.html';
			if (!hash || hash.length <= 1) path = this.config.homePagePath;
			_loadIframe(path, callback, state)
			return this;
		},

		/**
		 * @author: ares
		 * @date: 2020/2/1 17:25
		 * @description: 加载spa iframe
		 * @param {string}path            要加载文件的路径
		 * @param {object}params          给history添加的state数据
		 * @param {function}callback      加载完请求的文档后如果需要额外操作,可使用
		 */
		loadIframe: function (path, params, callback) {
			var queryParams = path.split('?')[1] ? qs.parse(path.split('?')[1]) : {},
				params = Object.prototype.toString.call(params) == "[object Object]" ? params : {};
			$.extend(true, params, queryParams);
			_loadIframe(path.split('?')[0], callback)
			history.pushState(params, null, '#' + path.replace(/\.html$/, ''));
			return this;
		},

		/**
		 * @author: ares
		 * @date: 2020/2/1 17:25
		 * @description: 刷新spa iframe
		 * @param callback
		 */
		refreshIframe: function (callback) {
			var hash = location.hash,
				path = /\.html$/.test(hash.slice(1)) ? hash.slice(1) : hash.slice(1) + '.html';
			if (!hash || hash.length <= 1) path = this.config.homePagePath;
			_loadIframe(path, callback)
			return this;
		}
	}


	return new H5router();
}))
```