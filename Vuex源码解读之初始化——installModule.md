#### 解读目标
> 搞明白上文末调用的两个方法，一个是installModule方法，另外一个是resetStoreVM方法，这一篇先解读installModule方法

#### installModule方法
installModule函数可接收5个参数，store、rootState、path、module、hot，store代表当前Store实例，rootStore代表根state，path表示当前嵌套模块的路径数组，module表示当前安装的模块，hot当动态改变modules或者热更新的时候为true。
```
function installModule (store, rootState, path, module, hot) {
  const isRoot = !path.length
  const namespace = store._modules.getNamespace(path)

  // register in namespace map
  if (module.namespaced) {
    store._modulesNamespaceMap[namespace] = module
  }

  // set state
  if (!isRoot && !hot) {
    const parentState = getNestedState(rootState, path.slice(0, -1))
    const moduleName = path[path.length - 1]
    store._withCommit(() => {
      Vue.set(parentState, moduleName, module.state)
    })
  }

  const local = module.context = makeLocalContext(store, namespace, path)

  module.forEachMutation((mutation, key) => {
    const namespacedType = namespace + key
    registerMutation(store, namespacedType, mutation, local)
  })

  module.forEachAction((action, key) => {
    const type = action.root ? key : namespace + key
    const handler = action.handler || action
    registerAction(store, type, handler, local)
  })

  module.forEachGetter((getter, key) => {
    const namespacedType = namespace + key
    registerGetter(store, namespacedType, getter, local)
  })

  module.forEachChild((child, key) => {
    installModule(store, rootState, path.concat(key), child, hot)
  })
}
```
代码首先通过path数组来判断是否为根。上面的构造函数中调用时为 installModule(this, state, [], this._modules.root) ，这里的isRoot为true。

```
const isRoot = !path.length
```
接着判断当不为根且非热更新的情况，然后设置级联状态。

```
// set state
if (!isRoot && !hot) {
    const parentState = getNestedState(rootState, path.slice(0, -1))
    const moduleName = path[path.length - 1]
    store._withCommit(() => {
      Vue.set(parentState, moduleName, module.state)
    })
}
```
不为根也就是说在初始化子模块的时候，isRoot为false，进入条件之后调用了getNestedState方法
```
function getNestedState (state, path) {
  return path.length
    ? path.reduce((state, key) => state[key], state)
    : state
}
```
根据path查找state上的嵌套state。最后调用了_withCommit方法。

接着下面调用了makeLocalContext方法，作用是根据是否开启命名空间来获取上下文，接下来会分别处理module中的mutation、action、getter，分别调用它们的方法进行注册。
```
const local = module.context = makeLocalContext(store, namespace, path)

module.forEachMutation((mutation, key) => {
    const namespacedType = namespace + key
    registerMutation(store, namespacedType, mutation, local)
})

module.forEachAction((action, key) => {
    const type = action.root ? key : namespace + key
    const handler = action.handler || action
    registerAction(store, type, handler, local)
})

module.forEachGetter((getter, key) => {
    const namespacedType = namespace + key
    registerGetter(store, namespacedType, getter, local)
})
```
看看比较重要的这三个方法吧——
1. registerMutation方法，处理mutation
```
function registerMutation (store, type, handler, local) {
  const entry = store._mutations[type] || (store._mutations[type] = [])
  entry.push(function wrappedMutationHandler (payload) {
    handler.call(store, local.state, payload)
  })
}
```
registerMutation是对store的mutation的初始化，参数依次为当前Store实例，模块名称，mutation执行的回调函数和当前模块的路径。mutation的**作用就是同步修改当前模块的state，函数首先通过模块拿到对应的mutation对象数组，然后把一个mutation的包装函数push到数组里，这个函数接收payload作为额外参数，这个函数在执行的时候会调用mutation的回调函数，并将实例对象store，当前模块的state和payload一起作为回调函数的参数。**

