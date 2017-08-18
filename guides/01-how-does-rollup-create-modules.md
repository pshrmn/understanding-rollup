# Creating Modules

For every module in your project, Rollup will use its loaders to read the contents of the file and create a `Module`. The two most important steps in constructing a `Module` object are parsing the file and analyzing the results

## Parsing

Rollup relies on abstract syntax trees (AST) to work with the code in a project. If you are unfamiliar with abstract syntax trees, it is important to know that the module will be broken down into nodes. Each node has a type that describes what it is. There are types for function declarations (`function a() {...}`), variable declarations (`const b = 'Bees!', c = 'Sea?';`), function calls (`a();`), and all other valid syntax that you can use in JavaScript. The nodes also have a number of type-specific properties. If you are interested in discovering these properties, you should check out [AST Explorer](http://astexplorer.net).

There are many different AST parsers, but the one that Rollup uses is [Acorn](https://github.com/ternjs/acorn). If you are using AST Explorer, you should select the Acorn parser so that you see the same output nodes that Rollup will receive.

Typically, the constructor will parse the contents of a module file and create an AST, but some Rollup loaders create an AST themselves. Creating the AST is just a matter of passing the content (along with any options) to Acorn.

Once the code for a module has been parsed into an AST (and a number of properties of the `Module` have been initialized), we are ready to analyze the module's AST.

## Analysis

### Enhancement

The first step in analyzing a module is to "enhance" it. Enhancing is broken down into a few steps.

#### Enhancing nodes

Starting with the root node, Rollup will walk over all of the nodes in a module's abstract syntax tree. For each one, it will set `parent`, `module`, and `keys` properties on the node.

* `parent` is the parent node of the current node. 
* `module` is the `module` node that the current node belongs to. 
* `keys` is an array of properties specific to that node.

Rollup also uses `enhance` to setup some source map tracking.

Finally, Rollup will set the `prototype` of the node based on its `type`. This will add Rollup related functionality to the node, which is useful for static analysis of the node.

#### Initializing Nodes

Rollup will walk over all of the nodes at the body level of the module (direct children of the module node) to do some utility work.

First, it will attach comments that were parsed out of the code to the nodes. This allows them to be rendered in the output file.

Next, it will call the nodes `initialise` method, passing it the module's `scope` object. This in turn calls three methods: `initialiseScope`, `initialiseNode`, and `initialiseChildren`.

* `initialiseScope` sets the `scope` variable for the node.
* `initialiseNode` does nothing by default, but some nodes implement this to add type-specific behavior.
  * For example, for a `VariableDeclarator` (code that looks like `id = init`), the node will extract all of the names from the nodes `id` and store each one individually. Given the variable declarator `const { a, b } = obj`, Rollup will extract both the names `a` and `b` and add them to the scope (which will help prevent variable name collisions).
  * Expressions may also set themselves as "dependent expressions" of the module. This means that they will be executed when the module is initialized, so Rollup needs to run them when it is tree shaking.
* `initialiseChildren` will iterate over all of the child nodes of the node and initialize them as well.
  * For some node types (like `IfStatement`s and `ForStatement`s), Rollup adds special behavior in order to properly initialize the node's children.
  * Some node types also use `initialiseChildren` to register themselves with their scope. This allows other nodes with an `Identifier` property to determine the node that that refers to.

### Determining Imports and Exports

After all of the nodes in a module have been enhanced, Rollup will walk over all of the body level nodes. For each one, it checks if the node is an import declaration or an export declaration.

When Rollup finds an import declaration node, it will add the source (the location we are importing from) to the module's array of sources. Next, Rollup iterates over the nodes specifiers (`c` and `d` are specifiers from `import { c, d } from 'CD'`) and stores them in its `imports` object.

When Rollup finds an export declaration node, the behavior is similar to an import node. However, Rollup has to add specific behavior for the different types of exports (`export default A`, `export { A, B }`, `export { A } from './A'`, and `export const A = ...`). While the behavior is different, each one will be added to the module's `exports` object. When an export is a re-export (`export { A } from './A'`), the source will be added to the module's sources array so that Rollup knows to add that module to the bundle.


Once Rollup has analyzed a module, it will fetch all of its dependencies. In the next guide, we will see [how rollup find modules](./02-how-does-rollup-find-modules.md).
