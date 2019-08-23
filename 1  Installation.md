# Installation

## Direct Download/CDN

<https://unpkg.com/vuex>
Unpkg.com provides NPM-based CDN links. The above link will always point to the latest release on NPM. You can also use a specific version/tag via URLs like
`https://unpkg.com/vuex@2.0.0.`

Include `vuex` after Vue and it will install itself automatically:

```html
<script src="/path/to/vue.js"></script>
<script src="/path/to/vuex.js"></script>
```

## NPM

```sh
npm install vuex --save
```

## Yarn

```sh
yarn add vuex
```

When used with a module system, you must explicitly install Vuex via `Vue.use()`:

```js
import Vue from 'vue'
import Vuex from 'vuex'

Vue.use(Vuex)
```

You don't need to do this when using global script tags.

## Promise

Vuex requires `Promise` . If your supporting browsers do not implement Promise (e.g. IE), you can use a polyfill library, such as `es6-promise` .

You can include it via CDN:

```html
<script src="https://cdn.jsdelivr.net/npm/es6-promise@4/dist/es6-promise.auto.js"></script>
```

Then `window.Promise` will be available automatically.

If you prefer using a package manager such as NPM or Yarn, install it with the following commands:

```sh
npm install es6-promise --save # NPM
yarn add es6-promise # Yarn
```

Furthermore, add the below line into anywhere in your code before using Vuex:

```js
import 'es6-promise/auto'
```

## Dev Build

You will have to clone directly from GitHub and build `vuex` yourself if you want to use the latest dev build.

```sh
git clone https://github.com/vuejs/vuex.git node_modules/vuex
cd node_modules/vuex
npm install
npm run build
```

