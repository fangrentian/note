# Vue构造函数

```js
function Vue(options) {
	if (process.env.NODE_ENV !== 'production' &&
		!(this instanceof Vue)
	) {
		warn('Vue is a constructor and should be called with the `new` keyword')
	}
	// 实例化时执行初始化方法
	this._init(options)
}
```

> 源码: src/core/instance/index.js

# 导入Vue时执行的方法

```js
// 定义 Vue.prototype._init
initMixin(Vue)
// 定义 Object.defineProperty(Vue.prototype, '$data', dataDef)
// 定义 Object.defineProperty(Vue.prototype, '$props', propsDef)
// 定义 Vue.prototype.$set
// 定义 Vue.prototype.$delete
// 定义 Vue.prototype.$watch
stateMixin(Vue)
// 定义 Vue.prototype.$on
// 定义 Vue.prototype.$once
// 定义 Vue.prototype.$off
// 定义 Vue.prototype.$emit
eventsMixin(Vue)
// 定义 Vue.prototype._update
// 定义 Vue.prototype.$forceUpdate
// 定义 Vue.prototype.$destroy
lifecycleMixin(Vue)
// installRenderHelpers(Vue.prototype) 导入辅助工具
// 定义 Vue.prototype.$nextTick
// 定义 Vue.prototype._render
renderMixin(Vue)
```

> 源码: src/core/instance/index.js

```js
// 定义 Object.defineProperty(Vue, 'config', configDef)
// 定义 Vue.set
// 定义 Vue.delete
// 定义 Vue.nextTick
// 定义 Vue.observable
// 定义 Vue.observable
// 定义 Vue.options = Object.create(null)
// 扩展 ASSET_TYPES.forEach(type => {
Vue.options[type + 's'] = Object.create(null)
})
// 定义 Vue.options._base = Vue
// 扩展组件 extend(Vue.options.components, builtInComponents)
initGlobalAPI(Vue)
```

> 源码: src/core/index.js

```js
// initGlobalAPI(Vue)内部执行的方法

// 定义 Vue.use
initUse(Vue)
// 定义 Vue.mixin
initMixin(Vue)
// 定义 Vue.extend
initExtend(Vue)
// 定义 ASSET_TYPES 方法     component|directive|filter
initAssetRegisters(Vue)
```

> 源码: src/core/global-api/index.js

# 导入Vue时扩展的属性

```js
Object.defineProperty(Vue.prototype, '$isServer', {
	get: isServerRendering
})

Object.defineProperty(Vue.prototype, '$ssrContext', {
	get() {
		/* istanbul ignore next */
		return this.$vnode && this.$vnode.ssrContext
	}
})

// expose FunctionalRenderContext for ssr runtime helper installation
Object.defineProperty(Vue, 'FunctionalRenderContext', {
	value: FunctionalRenderContext
})

Vue.version = '__VERSION__'
```

> 源码: src/core/index.js

# Vue实例化

```js
// render为 Vue.prototype._render方法中传入的vm.$createElement方法 (vnode = render.call(vm._renderProxy, vm.$createElement))
new Vue({
	render: h => h(App),
})
```

# 初始化时调用的方法

```js
Vue.prototype._init = function (options?: Object) {
	const vm: Component = this
	// a uid
	vm._uid = uid++

	let startTag, endTag
	/* istanbul ignore if */
	if (process.env.NODE_ENV !== 'production' && config.performance && mark) {
		startTag = `vue-perf-start:${vm._uid}`
		endTag = `vue-perf-end:${vm._uid}`
		mark(startTag)
	}

	// a flag to avoid this being observed
	vm._isVue = true
	// merge options
	if (options && options._isComponent) {
		// optimize internal component instantiation
		// since dynamic options merging is pretty slow, and none of the
		// internal component options needs special treatment.
		initInternalComponent(vm, options)
	} else {
		vm.$options = mergeOptions(
			resolveConstructorOptions(vm.constructor),
			options || {},
			vm
		)
	}
	/* istanbul ignore else */
	if (process.env.NODE_ENV !== 'production') {
		initProxy(vm)
	} else {
		vm._renderProxy = vm
	}
	// expose real self
	vm._self = vm
	// 初始化 vm.$parent,vm.$root,vm.$children,vm.$refs,vm._watcher,vm._inactive,vm._directInactive,vm._isMounted,vm._isDestroyed,vm._isBeingDestroyed
	initLifecycle(vm)
	// 初始化 vm._events,vm._hasHookEvent, 如果vm.$options._parentListeners存在,则调用updateComponentListeners
	initEvents(vm)
	// 定义 vm._vnode
	// 定义 vm._staticTrees
	// 定义 vm.$slots
	// 定义 vm.$scopedSlots
	// 定义 vm._c = (a, b, c, d) => createElement(vm, a, b, c, d, false)
	// 定义 vm.$createElement = (a, b, c, d) => createElement(vm, a, b, c, d, true)
	// 定义 defineReactive(vm, '$attrs', parentData && parentData.attrs || emptyObject, null, true)
	// 定义 defineReactive(vm, '$listeners', options._parentListeners || emptyObject, null, true)
	initRender(vm)
	// 调用 beforeCreate钩子方法
	callHook(vm, 'beforeCreate')
	// 解析 inject对象resolveInject(vm.$options.inject, vm)
	// inject对象响应式 defineReactive(vm, key, result[key])
	initInjections(vm) // resolve injections before data/props
	// 初始化 props,methods,data,computed,watch
	initState(vm)
	// 初始化 provide对象 vm.$options.provide
	initProvide(vm) // resolve provide after data/props
	// 调用 created钩子方法
	callHook(vm, 'created')

	/* istanbul ignore if */
	if (process.env.NODE_ENV !== 'production' && config.performance && mark) {
		vm._name = formatComponentName(vm, false)
		mark(endTag)
		measure(`vue ${vm._name} init`, startTag, endTag)
	}

	if (vm.$options.el) {
		vm.$mount(vm.$options.el)
	}
}
```

# Vue实例化挂载

```js
// 执行挂载组件方法
app.$mount('#app')
```

```js
// 挂载方法定义，实际执行的是mountComponent
Vue.prototype.$mount = function (
	el?: string | Element,
	hydrating?: boolean
): Component {
	el = el && inBrowser ? query(el) : undefined
	return mountComponent(this, el, hydrating)
}
```

> 源码：src/platforms/web/runtime/index.js

```js
// 实际执行的挂载组件的方法
// 挂载组件时，实例化一个观察者（new Watcher），用来适时更新组件（执行updateComponent, 实际调用的是vm._update）
export function mountComponent(
	vm: Component,
	el: ?Element,
	hydrating?: boolean
): Component {
	vm.$el = el
	if (!vm.$options.render) {
		vm.$options.render = createEmptyVNode
		if (process.env.NODE_ENV !== 'production') {
			/* istanbul ignore if */
			if ((vm.$options.template && vm.$options.template.charAt(0) !== '#') ||
				vm.$options.el || el) {
				warn(
					'You are using the runtime-only build of Vue where the template ' +
					'compiler is not available. Either pre-compile the templates into ' +
					'render functions, or use the compiler-included build.',
					vm
				)
			} else {
				warn(
					'Failed to mount component: template or render function not defined.',
					vm
				)
			}
		}
	}
	callHook(vm, 'beforeMount')

	let updateComponent
	/* istanbul ignore if */
	if (process.env.NODE_ENV !== 'production' && config.performance && mark) {
		updateComponent = () => {
			const name = vm._name
			const id = vm._uid
			const startTag = `vue-perf-start:${id}`
			const endTag = `vue-perf-end:${id}`

			mark(startTag)
			const vnode = vm._render()
			mark(endTag)
			measure(`vue ${name} render`, startTag, endTag)

			mark(startTag)
			vm._update(vnode, hydrating)
			mark(endTag)
			measure(`vue ${name} patch`, startTag, endTag)
		}
	} else {
		updateComponent = () => {
			vm._update(vm._render(), hydrating)
		}
	}

	// we set this to vm._watcher inside the watcher's constructor
	// since the watcher's initial patch may call $forceUpdate (e.g. inside child
	// component's mounted hook), which relies on vm._watcher being already defined
	new Watcher(vm, updateComponent, noop, {
		before() {
			if (vm._isMounted && !vm._isDestroyed) {
				callHook(vm, 'beforeUpdate')
			}
		}
	}, true /* isRenderWatcher */)
	hydrating = false

	// manually mounted instance, call mounted on self
	// mounted is called for render-created child components in its inserted hook
	if (vm.$vnode == null) {
		vm._isMounted = true
		callHook(vm, 'mounted')
	}
	return vm
}
```

