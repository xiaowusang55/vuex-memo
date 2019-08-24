# Vuex.Store Instance Methods

## commit

* commit(`type: string`, `payload?: any`, `options?: Object`)
* commit(`mutation: Object`, `options?: Object`)

Commit a mutation. `options` can have `root: true` that allows to commit root mutations **in namespaced modules**.

## dispatch

* dispatch(`type: string`, `payload?: any`, `options?: Object`)
* dispatch(`action: Object`, `options?: Object`)

Dispatch an action. options can have root: true that allows to dispatch root actions in **namespaced modules**. Returns a Promise that resolves all triggered action handlers.

## replaceState

* `replaceState(state: Object)`

Replace the store's root state. Use this only for state hydration / time-travel purposes.

## watch

* watch(`fn: Function`, `callback: Function`, `options?: Object`): `Function`

Reactively watch `fn`'s return value, and call the callback when the value changes. `fn` receives the store's state as the first argument, and getters as the second argument. Accepts an optional options object that takes the same options as `Vue's` `vm.$watch` method.

## subscribe

* subscribe(`handler: Function`):`Function`

Subscribe to store mutations. The handler is called after every mutation and receives the mutation descriptor and post-mutation state as arguments:

```js
store.subscribe((mutations, state) => {
    console.log(mutation.type)
    console.log(mutation.payload)
})
```

To stop subscribing, call the returned unsubscribe function.Most commonly used in plugins.

## subscribeAction

* subscribeAction(`handler: Function`): `Function`

***New in 2.5.0***

Subscribe to store actions. The `handler` is called for every dispatched action and receives the action descriptor and current store state as arguments:

```js
store.subscribeAction((action, state) => {
    console.log(action.type)
    console.log(action.payload)
})
```

To stop subscribing, call the returned unsubscribe function.

***New in 3.1.0***

```js
store.subscribeAction({
  before: (action, state) => {
    console.log(`before action ${action.type}`)
  },
  after: (action, state) => {
    console.log(`after action ${action.type}`)
  }
})
```

Most commonly used in plugins.

## registerModule

* registerModule(`path: string | Array<string>`, `module: Module`, `option?: Object`)

Register a dynamic module.

options can have preserveState: true that allows to preserve the previous state. Useful with Server Side Rendering.

## unregisterModule

* unregisterModule(path: string | Array<string>)

Unregister a dynamic module.

## hotUpdate

* hotUpdate(`newOptions: Object`)

Hot swap new actions and mutaions.

