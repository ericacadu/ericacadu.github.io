---
title: var、let、const
categories:
  - JS核心五分鐘
tags:
  - JavsScript
abbrlink: 659913096
date: 2021-04-11 19:23:04
---
## var
`var` 的宣告屬於 **全域變數**，代表 **全域物件的屬性**。
```javascript
for (var i=0; i<3; i++) {}
console.log(i) // 3
```
<!--more-->
假設一個 `for` 迴圈用 `var` 宣告 `i`，在全域還是可以取得 `i` 值。
如果想要限制 `i` 的作用域的話，可以使用立即函式，避免外層取到 `for` 迴圈裡面的值，最直接的方式就是使用 `let` 宣告。
```javascript
(function () {
    for (var i=0; i<3; i++) {}
})()
console.log(i) // i is not defined
// ---
for (let i=0; i<3; i++) {}
console.log(i) // i is not defined
```
在 JavaScript 中，`var` 的作用域是函式作用域，但在函式內的區域變數也可能被視為全域變數，傳統用 `var` 宣告變數可能會汙染全域，如果使用 `let`、`const` 就可以避免這種情形。
```javascript
if (true) {
    var a = 10
}
console.log(a) // 10
console.log(window.a) // 10
```

## let
`let` 的作用域屬於區塊內({} 大括號)，和 `const` 最大差異就是可以重新賦值，而 `const` 不行。
```javascript
let a = 0
{
    let a = 1 // 因為 let 的作用域是區塊內，所以不算重複宣告
    a = 2
    console.log(a) // 2
}
console.log(0) // 0
```
舉個非同步 `for` 迴圈的例子，假設想要讓 `setTimeout` 依序執行並印出結果，如果用 `var` 宣告的話，因為 `var` 是全域變數，所以只會取出最後一次執行的結果。
而 `setTimeout` 是非同步程式，會先放到「事件佇列」裡面，等到所有程式碼執行完畢後才會執行。
如果想要達到預期結果，只要用 `let` 宣告就可以依序執行並取得正確數值。
```javascript
for (var i=0; i<3; i++) {
    setTimeout(function () {
        console.log(`這是第 ${i} 次執行結果`)
    }, 0)
} // 這是第 3 次執行結果
---
for (let i=0; i<3; i++) {
    setTimeout(function () {
        console.log(`這是第 ${i + 1} 次執行結果`)
    }, 0)
}
// 這是第 0 次執行結果
// 這是第 1 次執行結果
// 這是第 2 次執行結果
```

## const
`const` 是宣告一個常數，用 `const` 宣告的變數沒辦法再被調整。
雖然變數沒辦法被調整，但如果是物件的話，因為物件屬於傳參考特性，只要不直接替換掉物件，可以修改內容屬性值。
```javascript
const person = {
    name: '小明',
    money: 500
}
person.name = '大明'
console.log(person.name) // 大明
// ---
person = {}
console.log(person) // Assignment to constant variable.
```

## Hoisting 與 暫時性死區
JavaScript 中有 hoisting 的現象，先來比較一下這三種的狀況：

```javascript
console.log(a) // undefined
var a = 1
// ---
console.log(b) // b is not defined
let b = 2
// ---
console.log(c) // Cannot access 'c' before initialization
const c = 3
```
1. var: `undefined`，有 hoisting
2. let: `not defined`，沒有 hoisting
3. const: 宣告前無法使用，沒有 hoisting

看起來沒什麼問題，接著猜測一下面範例結果：
* (1) 如果 `let` 沒有提升的話，就會往外層查找取得 **小明** 的值
* (2) 如果 `let` 有提升的話，就會出現其他狀況

```javascript
let Ming = '小明'
{
    console.log(Ming)
    let Ming = '大明'
}
```
結果是(2)： `Cannot access 'Ming' before initialization`
`let` 在提升的過程中會產生一個「暫時性死區」Temporal Dead Zone(TDZ)。
在這個區域內是無法存取變數，也就不會賦予 `undefined` 的值，如果試圖取值就會跳出錯誤提示。
所以 `let` 雖然有類似提升 **創造 - 執行階段** 的概念，但是和 `var` 的 hoisting 概念並不相同。
```javascript
let Ming = '小明'
{
    // 創造
    let Ming // 暫時性死區 TDZ
    
    // 執行
    console.log(Ming)
    let Ming = '大明'
}
```

## 重點整理
||作用範圍|重複宣告|重新賦值|Hoisting|
|:---:|:---:|:---:|:---:|:---:|
|var   | 全域 |  v  |  v  |  v  |
|let   | 區塊 |  x  |  v  |  v  |
|const | 區塊 |  x  |  x  |  x  |