> 源码：src/core/instance/lifecycle.js

```js
// 观察者
// expOrFn(getter)实际传入的是mountComponent方法内部定义的updateComponent方法
// 实例化时调用this.value = this.lazy ? undefined : this.get(),
// 实际调用的是value = this.getter.call(vm, vm)
export default class Watcher {
	vm: Component;
	expression: string;
	cb: Function;
	id: number;
	deep: boolean;
	user: boolean;
	lazy: boolean;
	sync: boolean;
	dirty: boolean;
	active: boolean;
	deps: Array<Dep>;
	newDeps: Array<Dep>;
	depIds: SimpleSet;
	newDepIds: SimpleSet;
	before: ?Function;
	getter: Function;
	value: any;

	constructor(
		vm: Component,
		expOrFn: string | Function,
		cb: Function,
		options?: ?Object,
		isRenderWatcher?: boolean
	) {
		this.vm = vm
		if (isRenderWatcher) {
			vm._watcher = this
		}
		vm._watchers.push(this)
		// options
		if (options) {
			this.deep = !!options.deep
			this.user = !!options.user
			this.lazy = !!options.lazy
			this.sync = !!options.sync
			this.before = options.before
		} else {
			this.deep = this.user = this.lazy = this.sync = false
		}
		this.cb = cb
		this.id = ++uid // uid for batching
		this.active = true
		this.dirty = this.lazy // for lazy watchers
		this.deps = []
		this.newDeps = []
		this.depIds = new Set()
		this.newDepIds = new Set()
		this.expression = process.env.NODE_ENV !== 'production'
			? expOrFn.toString()
			: ''
		// parse expression for getter
		if (typeof expOrFn === 'function') {
			this.getter = expOrFn
		} else {
			this.getter = parsePath(expOrFn)
			if (!this.getter) {
				this.getter = noop
				process.env.NODE_ENV !== 'production' && warn(
					`Failed watching path: "${expOrFn}" ` +
					'Watcher only accepts simple dot-delimited paths. ' +
					'For full control, use a function instead.',
					vm
				)
			}
		}
		this.value = this.lazy
			? undefined
			: this.get()
	}

	/**
	 * Evaluate the getter, and re-collect dependencies.
	 */
	get() {
		pushTarget(this)
		let value
		const vm = this.vm
		try {
			value = this.getter.call(vm, vm)
		} catch (e) {
			if (this.user) {
				handleError(e, vm, `getter for watcher "${this.expression}"`)
			} else {
				throw e
			}
		} finally {
			// "touch" every property so they are all tracked as
			// dependencies for deep watching
			if (this.deep) {
				traverse(value)
			}
			popTarget()
			this.cleanupDeps()
		}
		return value
	}

	/**
	 * Add a dependency to this directive.
	 */
	addDep(dep: Dep) {
		const id = dep.id
		if (!this.newDepIds.has(id)) {
			this.newDepIds.add(id)
			this.newDeps.push(dep)
			if (!this.depIds.has(id)) {
				dep.addSub(this)
			}
		}
	}

	/**
	 * Clean up for dependency collection.
	 */
	cleanupDeps() {
		let i = this.deps.length
		while (i--) {
			const dep = this.deps[i]
			if (!this.newDepIds.has(dep.id)) {
				dep.removeSub(this)
			}
		}
		let tmp = this.depIds
		this.depIds = this.newDepIds
		this.newDepIds = tmp
		this.newDepIds.clear()
		tmp = this.deps
		this.deps = this.newDeps
		this.newDeps = tmp
		this.newDeps.length = 0
	}

	/**
	 * Subscriber interface.
	 * Will be called when a dependency changes.
	 */
	update() {
		/* istanbul ignore else */
		if (this.lazy) {
			this.dirty = true
		} else if (this.sync) {
			this.run()
		} else {
			queueWatcher(this)
		}
	}

	/**
	 * Scheduler job interface.
	 * Will be called by the scheduler.
	 */
	run() {
		if (this.active) {
			const value = this.get()
			if (
				value !== this.value ||
				// Deep watchers and watchers on Object/Arrays should fire even
				// when the value is the same, because the value may
				// have mutated.
				isObject(value) ||
				this.deep
			) {
				// set new value
				const oldValue = this.value
				this.value = value
				if (this.user) {
					try {
						this.cb.call(this.vm, value, oldValue)
					} catch (e) {
						handleError(e, this.vm, `callback for watcher "${this.expression}"`)
					}
				} else {
					this.cb.call(this.vm, value, oldValue)
				}
			}
		}
	}

	/**
	 * Evaluate the value of the watcher.
	 * This only gets called for lazy watchers.
	 */
	evaluate() {
		this.value = this.get()
		this.dirty = false
	}

	/**
	 * Depend on all deps collected by this watcher.
	 */
	depend() {
		let i = this.deps.length
		while (i--) {
			this.deps[i].depend()
		}
	}

	/**
	 * Remove self from all dependencies' subscriber list.
	 */
	teardown() {
		if (this.active) {
			// remove self from vm's watcher list
			// this is a somewhat expensive operation so we skip it
			// if the vm is being destroyed.
			if (!this.vm._isBeingDestroyed) {
				remove(this.vm._watchers, this)
			}
			let i = this.deps.length
			while (i--) {
				this.deps[i].removeSub(this)
			}
			this.active = false
		}
	}
}
```

> 源码: src/core/observer/watcher.js

```js
// 更新组件(vm._update)时调用的方法, 实际调用的是vm.__patch__方法
Vue.prototype._update = function (vnode: VNode, hydrating?: boolean) {
	const vm: Component = this
	const prevEl = vm.$el
	const prevVnode = vm._vnode
	const restoreActiveInstance = setActiveInstance(vm)
	vm._vnode = vnode
	// Vue.prototype.__patch__ is injected in entry points
	// based on the rendering backend used.
	if (!prevVnode) {
		// initial render
		vm.$el = vm.__patch__(vm.$el, vnode, hydrating, false /* removeOnly */)
	} else {
		// updates
		vm.$el = vm.__patch__(prevVnode, vnode)
	}
	restoreActiveInstance()
	// update __vue__ reference
	if (prevEl) {
		prevEl.__vue__ = null
	}
	if (vm.$el) {
		vm.$el.__vue__ = vm
	}
	// if parent is an HOC, update its $el as well
	if (vm.$vnode && vm.$parent && vm.$vnode === vm.$parent._vnode) {
		vm.$parent.$el = vm.$el
	}
	// updated hook is called by the scheduler to ensure that children are
	// updated in a parent's updated hook.
}
```

> 源码: src/core/instance/lifecycle.js

```js
// 就是createPatchFunction({ nodeOps, modules })方法返回的return function patch (oldVnode, vnode, hydrating, removeOnly)
Vue.prototype.__patch__ = inBrowser ? patch : noop
```

> 源码: src/platforms/web/runtime/index.js

```js
// 返回return function patch (oldVnode, vnode, hydrating, removeOnly)
export const patch: Function = createPatchFunction({nodeOps, modules})
```

> 源码: src/platforms/web/runtime/patch.js

