# State

Vuex uses a **single state tree** - that is, this single object contains all your application level state and serves as the "single source of truth". This also means usually you will have only one store for each application. A single state tree makes it straightforward to locate a specific piece of state, and allows us to easily take snapshots of the current app state for debugging purposes.

The single state tree does not conflict with modularity - in later chapters we will discuss how to split your state and mutations into sub modules.

The data you store in Vuex follows the same rules as the `data` in a Vue instance, ie the state object must be plain. See also: `Vue#data`.

## Getting Vuex State into Vue Components

So how do we display state inside the store in our Vue components? Since Vuex stores are reactive, the simplest way to "retrieve" state from it is simply returning some store state from within a `computed property` :

```js
//let's create a Counter component
const Counter = {
    tempalte:`
        <div>{{ count }}</div>
    `,
    computed: {
        count () {
            return store.state.count
        }
    }

}
```

Whenever `store.state.count` changes, it will cause the computed property to re-evaluate, and trigger associated DOM updates.

```js
const app = new Vue({
    el: '#app',
    // provide the store using the "store" option.
    // this will inject the store instance to all child components.
    store,
    components: { Counter },
    template: `
        <div class="app">
            <counter></counter>
        </div>
    `
})
```

By providing the `store` option to the root instance, the store will be injected into all child components of the root and will be available on them as `this.$store`. Let's update our `Counter` implementation:

```js
const Counter = {
    template: `
        <div>{{ count }}</div>
    `,
    computed: {
        count () {
            return this.$store.state.count
        }
    }
}
```

## The `mapState` Helper

When a component needs to make use of multiple store state properties or getters, declaring all these computed properties can get repetitive and verbose. To deal with this we can make use of the `mapState` helper which generates computed getter functions for us, saving us some keystrokes:

```js
// in full builds helpers are exposed as Vuex.mapState
import { mapState } from 'vuex'

export default {
    //...
    computed: mapState({
        // arrow functions can make the code very succinct!
        count: state => state.count,

        // passing the string value 'count' is same as `state => state.count`
        countAlias: 'count',

        // to access local state with `this`, a normal function must be used
        countPlusLocalState (state) {
            return state.count + this.localCount
        }
    })
}
```

We can also pass a string array to `mapState` when the name of a mapped computed property is the same as a state sub tree name.

```js
computed: mapState([
  // map this.count to store.state.count
  'count'
])
```

