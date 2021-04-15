---
title: This
categories:
  - JS核心五分鐘
tags:
  - JavaScript
abbrlink: 1591487769
date: 2021-04-12 11:33:43
---
## this 是什麼
this 是 JavaScript 的一個關鍵字，函式執行時自動生成的一個內部物件，用來呼叫自身擁有的屬性或值。
```javascript
function fn () {
    console.log(this)
    debugger
}
fn() // window
```
![Image](https://i.imgur.com/kIAxMWk.png)
<!--more-->
---
## this 指向
`this` 的值跟「作用域」或「程式碼位置」在哪裡 <font color="red">完全無關</font>，只跟「<font color="red">**如何呼叫**</font>」有關。
```javascript
var scope = '全域'
function func () {
    console.log(this.scope)
}
func() // 全域
```
## 影響函式 this 的調用方式
* 作為物件方法
* 簡易呼叫 simple call (不建議使用 this)
* bind、apply、call 方法
* new
* DOM 事件處理器（用 `console.dir()` 取代 `console.log()`）
* 箭頭函式

## 物件方法
不管函式如何被定義，只管如何被執行，以下範例為例：
執行 `obj.func()`，因為在物件底下被呼叫，所以 `this` 指向物件。
如果另外宣告 `var newFunc = obj.func`，再執行 `newFunc()`，因為在全域底下被執行，所以 `this` 指向全域。
```javascript
var scope = '全域'
function func () {
    console.log(this.scope)
}

var obj = {
    scope: '物件',
    func: function () {
        console.log(this.scope)
    }
}
obj.func() // 物件

var newFunc = obj.func
newFunc() // 全域
```

## 簡易呼叫 Simple Call
用 simple call 的方式調用時，如果沒有另外定義 `this`，此時 `this` 指的就是全域。
```javascript
var scope = '全域'
const obj = {
    scope: '物件',
    func () {
        function func2 () {
            console.log(this.scope)
        }
        func2() // simple call // 全域
    }
}
obj.func() // 全域
```
雖然 simple call 的 `this` 是指向全域，但並不是在 `window` 的物件下去執行函式。
所以如果使用立即函式的話，`window.func()` 就會出現錯誤。
```javascript
var scope = 'window';
(function () {
    console.log(this.scope)
    function func () {
        console.log(this.scope)
    }
    window.func()
    // window.func is not a function
})()
```
另外 simple call 還有其他形式，像是閉包、callback function 等都是屬於 simple call。
### 閉包 Closure
```javascript
var scope = 'window'
function closureFunc () {
    return function () {
        console.log(this.scope)
    }
}
var newFunc = closureFunc()
newFunc() // window

```

### callback
將一個函式以參數的方式帶入另一個函式，並在另一個函式內執行。
```javascript
var scope = 'window'
function func (callback) {
    return callback()
}
func(function () {
    console.log(this.scope)
}) // window
```

### forEach
`forEach` 也是 callback function 的一種，所以也是屬於 simple call。
```javascript
var scope = 'window'
var a = [1, 2, 3]
a.forEach(function (i) {
    console.log(this.scope)
})
// window window window
```

### setTimeout
`setTimeout` 也是 callback function 的一種，所以也是屬於 simple call。
```javascript
var scope = 'window'
var obj = {
    scope: 'local',
    func: function () {
        setTimeout(function () {
            console.log(this.scope)
        }, 0)
    }
}
obj.func() // window
```
## 強制綁定 this
雖然 this 有自己的預設值，但仍然可以用一些方法改變 this 的值。
綁定方式有三種：`call()`、`apply()`、`bind()`
`call()` 和 `apply()` 很類似，差別在於 `call()` 是「依序」傳入參數， `apply()` 是傳入陣列。
```javascript
'use strict'
function func (a, b) {
    console.log(this, a, b)
}

func(1, 2) // 1, 2
func.call(undefined, 1, 2) // undefined, 1, 2
func.apply(undefined, [1, 2]) // undefined, 1, 2
```
由以上範例可以得知 `call()` 和 `apply()` 的第一個參數指的就是 this 的值。
所以如果把 `undefined` 改成其他內容，this 的值就會被替換掉。
```javascript
'use strict'
func.call('call', 1, 2) // call, 1, 2
func.apply('apply', [1, 2]) // apply, 1, 2
```

`call()` 和 `apply()` 都可以立刻執行，但是 `bind()` 不會，並須在調用時才會「依序」執行。
而且一旦 `bind()` 之後，this 的值就不會再改變。
```javascript
'use strict'
var func2 = func.bind('bind', 1, 2)
func2() // bind, 1, 2
func2.call('call', 1, 2) // bind, 1, 2
```
> 在非嚴格模式下，如果參數為 `null`、`undefined`，this 會自動轉成全域物件，this 會被指向 window，並且帶入的值會以建構式的方式呈現。
> 但是如果在嚴格模式下，就會保持原本的參數類型，this 的本質其實是 undefined，所以盡量不要用 simple call 的方式調用 this。

## 箭頭函式
箭頭函式沒有自己的 `this`，`this` 會指向外層的作用域。
以傳統函式的 `setTimeout` 為例，當執行 `setTimeout` 時，因為是 `callback`函式的關係，所以 `this` 會指向全域。
```javascript
var scope = '全域'
var obj = {
    scope: '物件',
    func: function () {
        setTimeout(function () {
            console.log(this.scope)
        }, 0)
    }
}
obj.func() // 全域
```
如果改成箭頭函式的話，`this` 會被限制在作用域內，所以會指向物件。
```javascript
var scope = '全域'
var obj = {
    scope: '物件',
    func: function () {
        setTimeout(() => {
            console.log(this.scope)
        }, 0)
    }
}
obj.func() // 物件
```
**＊陷阱**
箭頭函式的 `this` 要看外層是否有函式，如果沒有，則 `this` 指向全域。
以下範例 `callName()` 外層沒有函式，所以會指向全域。
```javascript
var name = '全域'
const person = {
  name: '小明',
  callName: () => { 
    console.log(this.name)
  },
}
person.callName() // 全域
```

---

## 參考文章
* [What's THIS in JavaScript ?](https://kuro.tw/posts/2017/10/17/What-s-THIS-in-JavaScript-%E4%B8%AD/)
* [淺談 JavaScript 頭號難題 this：絕對不完整，但保證好懂](https://blog.techbridge.cc/2019/02/23/javascript-this/)