```js
// 返回patch方法
export function createPatchFunction(backend) {
	let i, j
	const cbs = {}

	const {modules, nodeOps} = backend

	for (i = 0; i < hooks.length; ++i) {
		cbs[hooks[i]] = []
		for (j = 0; j < modules.length; ++j) {
			if (isDef(modules[j][hooks[i]])) {
				cbs[hooks[i]].push(modules[j][hooks[i]])
			}
		}
	}

	function emptyNodeAt(elm) {
		return new VNode(nodeOps.tagName(elm).toLowerCase(), {}, [], undefined, elm)
	}

	function createRmCb(childElm, listeners) {
		function remove() {
			if (--remove.listeners === 0) {
				removeNode(childElm)
			}
		}

		remove.listeners = listeners
		return remove
	}

	function removeNode(el) {
		const parent = nodeOps.parentNode(el)
		// element may have already been removed due to v-html / v-text
		if (isDef(parent)) {
			nodeOps.removeChild(parent, el)
		}
	}

	function isUnknownElement(vnode, inVPre) {
		return (
			!inVPre &&
			!vnode.ns &&
			!(
				config.ignoredElements.length &&
				config.ignoredElements.some(ignore => {
					return isRegExp(ignore)
						? ignore.test(vnode.tag)
						: ignore === vnode.tag
				})
			) &&
			config.isUnknownElement(vnode.tag)
		)
	}

	let creatingElmInVPre = 0

	function createElm(
		vnode,
		insertedVnodeQueue,
		parentElm,
		refElm,
		nested,
		ownerArray,
		index
	) {
		if (isDef(vnode.elm) && isDef(ownerArray)) {
			// This vnode was used in a previous render!
			// now it's used as a new node, overwriting its elm would cause
			// potential patch errors down the road when it's used as an insertion
			// reference node. Instead, we clone the node on-demand before creating
			// associated DOM element for it.
			vnode = ownerArray[index] = cloneVNode(vnode)
		}

		vnode.isRootInsert = !nested // for transition enter check
		if (createComponent(vnode, insertedVnodeQueue, parentElm, refElm)) {
			return
		}

		const data = vnode.data
		const children = vnode.children
		const tag = vnode.tag
		if (isDef(tag)) {
			if (process.env.NODE_ENV !== 'production') {
				if (data && data.pre) {
					creatingElmInVPre++
				}
				if (isUnknownElement(vnode, creatingElmInVPre)) {
					warn(
						'Unknown custom element: <' + tag + '> - did you ' +
						'register the component correctly? For recursive components, ' +
						'make sure to provide the "name" option.',
						vnode.context
					)
				}
			}

			vnode.elm = vnode.ns
				? nodeOps.createElementNS(vnode.ns, tag)
				: nodeOps.createElement(tag, vnode)
			setScope(vnode)

			/* istanbul ignore if */
			if (__WEEX__) {
				// in Weex, the default insertion order is parent-first.
				// List items can be optimized to use children-first insertion
				// with append="tree".
				const appendAsTree = isDef(data) && isTrue(data.appendAsTree)
				if (!appendAsTree) {
					if (isDef(data)) {
						invokeCreateHooks(vnode, insertedVnodeQueue)
					}
					insert(parentElm, vnode.elm, refElm)
				}
				createChildren(vnode, children, insertedVnodeQueue)
				if (appendAsTree) {
					if (isDef(data)) {
						invokeCreateHooks(vnode, insertedVnodeQueue)
					}
					insert(parentElm, vnode.elm, refElm)
				}
			} else {
				createChildren(vnode, children, insertedVnodeQueue)
				if (isDef(data)) {
					invokeCreateHooks(vnode, insertedVnodeQueue)
				}
				insert(parentElm, vnode.elm, refElm)
			}

			if (process.env.NODE_ENV !== 'production' && data && data.pre) {
				creatingElmInVPre--
			}
		} else if (isTrue(vnode.isComment)) {
			vnode.elm = nodeOps.createComment(vnode.text)
			insert(parentElm, vnode.elm, refElm)
		} else {
			vnode.elm = nodeOps.createTextNode(vnode.text)
			insert(parentElm, vnode.elm, refElm)
		}
	}

	function createComponent(vnode, insertedVnodeQueue, parentElm, refElm) {
		let i = vnode.data
		if (isDef(i)) {
			const isReactivated = isDef(vnode.componentInstance) && i.keepAlive
			if (isDef(i = i.hook) && isDef(i = i.init)) {
				i(vnode, false /* hydrating */)
			}
			// after calling the init hook, if the vnode is a child component
			// it should've created a child instance and mounted it. the child
			// component also has set the placeholder vnode's elm.
			// in that case we can just return the element and be done.
			if (isDef(vnode.componentInstance)) {
				initComponent(vnode, insertedVnodeQueue)
				insert(parentElm, vnode.elm, refElm)
				if (isTrue(isReactivated)) {
					reactivateComponent(vnode, insertedVnodeQueue, parentElm, refElm)
				}
				return true
			}
		}
	}

	function initComponent(vnode, insertedVnodeQueue) {
		if (isDef(vnode.data.pendingInsert)) {
			insertedVnodeQueue.push.apply(insertedVnodeQueue, vnode.data.pendingInsert)
			vnode.data.pendingInsert = null
		}
		vnode.elm = vnode.componentInstance.$el
		if (isPatchable(vnode)) {
			invokeCreateHooks(vnode, insertedVnodeQueue)
			setScope(vnode)
		} else {
			// empty component root.
			// skip all element-related modules except for ref (#3455)
			registerRef(vnode)
			// make sure to invoke the insert hook
			insertedVnodeQueue.push(vnode)
		}
	}

	function reactivateComponent(vnode, insertedVnodeQueue, parentElm, refElm) {
		let i
		// hack for #4339: a reactivated component with inner transition
		// does not trigger because the inner node's created hooks are not called
		// again. It's not ideal to involve module-specific logic in here but
		// there doesn't seem to be a better way to do it.
		let innerNode = vnode
		while (innerNode.componentInstance) {
			innerNode = innerNode.componentInstance._vnode
			if (isDef(i = innerNode.data) && isDef(i = i.transition)) {
				for (i = 0; i < cbs.activate.length; ++i) {
					cbs.activate[i](emptyNode, innerNode)
				}
				insertedVnodeQueue.push(innerNode)
				break
			}
		}
		// unlike a newly created component,
		// a reactivated keep-alive component doesn't insert itself
		insert(parentElm, vnode.elm, refElm)
	}

	function insert(parent, elm, ref) {
		if (isDef(parent)) {
			if (isDef(ref)) {
				if (nodeOps.parentNode(ref) === parent) {
					nodeOps.insertBefore(parent, elm, ref)
				}
			} else {
				nodeOps.appendChild(parent, elm)
			}
		}
	}

	function createChildren(vnode, children, insertedVnodeQueue) {
		if (Array.isArray(children)) {
			if (process.env.NODE_ENV !== 'production') {
				checkDuplicateKeys(children)
			}
			for (let i = 0; i < children.length; ++i) {
				createElm(children[i], insertedVnodeQueue, vnode.elm, null, true, children, i)
			}
		} else if (isPrimitive(vnode.text)) {
			nodeOps.appendChild(vnode.elm, nodeOps.createTextNode(String(vnode.text)))
		}
	}

	function isPatchable(vnode) {
		while (vnode.componentInstance) {
			vnode = vnode.componentInstance._vnode
		}
		return isDef(vnode.tag)
	}

	function invokeCreateHooks(vnode, insertedVnodeQueue) {
		for (let i = 0; i < cbs.create.length; ++i) {
			cbs.create[i](emptyNode, vnode)
		}
		i = vnode.data.hook // Reuse variable
		if (isDef(i)) {
			if (isDef(i.create)) i.create(emptyNode, vnode)
			if (isDef(i.insert)) insertedVnodeQueue.push(vnode)
		}
	}

	// set scope id attribute for scoped CSS.
	// this is implemented as a special case to avoid the overhead
	// of going through the normal attribute patching process.
	function setScope(vnode) {
		let i
		if (isDef(i = vnode.fnScopeId)) {
			nodeOps.setStyleScope(vnode.elm, i)
		} else {
			let ancestor = vnode
			while (ancestor) {
				if (isDef(i = ancestor.context) && isDef(i = i.$options._scopeId)) {
					nodeOps.setStyleScope(vnode.elm, i)
				}
				ancestor = ancestor.parent
			}
		}
		// for slot content they should also get the scopeId from the host instance.
		if (isDef(i = activeInstance) &&
			i !== vnode.context &&
			i !== vnode.fnContext &&
			isDef(i = i.$options._scopeId)
		) {
			nodeOps.setStyleScope(vnode.elm, i)
		}
	}

	function addVnodes(parentElm, refElm, vnodes, startIdx, endIdx, insertedVnodeQueue) {
		for (; startIdx <= endIdx; ++startIdx) {
			createElm(vnodes[startIdx], insertedVnodeQueue, parentElm, refElm, false, vnodes, startIdx)
		}
	}

	function invokeDestroyHook(vnode) {
		let i, j
		const data = vnode.data
		if (isDef(data)) {
			if (isDef(i = data.hook) && isDef(i = i.destroy)) i(vnode)
			for (i = 0; i < cbs.destroy.length; ++i) cbs.destroy[i](vnode)
		}
		if (isDef(i = vnode.children)) {
			for (j = 0; j < vnode.children.length; ++j) {
				invokeDestroyHook(vnode.children[j])
			}
		}
	}

	function removeVnodes(vnodes, startIdx, endIdx) {
		for (; startIdx <= endIdx; ++startIdx) {
			const ch = vnodes[startIdx]
			if (isDef(ch)) {
				if (isDef(ch.tag)) {
					removeAndInvokeRemoveHook(ch)
					invokeDestroyHook(ch)
				} else { // Text node
					removeNode(ch.elm)
				}
			}
		}
	}

	function removeAndInvokeRemoveHook(vnode, rm) {
		if (isDef(rm) || isDef(vnode.data)) {
			let i
			const listeners = cbs.remove.length + 1
			if (isDef(rm)) {
				// we have a recursively passed down rm callback
				// increase the listeners count
				rm.listeners += listeners
			} else {
				// directly removing
				rm = createRmCb(vnode.elm, listeners)
			}
			// recursively invoke hooks on child component root node
			if (isDef(i = vnode.componentInstance) && isDef(i = i._vnode) && isDef(i.data)) {
				removeAndInvokeRemoveHook(i, rm)
			}
			for (i = 0; i < cbs.remove.length; ++i) {
				cbs.remove[i](vnode, rm)
			}
			if (isDef(i = vnode.data.hook) && isDef(i = i.remove)) {
				i(vnode, rm)
			} else {
				rm()
			}
		} else {
			removeNode(vnode.elm)
		}
	}

	function updateChildren(parentElm, oldCh, newCh, insertedVnodeQueue, removeOnly) {
		let oldStartIdx = 0
		let newStartIdx = 0
		let oldEndIdx = oldCh.length - 1
		let oldStartVnode = oldCh[0]
		let oldEndVnode = oldCh[oldEndIdx]
		let newEndIdx = newCh.length - 1
		let newStartVnode = newCh[0]
		let newEndVnode = newCh[newEndIdx]
		let oldKeyToIdx, idxInOld, vnodeToMove, refElm

		// removeOnly is a special flag used only by <transition-group>
		// to ensure removed elements stay in correct relative positions
		// during leaving transitions
		const canMove = !removeOnly

		if (process.env.NODE_ENV !== 'production') {
			checkDuplicateKeys(newCh)
		}

		while (oldStartIdx <= oldEndIdx && newStartIdx <= newEndIdx) {
			if (isUndef(oldStartVnode)) {
				oldStartVnode = oldCh[++oldStartIdx] // Vnode has been moved left
			} else if (isUndef(oldEndVnode)) {
				oldEndVnode = oldCh[--oldEndIdx]
			} else if (sameVnode(oldStartVnode, newStartVnode)) {
				patchVnode(oldStartVnode, newStartVnode, insertedVnodeQueue, newCh, newStartIdx)
				oldStartVnode = oldCh[++oldStartIdx]
				newStartVnode = newCh[++newStartIdx]
			} else if (sameVnode(oldEndVnode, newEndVnode)) {
				patchVnode(oldEndVnode, newEndVnode, insertedVnodeQueue, newCh, newEndIdx)
				oldEndVnode = oldCh[--oldEndIdx]
				newEndVnode = newCh[--newEndIdx]
			} else if (sameVnode(oldStartVnode, newEndVnode)) { // Vnode moved right
				patchVnode(oldStartVnode, newEndVnode, insertedVnodeQueue, newCh, newEndIdx)
				canMove && nodeOps.insertBefore(parentElm, oldStartVnode.elm, nodeOps.nextSibling(oldEndVnode.elm))
				oldStartVnode = oldCh[++oldStartIdx]
				newEndVnode = newCh[--newEndIdx]
			} else if (sameVnode(oldEndVnode, newStartVnode)) { // Vnode moved left
				patchVnode(oldEndVnode, newStartVnode, insertedVnodeQueue, newCh, newStartIdx)
				canMove && nodeOps.insertBefore(parentElm, oldEndVnode.elm, oldStartVnode.elm)
				oldEndVnode = oldCh[--oldEndIdx]
				newStartVnode = newCh[++newStartIdx]
			} else {
				if (isUndef(oldKeyToIdx)) oldKeyToIdx = createKeyToOldIdx(oldCh, oldStartIdx, oldEndIdx)
				idxInOld = isDef(newStartVnode.key)
					? oldKeyToIdx[newStartVnode.key]
					: findIdxInOld(newStartVnode, oldCh, oldStartIdx, oldEndIdx)
				if (isUndef(idxInOld)) { // New element
					createElm(newStartVnode, insertedVnodeQueue, parentElm, oldStartVnode.elm, false, newCh, newStartIdx)
				} else {
					vnodeToMove = oldCh[idxInOld]
					if (sameVnode(vnodeToMove, newStartVnode)) {
						patchVnode(vnodeToMove, newStartVnode, insertedVnodeQueue, newCh, newStartIdx)
						oldCh[idxInOld] = undefined
						canMove && nodeOps.insertBefore(parentElm, vnodeToMove.elm, oldStartVnode.elm)
					} else {
						// same key but different element. treat as new element
						createElm(newStartVnode, insertedVnodeQueue, parentElm, oldStartVnode.elm, false, newCh, newStartIdx)
					}
				}
				newStartVnode = newCh[++newStartIdx]
			}
		}
		if (oldStartIdx > oldEndIdx) {
			refElm = isUndef(newCh[newEndIdx + 1]) ? null : newCh[newEndIdx + 1].elm
			addVnodes(parentElm, refElm, newCh, newStartIdx, newEndIdx, insertedVnodeQueue)
		} else if (newStartIdx > newEndIdx) {
			removeVnodes(oldCh, oldStartIdx, oldEndIdx)
		}
	}

	function checkDuplicateKeys(children) {
		const seenKeys = {}
		for (let i = 0; i < children.length; i++) {
			const vnode = children[i]
			const key = vnode.key
			if (isDef(key)) {
				if (seenKeys[key]) {
					warn(
						`Duplicate keys detected: '${key}'. This may cause an update error.`,
						vnode.context
					)
				} else {
					seenKeys[key] = true
				}
			}
		}
	}

	function findIdxInOld(node, oldCh, start, end) {
		for (let i = start; i < end; i++) {
			const c = oldCh[i]
			if (isDef(c) && sameVnode(node, c)) return i
		}
	}

	function patchVnode(
		oldVnode,
		vnode,
		insertedVnodeQueue,
		ownerArray,
		index,
		removeOnly
	) {
		if (oldVnode === vnode) {
			return
		}

		if (isDef(vnode.elm) && isDef(ownerArray)) {
			// clone reused vnode
			vnode = ownerArray[index] = cloneVNode(vnode)
		}

		const elm = vnode.elm = oldVnode.elm

		if (isTrue(oldVnode.isAsyncPlaceholder)) {
			if (isDef(vnode.asyncFactory.resolved)) {
				hydrate(oldVnode.elm, vnode, insertedVnodeQueue)
			} else {
				vnode.isAsyncPlaceholder = true
			}
			return
		}

		// reuse element for static trees.
		// note we only do this if the vnode is cloned -
		// if the new node is not cloned it means the render functions have been
		// reset by the hot-reload-api and we need to do a proper re-render.
		if (isTrue(vnode.isStatic) &&
			isTrue(oldVnode.isStatic) &&
			vnode.key === oldVnode.key &&
			(isTrue(vnode.isCloned) || isTrue(vnode.isOnce))
		) {
			vnode.componentInstance = oldVnode.componentInstance
			return
		}

		let i
		const data = vnode.data
		if (isDef(data) && isDef(i = data.hook) && isDef(i = i.prepatch)) {
			i(oldVnode, vnode)
		}

		const oldCh = oldVnode.children
		const ch = vnode.children
		if (isDef(data) && isPatchable(vnode)) {
			for (i = 0; i < cbs.update.length; ++i) cbs.update[i](oldVnode, vnode)
			if (isDef(i = data.hook) && isDef(i = i.update)) i(oldVnode, vnode)
		}
		if (isUndef(vnode.text)) {
			if (isDef(oldCh) && isDef(ch)) {
				if (oldCh !== ch) updateChildren(elm, oldCh, ch, insertedVnodeQueue, removeOnly)
			} else if (isDef(ch)) {
				if (process.env.NODE_ENV !== 'production') {
					checkDuplicateKeys(ch)
				}
				if (isDef(oldVnode.text)) nodeOps.setTextContent(elm, '')
				addVnodes(elm, null, ch, 0, ch.length - 1, insertedVnodeQueue)
			} else if (isDef(oldCh)) {
				removeVnodes(oldCh, 0, oldCh.length - 1)
			} else if (isDef(oldVnode.text)) {
				nodeOps.setTextContent(elm, '')
			}
		} else if (oldVnode.text !== vnode.text) {
			nodeOps.setTextContent(elm, vnode.text)
		}
		if (isDef(data)) {
			if (isDef(i = data.hook) && isDef(i = i.postpatch)) i(oldVnode, vnode)
		}
	}

	function invokeInsertHook(vnode, queue, initial) {
		// delay insert hooks for component root nodes, invoke them after the
		// element is really inserted
		if (isTrue(initial) && isDef(vnode.parent)) {
			vnode.parent.data.pendingInsert = queue
		} else {
			for (let i = 0; i < queue.length; ++i) {
				queue[i].data.hook.insert(queue[i])
			}
		}
	}

	let hydrationBailed = false
	// list of modules that can skip create hook during hydration because they
	// are already rendered on the client or has no need for initialization
	// Note: style is excluded because it relies on initial clone for future
	// deep updates (#7063).
	const isRenderedModule = makeMap('attrs,class,staticClass,staticStyle,key')

	// Note: this is a browser-only function so we can assume elms are DOM nodes.
	function hydrate(elm, vnode, insertedVnodeQueue, inVPre) {
		let i
		const {tag, data, children} = vnode
		inVPre = inVPre || (data && data.pre)
		vnode.elm = elm

		if (isTrue(vnode.isComment) && isDef(vnode.asyncFactory)) {
			vnode.isAsyncPlaceholder = true
			return true
		}
		// assert node match
		if (process.env.NODE_ENV !== 'production') {
			if (!assertNodeMatch(elm, vnode, inVPre)) {
				return false
			}
		}
		if (isDef(data)) {
			if (isDef(i = data.hook) && isDef(i = i.init)) i(vnode, true /* hydrating */)
			if (isDef(i = vnode.componentInstance)) {
				// child component. it should have hydrated its own tree.
				initComponent(vnode, insertedVnodeQueue)
				return true
			}
		}
		if (isDef(tag)) {
			if (isDef(children)) {
				// empty element, allow client to pick up and populate children
				if (!elm.hasChildNodes()) {
					createChildren(vnode, children, insertedVnodeQueue)
				} else {
					// v-html and domProps: innerHTML
					if (isDef(i = data) && isDef(i = i.domProps) && isDef(i = i.innerHTML)) {
						if (i !== elm.innerHTML) {
							/* istanbul ignore if */
							if (process.env.NODE_ENV !== 'production' &&
								typeof console !== 'undefined' &&
								!hydrationBailed
							) {
								hydrationBailed = true
								console.warn('Parent: ', elm)
								console.warn('server innerHTML: ', i)
								console.warn('client innerHTML: ', elm.innerHTML)
							}
							return false
						}
					} else {
						// iterate and compare children lists
						let childrenMatch = true
						let childNode = elm.firstChild
						for (let i = 0; i < children.length; i++) {
							if (!childNode || !hydrate(childNode, children[i], insertedVnodeQueue, inVPre)) {
								childrenMatch = false
								break
							}
							childNode = childNode.nextSibling
						}
						// if childNode is not null, it means the actual childNodes list is
						// longer than the virtual children list.
						if (!childrenMatch || childNode) {
							/* istanbul ignore if */
							if (process.env.NODE_ENV !== 'production' &&
								typeof console !== 'undefined' &&
								!hydrationBailed
							) {
								hydrationBailed = true
								console.warn('Parent: ', elm)
								console.warn('Mismatching childNodes vs. VNodes: ', elm.childNodes, children)
							}
							return false
						}
					}
				}
			}
			if (isDef(data)) {
				let fullInvoke = false
				for (const key in data) {
					if (!isRenderedModule(key)) {
						fullInvoke = true
						invokeCreateHooks(vnode, insertedVnodeQueue)
						break
					}
				}
				if (!fullInvoke && data['class']) {
					// ensure collecting deps for deep class bindings for future updates
					traverse(data['class'])
				}
			}
		} else if (elm.data !== vnode.text) {
			elm.data = vnode.text
		}
		return true
	}

	function assertNodeMatch(node, vnode, inVPre) {
		if (isDef(vnode.tag)) {
			return vnode.tag.indexOf('vue-component') === 0 || (
				!isUnknownElement(vnode, inVPre) &&
				vnode.tag.toLowerCase() === (node.tagName && node.tagName.toLowerCase())
			)
		} else {
			return node.nodeType === (vnode.isComment ? 8 : 3)
		}
	}

	return function patch(oldVnode, vnode, hydrating, removeOnly) {
		if (isUndef(vnode)) {
			if (isDef(oldVnode)) invokeDestroyHook(oldVnode)
			return
		}

		let isInitialPatch = false
		const insertedVnodeQueue = []

		if (isUndef(oldVnode)) {
			// empty mount (likely as component), create new root element
			isInitialPatch = true
			createElm(vnode, insertedVnodeQueue)
		} else {
			const isRealElement = isDef(oldVnode.nodeType)
			if (!isRealElement && sameVnode(oldVnode, vnode)) {
				// patch existing root node
				patchVnode(oldVnode, vnode, insertedVnodeQueue, null, null, removeOnly)
			} else {
				if (isRealElement) {
					// mounting to a real element
					// check if this is server-rendered content and if we can perform
					// a successful hydration.
					if (oldVnode.nodeType === 1 && oldVnode.hasAttribute(SSR_ATTR)) {
						oldVnode.removeAttribute(SSR_ATTR)
						hydrating = true
					}
					if (isTrue(hydrating)) {
						if (hydrate(oldVnode, vnode, insertedVnodeQueue)) {
							invokeInsertHook(vnode, insertedVnodeQueue, true)
							return oldVnode
						} else if (process.env.NODE_ENV !== 'production') {
							warn(
								'The client-side rendered virtual DOM tree is not matching ' +
								'server-rendered content. This is likely caused by incorrect ' +
								'HTML markup, for example nesting block-level elements inside ' +
								'<p>, or missing <tbody>. Bailing hydration and performing ' +
								'full client-side render.'
							)
						}
					}
					// either not server-rendered, or hydration failed.
					// create an empty node and replace it
					oldVnode = emptyNodeAt(oldVnode)
				}

				// replacing existing element
				const oldElm = oldVnode.elm
				const parentElm = nodeOps.parentNode(oldElm)

				// create new node
				createElm(
					vnode,
					insertedVnodeQueue,
					// extremely rare edge case: do not insert if old element is in a
					// leaving transition. Only happens when combining transition +
					// keep-alive + HOCs. (#4590)
					oldElm._leaveCb ? null : parentElm,
					nodeOps.nextSibling(oldElm)
				)

				// update parent placeholder node element, recursively
				if (isDef(vnode.parent)) {
					let ancestor = vnode.parent
					const patchable = isPatchable(vnode)
					while (ancestor) {
						for (let i = 0; i < cbs.destroy.length; ++i) {
							cbs.destroy[i](ancestor)
						}
						ancestor.elm = vnode.elm
						if (patchable) {
							for (let i = 0; i < cbs.create.length; ++i) {
								cbs.create[i](emptyNode, ancestor)
							}
							// #6513
							// invoke insert hooks that may have been merged by create hooks.
							// e.g. for directives that uses the "inserted" hook.
							const insert = ancestor.data.hook.insert
							if (insert.merged) {
								// start at index 1 to avoid re-invoking component mounted hook
								for (let i = 1; i < insert.fns.length; i++) {
									insert.fns[i]()
								}
							}
						} else {
							registerRef(ancestor)
						}
						ancestor = ancestor.parent
					}
				}

				// destroy old node
				if (isDef(parentElm)) {
					removeVnodes([oldVnode], 0, 0)
				} else if (isDef(oldVnode.tag)) {
					invokeDestroyHook(oldVnode)
				}
			}
		}

		invokeInsertHook(vnode, insertedVnodeQueue, isInitialPatch)
		return vnode.elm
	}
}
```

