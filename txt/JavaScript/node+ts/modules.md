# Modules

In Node.js, the module system is an implementation of the **CommonJS** format. Eeach file is treated as a separate module. Under the hood, Node.js wraps them in a function like this:

```js
function (exports, require, module, __filename, __dirname) {
  // code of the module
}
```

## Exporting and importing

### Export
```js
// module.js
function a() { ... }

module.exports = { a };
```

### Import
```js
// main.js
const { a } = require('./module.js');
const m = require('.module.js');

a();
m.a();
```

## `this` in modules

Node.js invokes the function that wraps our module a way, that the `this` keyword references to the `module.exports`. We can easily prove that:

```js
console.log(this === module.exports); // true
```

The above is often a cause of confusion because if you are running Node.js in a console, the `this` keyword references to the `global` object.

