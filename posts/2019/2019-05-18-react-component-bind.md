---
layout: post
title: 在 React 组件中使用 bind 方法
date: 2019-05-18
author: aaronchenwei
---

React.js 的作者总是强调 react.js 的本质还是 Javascript，所以在理解 React.js 中 bind 的使用之前，我们先来看看 Javascript 里面这个问题。

## JavaScript

从其他的编程语言比如 Java 或是 Ruby 切到 Javascript 编程，对于 this 的理解可能会产生一切混淆。在 Java 中，非静态方法中的 _this_ 总是指向定义这个方法所在类的对象。Ruby 中的 _self_ 也是类似的概念。所以，从这样的语言到了 Javascript 的时候，可能会有点不太适应。

Javascript 在这个 this 指向问题就有点不一样。

> **In JavaScript function context is defined while calling the function, not while defining it!**

怎么来理解这句话呢？我们可以按以下 4 种使用模式来详细说明 Javascript 中的 this。

-   function invocation pattern
-   method invocation pattern
-   constructor invocation pattern
-   apply invocation pattern

### Function Invocation Pattern

> If there are no dots in your function call, your context is likely to be window.

我们先来看看下面这段代码。

```javascript
var func = function () {
    console.log(this);
    // ...
};

func();
```

调用方法（func()）的时候，会把 _context(this)_ 设置成指向这段代码运行环境的全局变量。如果是在浏览器中运行这段代码，`console.log(this);` 会打印出 **window** 这个全局变量的值。你自己可以试一下在 node.js 运行这段代码，看看最后的 console 里面显示了什么。

我们再来看一段代码。

```javascript
var whoami = {
    func: function () {
        console.log(this);
        // ...
    },
};

var fun = whoami.func;
fun();
```

`object.func` 函数是对象`whoami`的一个方法。如果运行了这面的代码，this 还是指向运行这段代码环境的全局变量。这里和 Java 不一样，要特别注意一下。

### Method Invocation Pattern

> If there are dots in your function call, your function context will be the right-most element of your dots chain.

我们又来看一段代码

```javascript
var dog = {
    BARK_SOUND: "woof woof!!",
    run: function () {
        return this.BARK_SOUND;
    },
};

console.log(dog.run()); // print woof woof!!
var runningFun = dog.run;
console.log(runningFun()); // print undefined
```

通过对象的方法调用，this 是指向这个对象，这个和 Java 类似。

如果对象有嵌套(nested)，this 指向最临近的那个对象。

```javascript
var foo = {
    bar: {
        func: function () {
            console.log(this);
            //...
        },
    },
};

foo.bar.func(); // this will point to `bar`
```

打印出来的语句`{ func: [Function: func] }`，也就是 bar 对象。

### Constructor Invocation Pattern

> Every time you see a new followed by a function name, your this will point to a newly created empty object.

```javascript
function Dog() {
    this.bark = function () {
        return "woof!";
    };
}

// console.log(Dog()); // print undefined

var husky = new Dog(); // this is set to an empty object {}. Returns `this` implicitly.
console.log(husky.bark()); // print "woof!";
```

这个就是 Javascript 中的 new 操作符。

### Apply Invocation Pattern

> -   `call` - it takes a context as the first argument. The rest of arguments are arguments passed to the function being called this way.
> -   `apply` - it takes a context as the first argument and an array of arguments for the function being called as the second argument.

```javascript
function addAndSetX(a, b) {
    this.x += a + b;
}

var obj1 = { x: 1, y: 2 };

addAndSetX.call(obj1, 1, 1); // this = obj1, obj1 after call = { x: 3, y : 2 }
// It is the same as:
// addAndSetX.apply(obj1, [1, 1]);
```

### Binding functions

```javascript
function add(x, y) {
    this.result += x + y;
}

var computation1 = { result: 0 };
var boundedAdd = add.bind(computation1);

boundedAdd(1, 2); // `this` is set to `computation1`.
//  computation1 after call: { result: 3 }

var boundedAddPlusTwo = add.bind(computation1, 2);
boundedAddPlusTwo(4); // `this` is set to `computation1`.
// computation1 after call: { result: 9 }
```

```javascript
var obj = { boundedPlusTwo: boundedAddPlusTwo };
obj.boundedPlusTwo(4); // `this` is set to `computation1`.
// even though method is called on `obj`.
// computation1 after call: { result: 15 }
```

```javascript
var computation2 = { result: 0 };
boundedAdd.call(computation2, 1, 2); // `this` is set to `computation1`.
// even though context passed to call is
// `computation2`
// computation1 after call: { result: 18 }
```

## React.js

### ECMAScript 2015 classes

```javascript
class InputExample extends React.Component {
    constructor(props) {
        super(props);

        this.state = { text: "" };
        this.change = this.change.bind(this);
    }

    change(ev) {
        this.setState({ text: ev.target.value });
    }

    render() {
        let { text } = this.state;
        return <input type="text" value={text} onChange={this.change} />;
    }
}
```

### ECMAScript 2015 classes with class properties

```javascript
class InputExample extends React.Component {
    state = { text: "" };
    change = (ev) => this.setState({ text: ev.target.value });

    render() {
        let { text } = this.state;
        return <input type="text" value={text} onChange={this.change} />;
    }
}
```