```js
// createPatchFunction方法返回的方法
function patch(oldVnode, vnode, hydrating, removeOnly) {
	if (isUndef(vnode)) {
		if (isDef(oldVnode)) invokeDestroyHook(oldVnode)
		return
	}

	let isInitialPatch = false
	const insertedVnodeQueue = []

	if (isUndef(oldVnode)) {
		// empty mount (likely as component), create new root element
		isInitialPatch = true
		createElm(vnode, insertedVnodeQueue)
	} else {
		const isRealElement = isDef(oldVnode.nodeType)
		if (!isRealElement && sameVnode(oldVnode, vnode)) {
			// patch existing root node
			patchVnode(oldVnode, vnode, insertedVnodeQueue, null, null, removeOnly)
		} else {
			if (isRealElement) {
				// mounting to a real element
				// check if this is server-rendered content and if we can perform
				// a successful hydration.
				if (oldVnode.nodeType === 1 && oldVnode.hasAttribute(SSR_ATTR)) {
					oldVnode.removeAttribute(SSR_ATTR)
					hydrating = true
				}
				if (isTrue(hydrating)) {
					if (hydrate(oldVnode, vnode, insertedVnodeQueue)) {
						invokeInsertHook(vnode, insertedVnodeQueue, true)
						return oldVnode
					} else if (process.env.NODE_ENV !== 'production') {
						warn(
							'The client-side rendered virtual DOM tree is not matching ' +
							'server-rendered content. This is likely caused by incorrect ' +
							'HTML markup, for example nesting block-level elements inside ' +
							'<p>, or missing <tbody>. Bailing hydration and performing ' +
							'full client-side render.'
						)
					}
				}
				// either not server-rendered, or hydration failed.
				// create an empty node and replace it
				oldVnode = emptyNodeAt(oldVnode)
			}

			// replacing existing element
			const oldElm = oldVnode.elm
			const parentElm = nodeOps.parentNode(oldElm)

			// create new node
			createElm(
				vnode,
				insertedVnodeQueue,
				// extremely rare edge case: do not insert if old element is in a
				// leaving transition. Only happens when combining transition +
				// keep-alive + HOCs. (#4590)
				oldElm._leaveCb ? null : parentElm,
				nodeOps.nextSibling(oldElm)
			)

			// update parent placeholder node element, recursively
			if (isDef(vnode.parent)) {
				let ancestor = vnode.parent
				const patchable = isPatchable(vnode)
				while (ancestor) {
					for (let i = 0; i < cbs.destroy.length; ++i) {
						cbs.destroy[i](ancestor)
					}
					ancestor.elm = vnode.elm
					if (patchable) {
						for (let i = 0; i < cbs.create.length; ++i) {
							cbs.create[i](emptyNode, ancestor)
						}
						// #6513
						// invoke insert hooks that may have been merged by create hooks.
						// e.g. for directives that uses the "inserted" hook.
						const insert = ancestor.data.hook.insert
						if (insert.merged) {
							// start at index 1 to avoid re-invoking component mounted hook
							for (let i = 1; i < insert.fns.length; i++) {
								insert.fns[i]()
							}
						}
					} else {
						registerRef(ancestor)
					}
					ancestor = ancestor.parent
				}
			}

			// destroy old node
			if (isDef(parentElm)) {
				removeVnodes([oldVnode], 0, 0)
			} else if (isDef(oldVnode.tag)) {
				invokeDestroyHook(oldVnode)
			}
		}
	}

	invokeInsertHook(vnode, insertedVnodeQueue, isInitialPatch)
	return vnode.elm
}
```

