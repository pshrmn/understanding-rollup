# Big List of AST Node Types

These are Acorn node types, since that is what Rollup uses.

## Shared Types

### Node

### Function

[Source](https://github.com/rollup/rollup/blob/master/src/ast/nodes/shared/Function.js)

### Statement

[Source](https://github.com/rollup/rollup/blob/master/src/ast/nodes/shared/Statement.js)

### Class

[Source](https://github.com/rollup/rollup/blob/master/src/ast/nodes/shared/Class.js)

-----

## Types

### ArrayPattern

[Source](https://github.com/rollup/rollup/blob/master/src/ast/nodes/ArrayPattern.js)

#### Example 

```js
// the array param
function thing([a, b, c]) {}
```

#### Properties

* elements - array of `Identifier`s and `AssignmentPattern`s

### ArrowFunctionExpression

[Source](https://github.com/rollup/rollup/blob/master/src/ast/nodes/ArrowFunctionExpression.js)

Inherits from [Function](#function)

##### Example 

```js
() => {}
```

##### Properties

* params - array of `Identifier`s or patterns
* body - `BlockStatement`, expression, or `Literal`

### AssignmentExpression

[Source](https://github.com/rollup/rollup/blob/master/src/ast/nodes/AssignmentExpression.js)

#### Example 

```js
x = 5
```

#### Properties

* left - `Identifier` or pattern
* right - a `Literal` or expression
* operator - `=`, `+=`, etc.

### BinaryExpression

[Source](https://github.com/rollup/rollup/blob/master/src/ast/nodes/BinaryExpression.js)

#### Example 

```js
variable === 24
```

#### Properties

* left - `Literal` or expression
* right - `Literal` or expression
* operator - `==`, `+`, etc.

### BlockStatement

[Source](https://github.com/rollup/rollup/blob/master/src/ast/nodes/BlockStatement.js)

Inherits from [Statement](#statement)

#### Example 

```js
{
  const x = 'hello';
  console.log(x);
}
```

#### Properties

* body - array of statements and declarations

### CallExpression

[Source](https://github.com/rollup/rollup/blob/master/src/ast/nodes/CallExpression.js)

#### Example 

```js
fn(one, two)
```

#### Properties

* callee - `Identifier` or `MemberExpression`
* arguments - array of `Literal`s and expressions

### CatchClause

[Source](https://github.com/rollup/rollup/blob/master/src/ast/nodes/CatchClause.js)

#### Example 

```js
catch (e) {
  ...
}
```

#### Properties

* param - an `Identifier`
* body - a `BlockStatement`

### ClassDeclaration

[Source](https://github.com/rollup/rollup/blob/master/src/ast/nodes/ClassDeclaration.js)

Inherits from [Class](#class)

#### Example 

```js
class Thing {}
class AnotherThing extends Thing {}
// the class part of:
export default class {}
```

#### Properties

* id - an `Identifier`
* superClass - an `Identifier` or null
* body - a `ClassBody`

### ClassExpression

[Source](https://github.com/rollup/rollup/blob/master/src/ast/nodes/ClassExpression.js)

Inherits from [Class](#class)

#### Example 

```js
// the right node of
var x = class Thing {};
```

#### Properties

* id - an `Identifier` (optional)
* superClass - an `Identifier` or null
* body - a `ClassBody` 

###  ConditionalExpression

[Source](https://github.com/rollup/rollup/blob/master/src/ast/nodes/ConditionalExpression.js)

#### Example 

```js
test ? consequent : alternate
```

#### Properties

* test - an expression or `Identifier`
* consequent - an expression or `Literal`
* alternate - an expression or `Literal`

### EmptyStatement

[Source](https://github.com/rollup/rollup/blob/master/src/ast/nodes/EmptyStatement.js)

#### Example 

```js
;
```

### ExportAllDeclaration

[Source](https://github.com/rollup/rollup/blob/master/src/ast/nodes/ExportAllDeclaration.js)

#### Example 

```js
export * from 'another-module';
```

#### Properties

* source - a `Literal` string

### ExportDefaultDeclaration

[Source](https://github.com/rollup/rollup/blob/master/src/ast/nodes/ExportDefaultDeclaration.js)

#### Example 

```js
export default function() {...}
```

#### Properties

* declaration

### ExportNamedDeclaration

[Source](https://github.com/rollup/rollup/blob/master/src/ast/nodes/ExportNamedDeclaration.js)

#### Example 

```js
export const x = 'Exit';  // declaration

const y = 'Why';
export { y }              // specifiers
```

#### Properties

* declaration
* specifiers - an array of export specifiers

**Note:** This is a one or the other type of situation

### ExpressionStatement

[Source](https://github.com/rollup/rollup/blob/master/src/ast/nodes/ExpressionStatement.js)

Inherits from [Statement](#statement)

#### Example 

```js
variable = value;
fn();
```

#### Properties

* expression

### ForInStatement

[Source](https://github.com/rollup/rollup/blob/master/src/ast/nodes/ForInStatement.js)

Inherits from [Statement](#statement)

#### Example 

```js
for (let key in obj) {
  ...
}
```

#### Properties

* left - a `VariableDeclaration`
* right - an `Identifier` or expression
* body - a `BlockStatement`

### ForOfStatement

[Source](https://github.com/rollup/rollup/blob/master/src/ast/nodes/ForOfStatement.js)

#### Example 

```js
for (let key of iterable) {
  ...
}
```

#### Properties

* left - a `VariableDeclaration`
* right - an `Identifier` or expression
* body - a `BlockStatement`

### ForStatement

[Source](https://github.com/rollup/rollup/blob/master/src/ast/nodes/ForStatement.js)

#### Example 

```js
for (init; test; update) {
  ...
}
```

#### Properties

* init - a `VariableDeclaration` or null
* test - an expression
* update - an expression
* body - a `BlockStatement` or expression

### FunctionDeclaration

[Source](https://github.com/rollup/rollup/blob/master/src/ast/nodes/FunctionDeclaration.js)

Inherits from [Function](#function)

#### Example 

```js
function name(param1, param2) {
  ...
}
```

#### Properties
* id - `Identifier`
* generator - boolean
* expression - boolean (???)
* params - an array of `Identifier`s or patterns
* body - a `BlockStatement`

### FunctionExpression

[Source](https://github.com/rollup/rollup/blob/master/src/ast/nodes/FunctionExpression.js)

Inherits from [Function](#function)

#### Example 

```js
// the right part of:
let fn = function() {}
```

#### Properties

* id - `Identifier` or null
* generator - boolean
* expression - boolean (???)
* params - an array of `Identifier`s or patterns
* body - a `BlockStatement`

### Identifier

[Source](https://github.com/rollup/rollup/blob/master/src/ast/nodes/Identifier.js)

#### Example 

```js
// x in:
const x = 'hello';
```

#### Properties

* name - a string

### IfStatement

[Source](https://github.com/rollup/rollup/blob/master/src/ast/nodes/IfStatement.js)

Inherits from [Statement](#statement)

#### Example 

```js
if (condition) {
  ...
} else {
  ...
}
```

#### Properties

* test - an `Identifier` or expression
* consequent - a `BlockStatement`
* alternate - An `IfStatement` or `BlockStatement`

### ImportDeclaration

[Source](https://github.com/rollup/rollup/blob/master/src/ast/nodes/ImportDeclaration.js)

#### Example 

```js
import Name from 'module';
import { One, Two } from 'module';
import * as Grouped from 'module';
```

#### Properties

* specifiers - array of specifiers
* source - `Literal`

### Literal

[Source](https://github.com/rollup/rollup/blob/master/src/ast/nodes/Literal.js)

#### Example 

```js
// the string in
const x = 'test';
```

#### Properties

* value - the literal value
* raw - the value as a string

### LogicalExpression

[Source](https://github.com/rollup/rollup/blob/master/src/ast/nodes/LogicalExpression.js)

#### Example 

```js
thing && thing.property
```

#### Properties

* left - an `Identifier`, `Literal`, or expression
* operator - '&&' or '||'
* right - an `Identifier`, `Literal`, or expression

### MemberExpression

#### Example 

```js
object.property
object['property']
```

#### Properties

* object - an `Identifier` or expression
* property - an `Identifier` or `Literal`
* computed - boolean

### NewExpression

[Source](https://github.com/rollup/rollup/blob/master/src/ast/nodes/NewExpression.js)

#### Example 

```js
new Thing(arg1, arg2)
```

#### Properties

* callee - `Identifier` or expression
* arguments - an arry of `Identifier`s or expressions

### ObjectPattern

[Source](https://github.com/rollup/rollup/blob/master/src/ast/nodes/ObjectPattern.js)

#### Example 

```js
{ one: 1, two: 2 }
```

#### Properties

* properties - an array of `Property` nodes

### Property

[Source](https://github.com/rollup/rollup/blob/master/src/ast/nodes/Property.js)

#### Example

```
// any of the lines below (a,b,c, d)
{
  a: 'Ayy',
  b() {...},
  c,
  [d]: 'Dee'
}
```

#### Properties

* method - true if if `key()` format
* shorthand - true if in `{ key }` format
* computed - true if in `{ [key]: ... }` format
* key - an `Identifier`
* value - a `Literal`, pattern, or expression
* kind - "init"??

### RestElement

[Source](https://github.com/rollup/rollup/blob/master/src/ast/nodes/RestElement.js)

#### Example

```js
// the "...rest" element
const [first, ...rest] = [1,2,3,4];
```

#### Properties

* argument - an `Identifier`

### ReturnStatement 

[Source](https://github.com/rollup/rollup/blob/master/src/ast/nodes/ReturnStatement.js)

#### Example

```js
return something;
```

#### Properties

* argument - an `Identifier`, `Literal`, or expression

### SwitchStatement

[Source](https://github.com/rollup/rollup/blob/master/src/ast/nodes/SwitchStatement.js)

#### Example 

```js
switch (discriminant) {
case 1:
  ...
  break;
case 2:
  ...
  break;
default:
  ...
  break;
}
```

#### Properties

* discriminant - an `Identifier` or expression
* cases - an array of `SwitchCase` nodes

### TaggedTemplateExpression

[Source](https://github.com/rollup/rollup/blob/master/src/ast/nodes/TaggedTemplateExpression.js)

#### Example 

```js
tag\`template string\`
```

#### Properties

* tag - an `Identifier` (possibly a function expression?)
* quasi - a `TemplateLiteral`

### TemplateLiteral

[Source](https://github.com/rollup/rollup/blob/master/src/ast/nodes/TemplateLiteral.js)

#### Example 

```js
`I am a template literal`
`Name: ${obj.name}\nAge: ${obj.age}`
```

#### Properties

* expressions - an array of `Identifier`s or expressions
* quasis - an array of `TemplateElement`s

### ThisExpression

[Source](https://github.com/rollup/rollup/blob/master/src/ast/nodes/ThisExpression.js)

#### Example 

```js
// just the "this" part of the following lines
this.value = arg => { console.log(arg); };
this.value('hi!');
```

### ThrowStatement

[Source](https://github.com/rollup/rollup/blob/master/src/ast/nodes/ThrowStatement.js)

#### Example 

```js
throw new Error();
```

#### Properties

* argument - an `Identifier` or expression

### UnaryExpression

[Source](https://github.com/rollup/rollup/blob/master/src/ast/nodes/UnaryExpression.js)

#### Example 

```js
+plus
!not
~flipBits
typeof joke
```

#### Properties

* operator - `+`, `-`, `!`, `~`, `typeof`, `void`, `delete`
* prefix - true
* argument - an `Identifier` or expression

### UpdateExpression

[Source](https://github.com/rollup/rollup/blob/master/src/ast/nodes/UpdateExpression.js)

#### Example 

```js
variable++
```

#### Properties

* operator - '++' or '--'
* prefix - false
* argument - an `Identifier` or expression

### VariableDeclaration

[Source](https://github.com/rollup/rollup/blob/master/src/ast/nodes/VariableDeclaration.js)

#### Example 

```js
var one = 1;
let two = 2;
const three = 3, four = 4;
```

#### Properties

* kind - 'var', 'let', or 'const'
* declarations - an array of `VariableDeclarator`s

### VariableDeclarator

[Source](https://github.com/rollup/rollup/blob/master/src/ast/nodes/VariableDeclarator.js)

#### Example 

```js
x = 'exit'
{ y, z } = obj
[ a, b ] = arr
```

#### Properties

* id - an `Identifier` or pattern
* init - a `Literal`, `Identifier`, or expression
