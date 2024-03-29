---
layout: post
title: Currying 与 Higher-order 函数
date: 2019-05-18
author: aaronchenwei
---

<!-- # Currying 与 Higher-order 函数 -->

## :one: 绪

首先，让我们来定义一下 Currying 和 Higher-order 函数这两个概念。在 functional programming （函数式编程）中这两个概念从理论到实际编码中都很重要。

`Higher-order function (高阶函数)`： 简单来说 Higher-order 函数是指一个函数接受一个函数当成参数或者是返回一个函数当成结果。Higher-order 函数可以是 Curried(库里化)的，也可以是非库里化的。在通常的情况之下，人们常把接受一个函数当成参数的函数叫成 Higher-order 函数。React.js 里面就有 Higer-order 组件（HoC）的概念。其实组件的本身就是 function，这也是 Higer-order 函数的一个使用场景。

`Currying function (库里化函数)`： 数学上来说就是指一个函数返回一个函数当成结果，`f(n, m) --> f'(n)(m)`。完全库里化函数就是指一个函数只接受一个参数，返回一个函数或是一个普通的结果。所以，从定义上来说库里化函数是 Higher-order 函数的一种特例。Currying 也是可以级联下去。

`Partial Applicaiton`： 数学上来说`f(n, m) --> f'(m)`。利用 Higher-order 函数，可以表达出来。

下面的代码用了 ES6 （`ECMAScript 6`，也称作`ECMAScript 2015`）的 arrow function 来演示上述的概念。arrow function 可以方便的定义一个 function。

```javascript
/* arrow function */
const square = (x) => x * x;
console.log(square(4)); // print 16

/* currying */
const curry = (f) => (a) => (b) => f(a, b);

/* not curring but a higher-order function */
const uncurry = (f) => (a, b) => f(a)(b);

/* higher-order function with partial application */
const papply = (f, a) => (b) => f(a, b);
```

当然了，用 ES5 也是可以写出 Currying 和 Higher-order 函数的。用了 arrow function 这样的语法糖，可以更方便，更易懂的写出 Currying 和 Higher-order 函数。

## :two: Currying

那我们来详细讲解一下 Curring。先直接来看一段 ES5 代码。

```javascript
var greet = function (greeting, name) {
    console.log(greeting + ", " + name);
};

greet("Hello", "World"); // "Hello, World"
```

### First Currying

我们来把前面的例子按 Currying 函数方式来改写。

```javascript
var greetCurried = function (greeting) {
    return function (name) {
        console.log(greeting + ", " + name);
    };
};

const greetCurriedWithArrow = (greeting) => (name) => {
    console.log(greeting + ", " + name);
};
```

我们来调用一下上面的 Currying 函数。

```javascript
var greetHello = greetCurried("Hello");
greetHello("World"); // "Hello, World"
greetHello("Currying"); // "Hello, Currying"
```

其实我们也可以直接调用上面 Currying 函数，注意一下下面两个括号的使用。

```javascript
greetCurried("Hi there")("My friend"); //"Hi there, My friend"
```

### Curry All the Things

下面改用成 ES6 的 arrow function 来举例。

```javascript
const greetDeeplyCurried =
    (greeting) => (separator) => (emphasis) => (name) => {
        console.log(greeting + separator + name + emphasis);
    };

const greetAwkwardly = greetDeeplyCurried("Hello")("...")("?");
greetAwkwardly("Heidi"); //"Hello...Heidi?"
greetAwkwardly("Eddie"); //"Hello...Eddie?"

const sayHello = greetDeeplyCurried("Hello")(", ");
sayHello(".")("Heidi"); //"Hello, Heidi."
sayHello(".")("Eddie"); //"Hello, Eddie."

const askHello = sayHello("?");
askHello("Heidi"); //"Hello, Heidi?"
askHello("Eddie"); //"Hello, Eddie?"
```

### Argument Order

> One thing that’s important to keep in mind when currying is the order of the arguments. Using the approach we’ve described, you obviously want the argument that you’re most likely to replace from one variation to the next to be the last argument passed to the original function.

## :three: Real World Currying Examples

下面我们来看看实际中的一些例子。

### JavaScript Bind

Function.prototype.bind()就是一个 Currying 函数

```javaScript
/* first param is thisArg which is irrelevant now */
increment = add.bind(undefined, 1)
increment(4) === 5
```

### React and Redux

Redux 的`connect()`就是一个非常非常典型的 Currying 函数。这个可以开另外一篇 Blog 详细讲，特别是 Redux 的 middlewares 的应用。

```javaScript
export default connect(mapStateToProps)(TodoApp)
```

### Event Handling

```javaScript
const handleChange = (fieldName) => (event) => {
  saveField(fieldName, event.target.value)
}

<input type="text" onChange={handleChange('email')} ... />
```