> 源码: src/core/vdom/patch.js

## 执行更新组件方法mountComponent时实际调用vm._update(vm._render(), hydrating)的时候,先执行vm._render(),即renderMixin方法内部定义的Vue.prototype._render方法

```js
// 主要执行 vnode = render.call(vm._renderProxy, vm.$createElement), 其中render就是实例化Vue时传入的render方法,vm.$createElement就是render的回调方法h,
$createElement方法是在initRender方法里面扩展给vm的
Vue.prototype._render = function (): VNode {
	const vm: Component = this
	const {render, _parentVnode} = vm.$options

	if (_parentVnode) {
		vm.$scopedSlots = normalizeScopedSlots(
			_parentVnode.data.scopedSlots,
			vm.$slots,
			vm.$scopedSlots
		)
	}

	// set parent vnode. this allows render functions to have access
	// to the data on the placeholder node.
	vm.$vnode = _parentVnode
	// render self
	let vnode
	try {
		// There's no need to maintain a stack because all render fns are called
		// separately from one another. Nested component's render fns are called
		// when parent component is patched.
		currentRenderingInstance = vm
		vnode = render.call(vm._renderProxy, vm.$createElement)
	} catch (e) {
		handleError(e, vm, `render`)
		// return error render result,
		// or previous vnode to prevent render error causing blank component
		/* istanbul ignore else */
		if (process.env.NODE_ENV !== 'production' && vm.$options.renderError) {
			try {
				vnode = vm.$options.renderError.call(vm._renderProxy, vm.$createElement, e)
			} catch (e) {
				handleError(e, vm, `renderError`)
				vnode = vm._vnode
			}
		} else {
			vnode = vm._vnode
		}
	} finally {
		currentRenderingInstance = null
	}
	// if the returned array contains only a single node, allow it
	if (Array.isArray(vnode) && vnode.length === 1) {
		vnode = vnode[0]
	}
	// return empty vnode in case the render function errored out
	if (!(vnode instanceof VNode)) {
		if (process.env.NODE_ENV !== 'production' && Array.isArray(vnode)) {
			warn(
				'Multiple root nodes returned from render function. Render function ' +
				'should return a single root node.',
				vm
			)
		}
		vnode = createEmptyVNode()
	}
	// set parent
	vnode.parent = _parentVnode
	return vnode
}
```

