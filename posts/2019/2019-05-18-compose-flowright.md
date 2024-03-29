---
layout: post
title: compose/flowRight
date: 2019-05-18
author: aaronchenwei
---

<!-- # compose/flowRight -->

## Create-React-App 与 Decorators

目前的项目中正在安利 Create-React-App。Create-React-App 并不支持[Decorators](https://github.com/facebookincubator/create-react-app/blob/5ea6de91c2f291376578392e453c575785c5b67f/packages/react-scripts/template/README.md#can-i-use-decorators)的使用。

> Create React App doesn’t support decorator syntax at the moment because:
>
> -   It is an experimental proposal and is subject to change.
> -   The current specification version is not officially supported by Babel.
> -   If the specification changes, we won’t be able to write a codemod because we don’t use them internally at Facebook.

## 开发中的痛点

如果不使用 Decorators，那么开发中，可能会写出很长的[Currying Function](./jc9xtmvegeux.md)表达式的长链。比如，在使用了 mobx.js 以后，会在代码中出现如下的例子。

```javascript
import { inject, observer } from "mobx-react";
import restricted from "../utils/restricted";

// ...

// restricted() is a customized HoC
export default restricted(inject("rootStore")(observer(Comp)));
```

如果再加多几个 HoC，那么这句话会写得更长。从程序阅读的角度来看，并不是特别容易看明白。

这样的例子在使用 Redux 的时候也有。

```javascript
import { connect } from "react-redux";
import { withRouter } from "react-router-dom";

// ...

const mapStateToProps = (state) => {
    return {
        // ...
    };
};

const mapDispatchToProps = (dispatch) => {
    return {
        // ...
    };
};

export default withRouter(connect(mapStateToProps, mapDispatchToProps)(Comp));
```

### compose/flowRight

`compose(...functions)`是 redux 提供的一个工具函数。作用是把参数中的 functions 生成一个 Currying 库里化调用的方式。举个例子来说，`compose(f, g, h)` 相当于在`(...args) => f(g(h(...args)))`。

`flowRight`与`compose`的功能类似，是由 lodash 库里面提供。其实 lodash/fp 里面也有一个同名的 compse 函数，起了相似的功能。考虑到 bundle 库的大小问题，我们可以直接选用`flowRight`。

那么以上两个例子，可以改写为

```javascript
import { inject, observer } from "mobx-react";
import flowRight from "lodash/flowRight";
import restricted from "../utils/restricted";

// ...

// restricted() is a customized HoC
const enhance = compose(restricted, inject("rootStore"), observer);

export default enhance(Comp);
```

使用 Redux 的情况

```javascript
import { compose } from "redux";
import { connect } from "react-redux";
import { withRouter } from "react-router-dom";

// ...
const mapStateToProps = (state) => {
    return {
        // ...
    };
};

const mapDispatchToProps = (dispatch) => {
    return {
        // ...
    };
};

const enhance = compose(
    withRouter,
    connect(mapStateToProps, mapDispatchToProps)
);

export default enhance(Comp);
```
