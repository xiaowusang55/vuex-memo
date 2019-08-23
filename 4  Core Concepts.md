# Core Concepts

## State

Vuex uses a **single state tree** - that is, this single object contains all your application level state and serves as the "single source of truth". This also means usually you will have only one store for each application. A single state tree makes it straightforward to locate a specific piece of state, and allows us to easily take snapshots of the current app state for debugging purposes.

The single state tree does not conflict with modularity - in later chapters we will discuss how to split your state and mutations into sub modules.

The data you store in Vuex follows the same rules as the `data` in a Vue instance, ie the state object must be plain. See also: `Vue#data`.

### Getting Vuex State into Vue Components

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

### The `mapState` Helper

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

### Object Spread Operator

Note that mapState returns an object. How do we use it in combination with other local computed properties? Normally, we'd have to use a utility to merge multiple objects into one so that we can pass the final object to computed. However with the `object spread operator` (which is a stage-4 ECMAScript proposal), we can greatly simplify the syntax:

```js
computed: {
  localComputed () { /* ... */ },
  // mix this into the outer object with the object spread operator
  ...mapState({
    // ...
  })
}
```

## Components Can Still hava Local State

Using Vuex doesn't mean you should put all the state in Vuex. Although putting more state into Vuex makes your state mutations more explicit and debuggable, sometimes it could also make the code more verbose and indirect. If a piece of state strictly belongs to a single component, it could be just fine leaving it as local state. You should weigh the trade-offs and make decisions that fit the development needs of your app.

## Getters

Sometimes we may need to compute derived state based on store state, for example filtering through a list of items and counting them:

```js
computed: {
    doneTodosCount () {
        return this.$store.state.todos.filter(todo => todo.done).length
    }
}
```

If more than one component needs to make use of this, we have to either duplicate the function, or extract it into a shared helper and import it in multiple places - both are less than ideal.

Vuex allows us to define "getters" in the store. You can think of them as computed properties for stores. Like computed properties, a getter's result is cached based on its dependencies, and will only re-evaluate when some of its dependencies have changed.

Getters will receive the state as their 1st argument:

```js
const store =  new Vuex.Store({
    state: {
        todos: [
            {
                id: 1, text: '...', done: true
            },
            {
                id: 2, text: '...', done: false
            }
        ]
    },
    getters: [
        doneTodos: state => {
            return state.todos.filter(todo => todo.done)
        }
    ]
})
```

### Property-Style Access

The getters will be exposed on the `store.getters` object, and you access values as properties:

```js
store.getters.doneTodos //-> [{ id: 1, text: '...', done: true }]
```

Getters will also receive other getters as the 2nd argument:

```js
getters: {
    //...
    doneTodosCount: (state, getters) => {
        return getters.doneTodos.length
    }
}
```

```js
store.getters.doneTodosCount // -> 1
```

We can now easily make use of it inside any component:

```js
computed: {
  doneTodosCount () {
    return this.$store.getters.doneTodosCount
  }
}
```

>***Note that getters accessed as properties are cached as part of Vue's reactivity system.***

### Method-Style Access

You can also pass arguments to getters by returning a function. This is particularly useful when you want to query an array in the store:

```js
getters: {
    //...
    getTodoById: (state) => (id) => {
        return state.todos.find(todo => todo.id === id)
    }
}
```

```js
store.getters.getTodoById(2) //-> { id: 2, text: '...', done: false }
```

>***Note that getters accessed via methods will run each time you call them, and the result is not cached.***

### The `mapGetters` Helper

The `mapGetters` helper simply maps store getters to local computed properties:

```js
import { mapGetters } from 'vuex'

export default {
  // ...
  computed: {
    // mix the getters into computed with object spread operator
    ...mapGetters([
      'doneTodosCount',
      'anotherGetter',
      // ...
    ])
  }
}

```

If you want to map a getter to a different name, use an object:

```js
...mapGetters({
  // map `this.doneCount` to `this.$store.getters.doneTodosCount`
  doneCount: 'doneTodosCount'
})

```