> 源码: src/core/instance/render.js

## 执行vm._render(),实际执行的是createElement方法,createElement方法内部调用_createElement方法,_createElement内部主要通过调用

vnode = createComponent(Ctor, data, context, children, tag),它的内部通过Ctor = baseCtor.extend(Ctor)来生成子组件

```js
// 调用 _createElement(context, tag, data, children, normalizationType)
export function createElement(
	context: Component,
	tag: any,
	data: any,
	children: any,
	normalizationType: any,
	alwaysNormalize: boolean
): VNode | Array<VNode> {
	if (Array.isArray(data) || isPrimitive(data)) {
		normalizationType = children
		children = data
		data = undefined
	}
	if (isTrue(alwaysNormalize)) {
		normalizationType = ALWAYS_NORMALIZE
	}
	return _createElement(context, tag, data, children, normalizationType)
}
```

> 源码: src/core/vdom/create-element.js

```js
// 重点语句 vnode = createComponent(Ctor, data, context, children, tag)
export function _createElement(
	context: Component,
	tag?: string | Class<Component> | Function | Object,
	data?: VNodeData,
	children?: any,
	normalizationType?: number
): VNode | Array<VNode> {
	if (isDef(data) && isDef((data: any).__ob__)) {
		process.env.NODE_ENV !== 'production' && warn(
			`Avoid using observed data object as vnode data: ${JSON.stringify(data)}\n` +
			'Always create fresh vnode data objects in each render!',
			context
		)
		return createEmptyVNode()
	}
	// object syntax in v-bind
	if (isDef(data) && isDef(data.is)) {
		tag = data.is
	}
	if (!tag) {
		// in case of component :is set to falsy value
		return createEmptyVNode()
	}
	// warn against non-primitive key
	if (process.env.NODE_ENV !== 'production' &&
		isDef(data) && isDef(data.key) && !isPrimitive(data.key)
	) {
		if (!__WEEX__ || !('@binding' in data.key)) {
			warn(
				'Avoid using non-primitive value as key, ' +
				'use string/number value instead.',
				context
			)
		}
	}
	// support single function children as default scoped slot
	if (Array.isArray(children) &&
		typeof children[0] === 'function'
	) {
		data = data || {}
		data.scopedSlots = {default: children[0]}
		children.length = 0
	}
	if (normalizationType === ALWAYS_NORMALIZE) {
		children = normalizeChildren(children)
	} else if (normalizationType === SIMPLE_NORMALIZE) {
		children = simpleNormalizeChildren(children)
	}
	let vnode, ns
	if (typeof tag === 'string') {
		let Ctor
		ns = (context.$vnode && context.$vnode.ns) || config.getTagNamespace(tag)
		if (config.isReservedTag(tag)) {
			// platform built-in elements
			if (process.env.NODE_ENV !== 'production' && isDef(data) && isDef(data.nativeOn)) {
				warn(
					`The .native modifier for v-on is only valid on components but it was used on <${tag}>.`,
					context
				)
			}
			vnode = new VNode(
				config.parsePlatformTagName(tag), data, children,
				undefined, undefined, context
			)
		} else if ((!data || !data.pre) && isDef(Ctor = resolveAsset(context.$options, 'components', tag))) {
			// component
			vnode = createComponent(Ctor, data, context, children, tag)
		} else {
			// unknown or unlisted namespaced elements
			// check at runtime because it may get assigned a namespace when its
			// parent normalizes children
			vnode = new VNode(
				tag, data, children,
				undefined, undefined, context
			)
		}
	} else {
		// direct component options / constructor
		vnode = createComponent(tag, data, context, children)
	}
	if (Array.isArray(vnode)) {
		return vnode
	} else if (isDef(vnode)) {
		if (isDef(ns)) applyNS(vnode, ns)
		if (isDef(data)) registerDeepBindings(data)
		return vnode
	} else {
		return createEmptyVNode()
	}
}
```

