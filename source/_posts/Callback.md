---
title: Callback
categories:
  - JS核心五分鐘
tags:
  - JavsScript
  - Callback
abbrlink: 2156861040
date: 2021-04-11 19:37:47
---
## Callback Function
Callback Function 和一般函式沒有什麼不同，差別在於被呼叫執行的時機。
JavaScript 是一個「**事件驅動**」的程式語言，也就是說每段程式都必須滿足一定的驅動條件，才會啟動執行程式。
<!--more-->
```javascript
function fn () {
    console.log('This is a function')
}
// 如果沒有呼叫 fn，就不會執行函式
```
所謂的「Callback Function」其實就是「**把函式當作參數，透過另外一個函式呼叫**」。
可以解釋成：希望在某個 **地點**，透過某個 **動作**，執行某個 **程式**，來達成結果。
> 地點.addEventListener('動作', 參數)

我們可以用一段匿名函式的方式把函式帶入參數：
```javascript
window.addEventListener('click', function () {
    alert('This is callback function.')
})
```

也可以用具名函式的方式帶入：
```javascript
function active () {
    alert('This is callback function.')
}
window.addEventListener('click', active)
```
## 控制多個函式間執行的順序
Callback Function 還有另外一個功能，就是「**控制多個函式間執行的順序**」。

```javascript
function funcA () {
    console.log('A')
}
function funcB () {
    console.log('B')
}
funcA()
funcB()
```

範例中定義兩個函式，並且分別啟動 `funcA` 和 `funcB`，此時兩個函式都會被立刻執行。
如果這時用非同步程式加上一段等待時間：
```javascript
function funcA () {
    window.setTimeout(function () {
        console.log('A')
    }, 1000)
}
function funcB () {
    window.setTimeout(function () {
        console.log('B')
    }, 1000)
}
funcA()
funcB()
```
這樣就沒辦法確定 `funcA` 和 `funcB` 誰會先出現。
類似這種情況，為了確保執行順序，就會透過 Callback 的方式來處理。
```javascript
function funcA (callback) {
    window.setTimeout(function () {
        console.log('A')
        callback()
    }, 1000)
}
function funcB () {
    window.setTimeout(function () {
        console.log('B')
    }, 1000)
}
// 將 funcB 作為參數帶入 funcA
funcA(funcB)
```
如此一來就能確保，無論 `funcA` 的等待時間有多長，`funcB` 都會在 `funcA` 結束後才接著執行。
但是如果程式之間相依程度過深，就會造成傳說中的 **波動拳**，也就是「**Callback Hell**」，讓程式難以維護。
![](https://i.imgur.com/L5NnPh5.png)

-----

參考文章：[重新認識 JavaScript: Day 18 Callback Function 與 IIFE](https://ithelp.ithome.com.tw/articles/10192739)


