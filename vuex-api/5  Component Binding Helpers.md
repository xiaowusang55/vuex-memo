# Component Binding Helpers

## mapState

* mapState(`namespace?: string`, `map: Array<string> | Object<string | function>`) : `Object`

Create component computed options that return the sub tree of the Vuex store.

The first argument can optionally be a namespace string.
The second object argument's members can be a function. function(state: any)

## mapGetters

* mapGetters(`namespace?: string`, `map: Array<string> | Object<string>`) : `Object`

Create component computed options that return the evaluated value of a getter.
The first argument can optionally be a namespace string.

## mapActions

* mapActions`namespace?: string`, `map: Array<string> | Object<string | function>`) : `Object`

Create component methods options that dispatch an action.
The first argument can optionally be a namespace string.
The second object argument's members can be a function. `function(dispatch: function, ...args: any[])`

## mapMutations

* mapMutation`namespace?: string`, `map: Array<string> | Object<string | function>`) : `Object`

Create component methods options that commit a mutation.
he first argument can optionally be a namespace string.
he second object argument's members can be a function. `function(commit: function, ...args: any[])`

## createNamespacedHelpers

* createNamespacedHelper(`namespace: string`): `Object`

Create namespaced component binding helpers. The returned object contains `mapState`, `mapGetters`, `mapActions` and `mapMutations` that are bound with the given namespace.