> 源码: src/core/vdom/create-element.js

```js
// Ctor = baseCtor.extend(Ctor), extend用来生成子组件
// installComponentHooks(data), 子组件装载钩子方法 init|prepatch|insert|destroy
export function createComponent(
	Ctor: Class<Component> | Function | Object | void,
	data: ?VNodeData,
	context: Component,
	children: ?Array<VNode>,
	tag?: string
): VNode | Array<VNode> | void {
	if (isUndef(Ctor)) {
		return
	}

	const baseCtor = context.$options._base

	// plain options object: turn it into a constructor
	if (isObject(Ctor)) {
		Ctor = baseCtor.extend(Ctor)
	}

	// if at this stage it's not a constructor or an async component factory,
	// reject.
	if (typeof Ctor !== 'function') {
		if (process.env.NODE_ENV !== 'production') {
			warn(`Invalid Component definition: ${String(Ctor)}`, context)
		}
		return
	}

	// async component
	let asyncFactory
	if (isUndef(Ctor.cid)) {
		asyncFactory = Ctor
		Ctor = resolveAsyncComponent(asyncFactory, baseCtor)
		if (Ctor === undefined) {
			// return a placeholder node for async component, which is rendered
			// as a comment node but preserves all the raw information for the node.
			// the information will be used for async server-rendering and hydration.
			return createAsyncPlaceholder(
				asyncFactory,
				data,
				context,
				children,
				tag
			)
		}
	}

	data = data || {}

	// resolve constructor options in case global mixins are applied after
	// component constructor creation
	resolveConstructorOptions(Ctor)

	// transform component v-model data into props & events
	if (isDef(data.model)) {
		transformModel(Ctor.options, data)
	}

	// extract props
	const propsData = extractPropsFromVNodeData(data, Ctor, tag)

	// functional component
	if (isTrue(Ctor.options.functional)) {
		return createFunctionalComponent(Ctor, propsData, data, context, children)
	}

	// extract listeners, since these needs to be treated as
	// child component listeners instead of DOM listeners
	const listeners = data.on
	// replace with listeners with .native modifier
	// so it gets processed during parent component patch.
	data.on = data.nativeOn

	if (isTrue(Ctor.options.abstract)) {
		// abstract components do not keep anything
		// other than props & listeners & slot

		// work around flow
		const slot = data.slot
		data = {}
		if (slot) {
			data.slot = slot
		}
	}

	// install component management hooks onto the placeholder node
	installComponentHooks(data)

	// return a placeholder vnode
	const name = Ctor.options.name || tag
	const vnode = new VNode(
		`vue-component-${Ctor.cid}${name ? `-${name}` : ''}`,
		data, undefined, undefined, undefined, context,
		{Ctor, propsData, listeners, tag, children},
		asyncFactory
	)

	// Weex specific: invoke recycle-list optimized @render function for
	// extracting cell-slot template.
	// https://github.com/Hanks10100/weex-native-directive/tree/master/component
	/* istanbul ignore if */
	if (__WEEX__ && isRecyclableComponent(vnode)) {
		return renderRecyclableComponentTemplate(vnode)
	}

	return vnode
}
```

> 源码: src/core/vdom/create-component.js

```js
// 组件钩子方法
const componentVNodeHooks = {
	init(vnode: VNodeWithData, hydrating: boolean): ?boolean {
		if (
			vnode.componentInstance &&
			!vnode.componentInstance._isDestroyed &&
			vnode.data.keepAlive
		) {
			// kept-alive components, treat as a patch
			const mountedNode: any = vnode // work around flow
			componentVNodeHooks.prepatch(mountedNode, mountedNode)
		} else {
			const child = vnode.componentInstance = createComponentInstanceForVnode(
				vnode,
				activeInstance
			)
			child.$mount(hydrating ? vnode.elm : undefined, hydrating)
		}
	},

	prepatch(oldVnode: MountedComponentVNode, vnode: MountedComponentVNode) {
		const options = vnode.componentOptions
		const child = vnode.componentInstance = oldVnode.componentInstance
		updateChildComponent(
			child,
			options.propsData, // updated props
			options.listeners, // updated listeners
			vnode, // new parent vnode
			options.children // new children
		)
	},

	insert(vnode: MountedComponentVNode) {
		const {context, componentInstance} = vnode
		if (!componentInstance._isMounted) {
			componentInstance._isMounted = true
			callHook(componentInstance, 'mounted')
		}
		if (vnode.data.keepAlive) {
			if (context._isMounted) {
				// vue-router#1212
				// During updates, a kept-alive component's child components may
				// change, so directly walking the tree here may call activated hooks
				// on incorrect children. Instead we push them into a queue which will
				// be processed after the whole patch process ended.
				queueActivatedComponent(componentInstance)
			} else {
				activateChildComponent(componentInstance, true /* direct */)
			}
		}
	},

	destroy(vnode: MountedComponentVNode) {
		const {componentInstance} = vnode
		if (!componentInstance._isDestroyed) {
			if (!vnode.data.keepAlive) {
				componentInstance.$destroy()
			} else {
				deactivateChildComponent(componentInstance, true /* direct */)
			}
		}
	}
}
```

> 源码: src/core/vdom/create-component.js

