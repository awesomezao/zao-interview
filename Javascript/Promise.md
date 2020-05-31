## 1.Promise是什么

Promise是异步编程的一种解决方案，比传统的解决方案回调函数和事件更合理更强大

## 2.Promise解决了什么问题

回调函数回调地狱的情况，让异步操作更加方便美观

## 3.如何实现Promise

```javascript
const PENDING = 'pending'
const FULFILLED = 'fulfilled'
const REJECTED = 'rejected'

// utils
const isFunction = target => typeof target === 'function'
const isObject = target => !!(target && typeof target === 'object')
const isThenable = target => (isFunction(target) || isObject(target)) && 'then' in target
const isPromise = target => target instanceof MyPromise

// 处理回调函数
const handleCallback = (callback, state, result) => {
  let {
    onFulfilled,
    onRejected,
    resolve,
    reject
  } = callback
  try {
    if (state === FULFILLED) {
      isFunction(onFulfilled) ? resolve(onFulfilled(result)) : resolve(result)
    } else if (state === REJECTED) {
      isFunction(onRejected) ? resolve(onRejected(result)) : reject(result)
    }
  } catch (error) {
    reject(error)
  }
}

// 统一处理队列中的函数
const handleCallbacks = (callbacks, state, result) => {
  while (callbacks.length) {
    handleCallback(callbacks.shift(), state, result)
  }
}

// 改变状态
const transition = (promise, state, result) => {
  if (promise.state !== PENDING) return
  promise.state = state
  promise.result = result
  setTimeout(() => {
    handleCallbacks(promise.callbacks, state, result)
  }, 0);
}

// 处理成功和失败的回调
const resolvePromise = (promise, result, resolve, reject) => {
  if (result === promise) {
    let reason = new TypeError('Can not fulfill promise with itself')
    reject(reason)
  }
  if (isPromise(result)) {
    return result.then(resolve, reject)
  }
  if (isThenable(result)) {
    try {
      let then = result.then
      if (isFunction(then)) {
        return new MyPromise(then.bind(result)).then(resolve, reject)
      }
    } catch (error) {
      return reject(error)
    }
  }
  resolve(result)
}
class MyPromise {
  constructor(exector) {
    this.state = PENDING
    this.result = null
    this.callbacks = []

    // 失败和成功的回调
    let onFulfilled = value => transition(this, FULFILLED, value)
    let onRejected = reason => transition(this, REJECTED, reason)

    let ignore = false
    let resolve = value => {
      if (ignore) return
      ignore = true
      resolvePromise(this, value, onFulfilled, onRejected)
    }
    let reject = reason => {
      if (ignore) return
      ignore = true
      onRejected(reason)
    }

    // 执行
    try {
      exector(resolve, reject)
    } catch (error) {
      reject(error)
    }
  }
  then(onFulfilled, onRejected) {
    return new MyPromise((resolve, reject) => {
      let callback = {
        onFulfilled,
        onRejected,
        resolve,
        reject
      }
      if (this.state === PENDING) {
        this.callbacks.push(callback)
      } else {
        setTimeout(() => {
          handleCallback(callback, this.state, this.result)
        }, 0);
      }
    })
  }
  catch (onRejected) {
    return this.then(null, onRejected)
  }
  static resolve(value) {
    return new MyPromise(resolve => resolve(value))
  }
  static reject(reason) {
    return new MyPromise((_, reject) => reject(reason))
  }
  static all(promises) {
    const pLen = promises.length
    const values = new Array(pLen)
    let successCount = 0
    return new MyPromise((resolve, reject) => {
      promises.forEach((promise, index) => {
        MyPromise.resolve(promise).then(
          value => {
            successCount++
            values[index] = value
            if (successCount === pLen) {
              resolve(values)
            }
          }, reason => {
            reject(reason)
          })
      })
    })
  }
  static race(promises) {
    return new MyPromise((resolve, reject) => {
      promises.forEach(promise => {
        MyPromise.resolve(promise).then(
          value => {
            resolve(value)
          }, reason => {
            reject(reason)
          }
        )
      })
    })
  }
  static allSettled(promises) {
    const pLen = promises.length
    const res = new Array(pLen)
    return new MyPromise((resolve, reject) => {
      promises.forEach((promise, index) => {
        Promise.resolve(promise).then(
          value => {
            res[index] = {
              status: 'fulfilled',
              value
            }
            index === pLen - 1 && resolve(res)
          }, reason => {
            res[index] = {
              status: 'rejected',
              reason
            }
            index === pLen - 1 && resolve(res)
          }
        )
      })
    })
  }
}


module.exports = MyPromise
```



## 4.async await

`async` 函数是 `Generator` 函数的语法糖。使用 关键字 `async` 来表示，在函数内部使用 `await` 来表示异步。
想较于 Generator，`Async` 函数的改进在于下面四点：

- **内置执行器**。Generator 函数的执行必须依靠执行器，而 `Aysnc` 函数自带执行器，调用方式跟普通函数的调用一样
- **更好的语义**。`async` 和 `await` 相较于 `*` 和 `yield` 更加语义化
- **更广的适用性**。`co` 模块约定，`yield` 命令后面只能是 Thunk 函数或 Promise对象。而 `async` 函数的 `await` 命令后面则可以是 Promise 或者 原始类型的值（Number，string，boolean，但这时等同于同步操作）
- **返回值是 Promise**。`async` 函数返回值是 Promise 对象，比 Generator 函数返回的 Iterator 对象方便，可以直接使用 `then()` 方法进行调用




[100 行代码实现 Promises/A+ 规范]( https://mp.weixin.qq.com/s/qdJ0Xd8zTgtetFdlJL3P1g )

[Promise 必知必会（十道题）]( https://juejin.im/post/5a04066351882517c416715d )