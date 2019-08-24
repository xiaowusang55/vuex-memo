# Vuex.Store Constructor Options

## state

* type: `Object` | `Function`

The root state object for the Vuex store.

If you pass a function that returns an object, the returned object is used as the root state. This is useful when you want to reuse the state object especially for module reuse.

## mutations

* type: { `[type: string]`: `Function` }

Register mutations on the store. The handler function always receives `state` as the first argument (will be module local state if defined in a module), and receives a second payload argument if there is one.

## actions

* type: { `[type: string]` : `Function` }

Register actions on the store. The handler function receives a context object that exposes the following properties:

```js
{
  state,      // same as `store.state`, or local state if in modules
  rootState,  // same as `store.state`, only in modules
  commit,     // same as `store.commit`
  dispatch,   // same as `store.dispatch`
  getters,    // same as `store.getters`, or local getters if in modules
  rootGetters // same as `store.getters`, only in modules
}
```

And also receives a second `payload` argument if there is one.

## getters

* type: {`[key:string]`:` Function` }

Register getters on the store. The getter function receives the following arguments:

```js
state,     // will be module local state if defined in a module.
getters    // same as store.getters
```

Specific when defined in a module

```js
state,       // will be module local state if defined in a module.
getters,     // module local getters of the current module
rootState,   // global state
rootGetters  // all getters
```

Registered getters are exposed on store.getters.

## modules

* type: `Object`

An object containing sub modules to be merged into the store, in the shape of:

```js
{
  key: {
    state,
    namespaced?,
    mutations?,
    actions?,
    getters?,
    modules?
  },
  ...
}
```

Each module can contain state and mutations similar to the root options. A module's state will be attached to the store's root state using the module's key. A module's mutations and getters will only receives the module's local state as the first argument instead of the root state, and module actions' context.state will also point to the local state.

## plugins

* type: `Array<Function>`

An array of plugin functions to be applied to the store. The plugin simply receives the store as the only argument and can either listen to mutations (for outbound data persistence, logging, or debugging) or dispatch mutations (for inbound data e.g. websockets or observables).

## Strict

* type: `Boolean`
* default: `false`

Force the Vuex store into strict mode. In strict mode any mutations to Vuex state outside of mutation handlers will throw an Error.

## devtools

* type: `Boolean`

Turn the devtools on or off for a particular vuex instance. For instance passing false tells the Vuex store to not subscribe to devtools plugin. Useful for if you have multiple stores on a single page.

```js
{
    devtools: false
}
```