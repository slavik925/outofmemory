---
title: New features of ECMAScript 2019/2020
date: "2019-12-06T16:05:00Z"
---

## Understanding the naming

The naming in JS world has always been something that confused me.
ES6 and ES10 is that EScript? ECMAScript 2019 it this JavaScript or no? ts39 - what's that about?

So once for all lets deal with this names:

ECMAScript (ES) - [Wiki](https://en.wikipedia.org/wiki/ECMAScript)

- is a scripting-language specification standardized by Ecma International.
- ECMA (European Computer Manufacturers Association) - is a standards organization for information and communication systems.
- ECMAScript was created by Brendan Eich to standardize the JavaScript language (initially it was named Mocha, later LiveScript, and finally JavaScript).

JavaScript (JS) - [Wiki](https://en.wikipedia.org/wiki/JavaScript)

- High-level, just-in-time compiled programming language that conforms to the ECMAScript specification.

TS39 is a group that is a part of ECMA organization.

> is a group of JavaScript developers, implementers, academics, and more, collaborating with the community to maintain and evolve the definition of JavaScript.
> https://tc39.es/

Now it should be clear who is who and what is what. :)

## What's new do we have in the ES10/11?

The last 10th edition of the ECMAScript language specification is available here:

https://www.ecma-international.org/ecma-262/10.0/index.html

And here is the latest draft:

https://tc39.es/ecma262/

There aren't a lot of the new additions. So lets focus on the most significant ones:

ES2019

- Object.fromEntries
- String.trimming
- Array.prototype.{flat, flatMap}
- optional catch binding
- Symbol.prototype.description
- Function.prototype.toString revision

ES2020

- String.prototype.matchAll
- BigInt
- Promise.allSettled
- globalThis

Let's walk through them briefly:

## ES2019 features overview

### Object.fromEntries

This method takes a list of key-value pairs and returns a new object whose properties are given by those entries.

```javascript
const arr = [['a', '1'],  'b', '2']];
const obj = Object.fromEntries(arr);

// Result: obj = { a: '1', b: '2' }
```

The same will be true for the `Map`:

```javascript
const arr = Map([['a', '1'],  'b', '2']]);
const obj = Object.fromEntries(arr);

// Result: obj = { a: '1', b: '2'}
```

<p>
<small>
Check current support at <a href="https://kangax.github.io/compat-table/es2016plus/#test-Object.fromEntries">https://kangax.github.io/compat-table/es2016plus/#test-Object.fromEntries</a>
</small>
</p>

___

#### String.trimming (trimStart, trimEnd)

trimStart() - removes whitespace from the beginning of a string.

```javascript
const test = "   This is a test string   "
const obj = test.trimStart()

// Result: obj = 'This is a test string   ';
```

trimEnd does the same but for the end of string. This functions has aliases:

- String.trimLeft()
- String.trimRight()

<p>
<small>
Check current support at <a href="https://kangax.github.io/compat-table/es2016plus/#test-string_trimming">https://kangax.github.io/compat-table/es2016plus/#test-string_trimming</a>
</small>
</p>

---

### Array.{flat, flatMap}

- Array.flat(depth: int) - flattens the array with specified depth, it's easier to show this on the example:

```javascript
const numbers = [1, 2, [3, 4]];
const obj = numbers.flat();
// Result: obj = [1, 2, 3, 4]

const numbers = [1, 2, [3, 4, [5, 6]]];
const numbers = numbers.flat();
// Result: obj = [1, 2, 3, 4, [5, 6]]

const numbers = [1, 2, [3, 4, [5, 6]]];
const obj = const numbers = [1, 2, [3, 4, [5, 6]]];
.flat(2);
// Result: obj = [1, 2, 3, 4, 5, 6]

const numbers = [1, 2, [3, 4, [5, 6, [7, 8, [9, 10]]]]];
const obj = numbers.flat(Infinity);
// Result: obj = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
```

- Array.flatMap(function(currentVal, index, array)) - first applies the given function to every array element and then 'flat's the result and inserts one in the new array. The same could be achieved by calling a `map` and then `flat` with depth = 1.

```javascript
const numbers = [1, 2, 3, 4]
const obj = numbers.flatMap(x => [x * 2])
// Result: obj = [2, 4, 6, 8]
```

<p>
<small>
Check current support at <a href="https://kangax.github.io/compat-table/es2016plus/#test-Array.prototype.{flat,_flatMap}">https://kangax.github.io/compat-table/es2016plus/#test-Array.prototype.{flat,_flatMap}</a>
</small>
</p>

___

#### Optional catch binding

If you are not using the exception argument inside a catch block you could avoid it now:

```javascript
try {
  //...
} catch {
  console.error("Ooops")
}
```

<p>
<small>
Check current support at <a href="https://kangax.github.io/compat-table/es2016plus/#test-optional_catch_binding">https://kangax.github.io/compat-table/es2016plus/#test-optional_catch_binding</a>
</small>
</p>

___

#### Symbol.description

Now instead of calling `Symbol`.toString you could use description property. The main difference is that `description` property does not return stringified class name:

```javascript
const obj = Symbol("Description")

console.log(obj.description)
console.log(obj.toString())

// Result: 'Description'
// Result: 'Symbol(Description)'
```

<p>
<small>
Check current support at <a href="https://kangax.github.io/compat-table/es2016plus/#test-Symbol.prototype.description">https://kangax.github.io/compat-table/es2016plus/#test-Symbol.prototype.description</a>
</small>
</p>

---

## ES2020 features overview

#### String.matchAll

Returns and iterator of all results matching a string against a regular expression (including capturing groups).

```javascript
const regexp = /t(e)(st(\d?))/g
const str = "test1test2"

const obj = [...str.matchAll(regexp)]

console.log(array[0])
// Result = ["test1", "e", "st1", "1"]

console.log(array[1])
// Result = ["test2", "e", "st2", "2"]
```

<p>
<small>
Check current support at <a href="https://kangax.github.io/compat-table/es2016plus/#test-String.prototype.matchAll">https://kangax.github.io/compat-table/es2016plus/#test-String.prototype.matchAll</a>
</small>
</p>

---

#### BigInt

BigInt is a new 7th primitive type. Fixed number of varying length. You could create it with BigInt constructor of with `n` literal;

```javascript
const bigNumber = 9007199254740991n
const alsoHuge = BigInt(9007199254740991)
```

<p>
<small>
Check current support at <a href="https://kangax.github.io/compat-table/es2016plus/#test-BigInt">https://kangax.github.io/compat-table/es2016plus/#test-BigInt</a>
</small>
</p>

---

#### Promise.allSettled

Returns a promise that resolves after all of the given promises have either resolved or rejected.

```javascript
const promise1 = Promise.resolve(3)
const promise2 = new Promise((resolve, reject) =>
  setTimeout(reject, 100, "foo")
)
const promises = [promise1, promise2]

Promise.allSettled(promises).then(results =>
  results.forEach(result => console.log(result.status))
)

// expected output:
// "fulfilled"
// "rejected"
```

<p>
<small>
Check current support at <a href="https://kangax.github.io/compat-table/es2016plus/#test-Promise.allSettled">https://kangax.github.io/compat-table/es2016plus/#test-Promise.allSettled</a>
</small>
</p>

___

#### globalThis

Not a lot to say here. This object was not standardized before ES11. Now it could be used directly without any hacks for different platforms.

<p>
<small>
Check current support at <a href="https://kangax.github.io/compat-table/es2016plus/#test-globalThis">https://kangax.github.io/compat-table/es2016plus/#test-globalThis</a>
</small>
</p>

___

<h2 style="text-align: center">The End</h2>
