# Arrow function declaration

## ES7/8 Proposal

**Stage:** 0

**Author:** Fedor Nezhivoi

## Current state

For now we do have primary two ways of defining functions: function declaration and function expression.

Note: Probably we do have more ways. Here I'm not talking about definition withing objects or classes.

```js
// ES5
function fname(fargs) {
  fbody
}

// ES5
var fname = function (fargs) { fbody }
// ES6
const fname = (fargs) => fbody
const fname = (fargs) => {
  fbody
}
```

As we can see while function expression got a new syntax function declaration did not.
This is propose to create arrow function declaration syntax for symmetry.

## Problem and rationale

Since now lets assume we do have some functions declared in the scope.

```js
function compose(fn1, fn2) {
  // Implementation
}

function map(predicate) {
  return function (list) {
    // Implementation
  }
}

function reduce(predicate, acc) {
  return function (list) {
    // Implementation
  }
}
```

There is a common way in functional programming to partially apply hogher-order function in order to create new functions.
With arrow functions it looks pretty much better without intermediate variables as you can se below.

```js
// ES5
function squares(list) {
  return map(x => x * x)(list);
}

// ES6
const squares = map(x => x * x)
```

The problem here: we have different things in these two ways because first is function declaration and the second is function expression. The primary drawback of using the second approach hides inside hoisting mechanism. When defining function with function expressions order does really matter.

```js
const sumOfSquares = compose(sum, squares);

const sum = reduce((x, y) => x + y, 0);

const squares = map(x => x * x);
```

Here we would have an error since `squares` and `sum` are `undefined` when calling `compose`.

**Summary:** In order to let developers use benefits from both function declarations and arrow functions we need to add arrow function declaration syntax. One of use cases for this is to be able to declare implementation details later in the scope like your can see [here](https://github.com/graphql/graphql-js/blob/master/src/execution/execute.js).

## Proposed syntax

```js
function fname(fargs) => fbody
function fname(fargs) => {
  fbody
}
```

Here are alternatives (but we do not have reserved keywords for them):
```js
def fname(fargs) => fbody
def fname(fargs) => {
  fbody
}

fn fname(fargs) => fbody
fn fname(fargs) => {
  fbody
}
```
Personally I prefer the first way.

## Desugaring

```js
function fname(fargs) {
  return fbody
}
function fname(fargs) {
  fbody
}
```
