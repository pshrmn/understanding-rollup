# What is Rollup?

[Rollup](https://github.com/rollup/rollup) is a module bundler. It takes all of the modules in your project and combines them into one file.

Basically, you can start with a project that has three modules:

```js
// index.js
import one from './one';
import two from './two';

console.log(one + two);
```

```js
// one.js
export default 1;
```

```js
// two.js
export default 2;
```

And Rollup will give you a single file:

```js
'use strict';

var one = 1;

var two = 2;

console.log(one + two);
```

**Note:** The above code uses `import`/`export` and not `require()` and `module.exports`. Rollup expects the modules that it uses to use ES2015 imports/exports so that it can perform static analysis of your code (basically, to determine what code is necessary in your application).

For each module file in your project, Rollup will create a `Module` object and analyze the contents of the file. Next, let's take a look at [how Rollup creates Modules](./01-how-does-rollup-create-modules.md).
