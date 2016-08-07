Higher-Order Iterator
=====================

[![NPM Status][npm-img]][npm]
[![Travis Status][test-img]][travis]
[![Coverage Status][coverage-img]][coveralls]
[![Dependency Status][dependency-img]][david]

[npm]:            https://www.npmjs.org/package/ho-iter
[npm-img]:        https://img.shields.io/npm/v/ho-iter.svg

[travis]:         https://travis-ci.org/blond/ho-iter
[test-img]:       https://img.shields.io/travis/blond/ho-iter.svg?label=tests

[coveralls]:      https://coveralls.io/r/blond/ho-iter
[coverage-img]:   https://img.shields.io/coveralls/blond/ho-iter.svg

[david]:          https://david-dm.org/blond/ho-iter
[dependency-img]: http://img.shields.io/david/blond/ho-iter.svg

The iterator which takes iterators as arguments and returns an iterator.

> [Iterators and Generators Guide](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Iterators_and_Generators)

Install
-------

```
$ npm install --save ho-iter
```

Usage
-----

```js
const hoi = require('ho-iter');

const entries1 = hoi({ foo: 'bar' });
const entries2 = hoi({ baz: 42 });

const keys = [];
const values = [];

for (let [key, value] of hoi.series(entries1, entries2)) {
    keys.push(key);
    values.push(value);
}

console.log(`keys: ${keys}`);
console.log(`values: ${values}`);

// ➜ keys: [foo, baz]
// ➜ values: [bar, 42]
```

API
---

* [isIterable(iterable)](#isiterableiterable)
* [isIterator(iterator)](#isiteratoriterator)
* [series(...iterables)](#seriesiterators)
* [evenly(...iterables)](#evenlyiterators)

### isIterable(iterable)

Returns `true` if the specified object implements the Iterator protocol via implementing a `Symbol.iterator`.

**Example:**

```js
const isIterable = require('ho-iter').isIterable;

isIterable([1, 2, 3]); // true
isIterable(123); // false
```

### isIterator(iterator)

Returns `true` if the specified object is iterator.

```js
const isIterator = require('ho-iter').isIterator;

const generator = function *() {};
const iterator = generator();

isIterator(generator) // false
isIterator(iterator) // true
```

### series(...iterables)

Returns an Iterator, that traverses iterators in series.

This is reminiscent of the concatenation of arrays.

**Example:**

```js
const series = require('ho-iter').series;

const arr1 = [1, 2];
const arr2 = [3, 4];

const set1 = new Set([1, 2]);
const set2 = new Set([3, 4]);

[].concat(arr1, arr2);

// ➜ [1, 2, 3, 4]

for (let item of series(set1, set2)) {
    console.log(item);
}

// ➜ 1 2 3 4
```

### evenly(...iterables)

Returns an Iterator, that traverses iterators evenly.

This is reminiscent of the traversing of several arrays.

**Example:**

```js
const evenly = require('ho-iter').evenly;

const arr1 = [1, 2];
const arr2 = [3, 4];

const set1 = new Set([1, 2]);
const set2 = new Set([3, 4]);

for (let i = 0; i < arr1.length; i++) {
    console.log(arr1[i]);
    console.log(arr2[i]);
}

// ➜ 1 3 2 4

for (let item of evenly(set1, set2)) {
    console.log(item);
}

// ➜ 1 3 2 4
```

License
-------

MIT © [Andrew Abramov](https://github.com/blond)
