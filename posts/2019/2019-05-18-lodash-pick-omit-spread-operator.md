---
layout: post
title: Lodash pick/omit 与 ES7 Spread operator
date: 2019-05-18
author: aaronchenwei
---

日常编程中，一般会遇上这样的 case：对一个对象增加或是删除一些 properties。在 react.js 编程中，一般会用 immutable 的对象，我们并不是去直接改变原来的对象，而是创建一个新的对象。

## Lodash pick

```javascript
const object = { a: 1, b: "2", c: 3 };
const picked = _.pick(object, ["a", "c"]); // picked => { 'a': 1, 'c': 3 }
```

## Lodash omit

```javascript
let object = { a: 1, b: "2", c: 3 };
const omitted = _.omit(object, ["a", "c"]); // omitted => { 'b': '2' }
```

注意：　 lodash v5 将会删除 omit。

## Spread operator

需要\*\*Babel plugin transform-object-rest-spread

```javascript
let object = { a: 1, b: "2", c: 3 };
const { b, ...picked } = object; // picked => { 'a': 1, 'c': 3 }

let object = { a: 1, b: "2", c: 3 };
const { a, c, ...omitted } = object; // omitted => { 'b': '2' }
```

## Object Destructuring and Property Shorthand

```javascript
let object = { a: 1, b: "2", c: 3 };
const picked = (({ a, c }) => ({ a, c }))(object);
```