```js
// 生成子组件构造函数
Vue.extend = function (extendOptions: Object): Function {
	extendOptions = extendOptions || {}
	const Super = this
	const SuperId = Super.cid
	const cachedCtors = extendOptions._Ctor || (extendOptions._Ctor = {})
	if (cachedCtors[SuperId]) {
		return cachedCtors[SuperId]
	}

	const name = extendOptions.name || Super.options.name
	if (process.env.NODE_ENV !== 'production' && name) {
		validateComponentName(name)
	}

	const Sub = function VueComponent(options) {
		this._init(options)
	}
	Sub.prototype = Object.create(Super.prototype)
	Sub.prototype.constructor = Sub
	Sub.cid = cid++
	Sub.options = mergeOptions(
		Super.options,
		extendOptions
	)
	Sub['super'] = Super

	// For props and computed properties, we define the proxy getters on
	// the Vue instances at extension time, on the extended prototype. This
	// avoids Object.defineProperty calls for each instance created.
	if (Sub.options.props) {
		initProps(Sub)
	}
	if (Sub.options.computed) {
		initComputed(Sub)
	}

	// allow further extension/mixin/plugin usage
	Sub.extend = Super.extend
	Sub.mixin = Super.mixin
	Sub.use = Super.use

	// create asset registers, so extended classes
	// can have their private assets too.
	ASSET_TYPES.forEach(function (type) {
		Sub[type] = Super[type]
	})
	// enable recursive self-lookup
	if (name) {
		Sub.options.components[name] = Sub
	}

	// keep a reference to the super options at extension time.
	// later at instantiation we can check if Super's options have
	// been updated.
	Sub.superOptions = Super.options
	Sub.extendOptions = extendOptions
	Sub.sealedOptions = extend({}, Sub.options)

	// cache constructor
	cachedCtors[SuperId] = Sub
	return Sub
}
```

> 源码: src/core/global-api/extend.js

## 调用patch方法时,会调用`createElm(vnode, insertedVnodeQueue,oldElm._leaveCb ? null : parentElm, nodeOps.nextSibling(oldElm))`方法

```js
// 调用createComponent(vnode, insertedVnodeQueue, parentElm, refElm)挂载子组件
function createElm(
	vnode,
	insertedVnodeQueue,
	parentElm,
	refElm,
	nested,
	ownerArray,
	index
) {
	if (isDef(vnode.elm) && isDef(ownerArray)) {
		// This vnode was used in a previous render!
		// now it's used as a new node, overwriting its elm would cause
		// potential patch errors down the road when it's used as an insertion
		// reference node. Instead, we clone the node on-demand before creating
		// associated DOM element for it.
		vnode = ownerArray[index] = cloneVNode(vnode)
	}

	vnode.isRootInsert = !nested // for transition enter check
	if (createComponent(vnode, insertedVnodeQueue, parentElm, refElm)) {
		return
	}

	const data = vnode.data
	const children = vnode.children
	const tag = vnode.tag
	if (isDef(tag)) {
		if (process.env.NODE_ENV !== 'production') {
			if (data && data.pre) {
				creatingElmInVPre++
			}
			if (isUnknownElement(vnode, creatingElmInVPre)) {
				warn(
					'Unknown custom element: <' + tag + '> - did you ' +
					'register the component correctly? For recursive components, ' +
					'make sure to provide the "name" option.',
					vnode.context
				)
			}
		}

		vnode.elm = vnode.ns
			? nodeOps.createElementNS(vnode.ns, tag)
			: nodeOps.createElement(tag, vnode)
		setScope(vnode)

		/* istanbul ignore if */
		if (__WEEX__) {
			// in Weex, the default insertion order is parent-first.
			// List items can be optimized to use children-first insertion
			// with append="tree".
			const appendAsTree = isDef(data) && isTrue(data.appendAsTree)
			if (!appendAsTree) {
				if (isDef(data)) {
					invokeCreateHooks(vnode, insertedVnodeQueue)
				}
				insert(parentElm, vnode.elm, refElm)
			}
			createChildren(vnode, children, insertedVnodeQueue)
			if (appendAsTree) {
				if (isDef(data)) {
					invokeCreateHooks(vnode, insertedVnodeQueue)
				}
				insert(parentElm, vnode.elm, refElm)
			}
		} else {
			createChildren(vnode, children, insertedVnodeQueue)
			if (isDef(data)) {
				invokeCreateHooks(vnode, insertedVnodeQueue)
			}
			insert(parentElm, vnode.elm, refElm)
		}

		if (process.env.NODE_ENV !== 'production' && data && data.pre) {
			creatingElmInVPre--
		}
	} else if (isTrue(vnode.isComment)) {
		vnode.elm = nodeOps.createComment(vnode.text)
		insert(parentElm, vnode.elm, refElm)
	} else {
		vnode.elm = nodeOps.createTextNode(vnode.text)
		insert(parentElm, vnode.elm, refElm)
	}
}
```

> 源码: src/core/vdom/patch.js

```js
// 挂载子组件
function createComponent(vnode, insertedVnodeQueue, parentElm, refElm) {
	let i = vnode.data
	if (isDef(i)) {
		const isReactivated = isDef(vnode.componentInstance) && i.keepAlive
		if (isDef(i = i.hook) && isDef(i = i.init)) {
			i(vnode, false /* hydrating */)
		}
		// after calling the init hook, if the vnode is a child component
		// it should've created a child instance and mounted it. the child
		// component also has set the placeholder vnode's elm.
		// in that case we can just return the element and be done.
		if (isDef(vnode.componentInstance)) {
			initComponent(vnode, insertedVnodeQueue)
			insert(parentElm, vnode.elm, refElm)
			if (isTrue(isReactivated)) {
				reactivateComponent(vnode, insertedVnodeQueue, parentElm, refElm)
			}
			return true
		}
	}
}
```

**大致流程**

```js

var app = new Vue({
	render: h => h(App)
})

app.$mount('#app')

mountComponent(this, el, hydrating)

new Watcher(vm, updateComponent, noop, {
	before() {
		if (vm._isMounted && !vm._isDestroyed) { // 更新组件之前调用beforeUpdate钩子方法
			callHook(vm, 'beforeUpdate')
		}
	}
}, true /* isRenderWatcher */)

updateComponent()

vm._render()

vnode = render.call(vm._renderProxy, vm.$createElement)

createElement(context, tag, data, children, normalizationType, alwaysNormalize)

_createElement(context, tag, data, children, normalizationType)

vnode = createComponent(tag, data, context, children)

installComponentHooks(data)

const vnode = new VNode(
	`vue-component-${Ctor.cid}${name ? `-${name}` : ''}`, data, undefined, undefined, undefined, context, {
		Ctor, propsData,
		listeners, tag, children
	}, asyncFactory
)

vm._update(vm._render(), hydrating)

vm.$el = vm.__patch__(vm.$el, vnode, hydrating, false /* removeOnly */)

patch(oldVnode, vnode, hydrating, removeOnly)

oldVnode = emptyNodeAt(oldVnode)

createElm(
	vnode, insertedVnodeQueue, oldElm._leaveCb ? null : parentElm, nodeOps.nextSibling(oldElm)
)

createComponent(vnode, insertedVnodeQueue, parentElm, refElm)

if (isDef(i = i.hook) && isDef(i = i.init)) {
	i(vnode, false /* hydrating */);
}

var child = vnode.componentInstance = createComponentInstanceForVnode(vnode, activeInstance)

new vnode.componentOptions.Ctor(options)

var Sub = function VueComponent(options) {
	this._init(options);
}

child.$mount(hydrating ? vnode.elm : undefined, hydrating)

mountComponent(this, el, hydrating)

new Watcher(vm, updateComponent, noop, {
	before: function before() {
		if (vm._isMounted && !vm._isDestroyed) {
			callHook(
				vm, 'beforeUpdate');
		}
	}
}, true /* isRenderWatcher */)

updateComponent()

vm._render()

vnode = render.call(vm._renderProxy, vm.$createElement)

vnode = new VNode(
	config.parsePlatformTagName(tag), data, children, undefined, undefined, context
)

```

**Vue导入-->实例化-->挂载组件流程完毕**