再来看看mutation的回调函数的机制，mutation的调用是通过store实例的API接口commit来调用的

```
commit (_type, _payload, _options) {
    // check object-style commit
    const {
      type,
      payload,
      options
    } = unifyObjectStyle(_type, _payload, _options)

    const mutation = { type, payload }
    const entry = this._mutations[type]
    if (!entry) {
      if (process.env.NODE_ENV !== 'production') {
        console.error(`[vuex] unknown mutation type: ${type}`)
      }
      return
    }
    this._withCommit(() => {
      entry.forEach(function commitIterator (handler) {
        handler(payload)
      })
    })
    this._subscribers.forEach(sub => sub(mutation, this.state))

    if (
      process.env.NODE_ENV !== 'production' &&
      options && options.silent
    ) {
      console.warn(
        `[vuex] mutation type: ${type}. Silent option has been removed. ` +
        'Use the filter functionality in the vue-devtools'
      )
    }
}
```
commit支持3个参数，_type表示mutation的类型，_payload表示额外的参数，_options表示一些配置

2. registerAction方法，处理action

```
function registerAction (store, type, handler, local) {
  const entry = store._actions[type] || (store._actions[type] = [])
  entry.push(function wrappedActionHandler (payload, cb) {
    let res = handler.call(store, {
      dispatch: local.dispatch,
      commit: local.commit,
      getters: local.getters,
      state: local.state,
      rootGetters: store.getters,
      rootState: store.state
    }, payload, cb)
    if (!isPromise(res)) {
      res = Promise.resolve(res)
    }
    if (store._devtoolHook) {
      return res.catch(err => {
        store._devtoolHook.emit('vuex:error', err)
        throw err
      })
    } else {
      return res
    }
  })
}
```
registerAction是对store的action的初始化，它和registerMutation的参数一致，和mutation不同一点，mutation是同步修改当前模块的state，而action是可以异步去修改state。在action的回调中并不会直接修改state，仍然是通过提交一个mutation去修改state（在Vuex中，mutation是修改state的唯一途径）。

action 的回调函数的调用时机，在Vuex中，action的调用是通过store实例的API接口dispatch来调用的

```
dispatch (_type, _payload) {
    // check object-style dispatch
    const {
      type,
      payload
    } = unifyObjectStyle(_type, _payload)

    const action = { type, payload }
    const entry = this._actions[type]
    if (!entry) {
      if (process.env.NODE_ENV !== 'production') {
        console.error(`[vuex] unknown action type: ${type}`)
      }
      return
    }

    this._actionSubscribers.forEach(sub => sub(action, this.state))

    return entry.length > 1
      ? Promise.all(entry.map(handler => handler(payload)))
      : entry[0](payload)
  }
```

3. registerGetter方法，处理getter

```
function registerGetter (store, type, rawGetter, local) {
  if (store._wrappedGetters[type]) {
    if (process.env.NODE_ENV !== 'production') {
      console.error(`[vuex] duplicate getter key: ${type}`)
    }
    return
  }
  store._wrappedGetters[type] = function wrappedGetter (store) {
    return rawGetter(
      local.state, // local state
      local.getters, // local getters
      store.state, // root state
      store.getters // root getters
    )
  }
}
```



因为Vuex本身是单一状态树，如果所有的状态都包含在一个大对象里面，Store会变得越来越臃肿，所以Vuex提供了module，允许我们把store分为module，每一个模块包含各自的state、mutations、actions和getters，甚至嵌套模块。所以有了下面的代码，通过遍历module中的子模块，递归调用installModule去安装子模块。参数里面传进去的path不为空，module对应为子模块
```
module.forEachChild((child, key) => {
    installModule(store, rootState, path.concat(key), child, hot)
})
```


看上面注释大概了解，installModule方法主要实现的是初始化module，递归注册所有的子module以及收集this._wrappedGetters中的所有模块获取器，

