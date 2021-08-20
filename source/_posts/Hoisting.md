---
title: 提升 Hoisting
abbrlink: 4175334150
date: 2021-08-20 17:17:22
categories:
- JS核心五分鐘
tags:
- JavaScript
---
在 **創造環境** 將記憶體空間準備好的流程，稱為 **提升 (Hoisting)** 。
<!--more-->

先看以下三段程式碼範例：
在正常情況下，我們會先宣告變數，接著再使用或查看變數。
```javascript
var Ming = '小明'
console.log(Ming) // 小明
```
如果沒有事先宣告的話，就會出現變數尚未被定義的錯誤。
```javascript
console.log(Ming) // Ming is not defined
```
但是以下程式碼在查看 Ming 之後才宣告變數，卻會得到 undefined 的結果。
```javascript
console.log(Ming) // undefined
var Ming = '小明'
```
這是因為 JavaScript 在執行的時候會分為 **創造** 和 **執行** 兩個階段。

<div class="alert alert-success">
在創造階段時，會先把全域環境下的所有變數都先存進記憶體，如果在此時查看，便會出現 undefined 的結果，接著在執行階段時才會將變數賦值。
</div>

因此第三個範例可以這樣拆解：
```javascript
// 創造階段：先創造記憶體，但尚未賦值
var Ming

// 執行階段：在賦值前就先查看，所以會出現 undefined 的結果
console.log(Ming)
Ming = '小明'
```


## 函式的提升
**函式陳述式** 在創造階段就會先準備好記憶體和函式，因此可以在任何地方呼叫函式。
```javascript
callName() // 小明
function callName() {
    console.log('小明')
}
```
拆解函式：
```javascript
// 創造
function callName() {
    console.log('小明')
}
// 執行
callName()
```

## 函式優先
<div class="alert alert-info">
在創造階段，函式優先，然後才是變數
</div>

### 函式表達式 1
如果使用 **函式表達式**，因為 **函式優先** 的原則，會有不一樣的結果。
```javascript
callName()
var callName = function() {
    console.log('小明')
}
// callName is not a function
```
在 **函式表達式** (運算式)中，因為先宣告一個變數，在創造階段會先準備記憶體但還不會先賦值，若在此時執行函式，JavaScript 會判斷變數不是一個函式，直到執行階段時才會把函式指派給變數。

拆解函式：
```javascript
// 創造
var callName

// 執行：在執行階段，才會把函式內容指派給變數
callName()
callName = function() {
    console.log('小明')
}
```
---
### 函式表達式 2
```javascript
var callName = function() {
    console.log('小明1')
}
function callName() {
    console.log('小明2')
}
callName()
// 小明1
```

拆解函式：
```javascript
// 創造階段：函式優先於變數
function callName() {
    console.log('小明2')
}
var callName

// 執行
callName = function() {
    console.log('小明1')
}
callName()
```

<br>

## 為什麼需要 Hoisting?
Hoisting 最初的設計是為了實現函式互相傳遞，畢竟在開發時會不只有一個函式，如果每個函式都要事先宣告的話，不僅繁瑣而且不好使用，如果有 Hoisting 的話就可以在任何地方呼叫函示。

<br>

## undefined 與 not defined
* undefined：記憶體已建立，但尚未給予值，屬於不明確狀態
* not defined：尚未定義變數，所以記憶體尚未建立

<div class="alert alert-danger">
宣告變數時應避免定義 undefined，若需使用不確定變數，則可使用 null
</div>

-----

## 參考文章
* [JavaScript 核心篇](https://www.hexschool.com/courses/js-core.html)
* [我知道你懂 hoisting，可是你了解到多深？](https://blog.techbridge.cc/2018/11/10/javascript-hoisting/)


