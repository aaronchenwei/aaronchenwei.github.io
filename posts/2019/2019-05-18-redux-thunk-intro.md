---
layout: post
title: Redux-Thunk 中间件(Middleware)与异步处理
date: 2019-05-18
author: aaronchenwei
---

作为 Redux 作者自己开发的中间件(Middleware)，Redux-Thunk 相对于其他的 Middleware，简单并且容易上手(主要是与 Redux 系列的概念的融合)

## 1. Redux-Thunk 的源代码

Redux-Thunk 的[源代码](https://github.com/gaearon/redux-thunk/blob/master/src/index.js)是相当简练的。

```javascript
function createThunkMiddleware(extraArgument) {
    return ({ dispatch, getState }) =>
        (next) =>
        (action) => {
            if (typeof action === "function") {
                return action(dispatch, getState, extraArgument);
            }

            return next(action);
        };
}

const thunk = createThunkMiddleware();
thunk.withExtraArgument = createThunkMiddleware;

export default thunk;
```

熟悉 Redux 的知道，如果 action creator 函数返回的是一个 funciton，那么我们就把这样的 function 叫做 thunk。

> A thunk is a function that wraps an expression to delay its evaluation.

```javascript
// calculation of 1 + 2 is immediate
// x === 3
let x = 1 + 2;

// calculation of 1 + 2 is delayed
// foo can be called later to perform the calculation
// foo is a thunk!
let foo = () => 1 + 2;
```

在 Javscript 中，使用 Higher-order function 可以生成 thunk。可以点击此处回顾[Higher-order function](./jc9xtmvegeux.md)

## 2. Redux-Thunk 与异步处理

thunk 可以 delay its evaluation 的特点，可以很好的用来实现异步处理。下面的简单代码就有三种异步处理的方式。可以选择自己习惯的写法进行处理。react.js 的环境用了[create-react-app](https://github.com/facebookincubator/create-react-app)。所以，已经有了 async/await 的 Polyfill。考虑到 chrome 已经原生支持 async/await 了，这种写法可以放心使用。

```javascript
import { applyMiddleware, createStore } from "redux";
import thunk from "redux-thunk";
import co from "co";

const reducer = (state = {}, action) => {
    switch (action.type) {
        case "pet":
        case "fruite":
        case "json":
        case "error":
            return {
                ...state,
                [action.name]: action.value,
            };
        default:
            return state;
    }
};

const store = createStore(reducer, applyMiddleware(thunk));

/**
 * action creator
 * Thunk - asynchronization is based on fetch/Promise
 */
function asyncThunkWithPromise() {
    return (dispatch, getState) => {
        dispatch({ type: "pet", name: "dog", value: "cute" });

        fetch("https://<host>:<port>/<api>/<entity>")
            .then((response) => response.json())
            .then((json) =>
                dispatch({ type: "json", name: "posts", value: json })
            )
            .catch((e) =>
                dispatch({ type: "error", name: "error", value: e.message })
            );

        dispatch({ type: "fruite", name: "apple", value: "sweet" });
        console.log("state: ", getState());
    };
}

/**
 * action creator
 * Thunk - asynchronization based on async/await
 */
function asyncThunkWithAwait() {
    return async (dispatch, getState) => {
        dispatch({ type: "pet", name: "dog", value: "cute" });

        try {
            const response = await fetch(
                "https://<host>:<port>/<api>/<entity>"
            );
            const json = await response.json();
            dispatch({ type: "json", name: "posts", value: json });
        } catch (e) {
            dispatch({ type: "error", name: "error", value: e.message });
        }

        dispatch({ type: "fruite", name: "apple", value: "sweet" });
        console.log("state: ", getState());
    };
}

/**
 * action creator
 * Thunk - asynchronization based on yield/generator function
 * It is based on co.js and just for demo purpose
 * NOT recommend for real use, better use
 * middleware [Redux-Saga](https://github.com/redux-saga/redux-saga)
 */
function asyncThunkWithYield() {
    return async (dispatch, getState) => {
        co(function* () {
            dispatch({ type: "pet", name: "dog", value: "cute" });

            try {
                const response = yield fetch(
                    "https://<host>:<port>/<api>/<entity>"
                );
                const json = yield response.json();
                dispatch({ type: "json", name: "posts", value: json });
            } catch (e) {
                dispatch({ type: "error", name: "error", value: e.message });
            }

            dispatch({ type: "fruite", name: "apple", value: "sweet" });
            console.log("state: ", getState());
        });
    };
}
```
