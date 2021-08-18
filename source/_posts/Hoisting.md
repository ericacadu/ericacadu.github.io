---
title: Hoisting
abbrlink: 4175334150
date: 2021-04-11 18:17:22
categories:
- JS核心五分鐘
tags:
- JavaScript
---
在 JavaScript 中，把宣告的 **變數** 和 **函式** 拉升到最上層，在被呼叫執行之前先存進記憶體，這個現象就叫做 「提升（hoisting）」。

<!--more-->

-----

一般來說，我們都會事先宣告變數，接著才會執行。
如果沒有宣告就執行的話，會得到 `ReferenceError: a is not defined` 的錯誤，因為 JavaScript 找不到這個名為 `a` 的變數。
```javascript
console.log(a) // 'a is not defined'
```
假設一段程式在執行後接著宣告變數，依照上面敘述答案應該是 `a is not defined`，但是結果卻是 `undefined` ，Why ?
```javascript
console.log(a) // undefined
var a
```
JavaScript 的運行分為「創造階段」和「執行階段」。
在創造環境時，會將宣告的變數拉升到最上面，這個現象就叫做提升。
雖然理解上是移動到最上面，實際上位置並沒有改變，也可以說是靠「想像」把變數移動到最上面，而移動的行為就是把變數存進記憶體裡。
要特別注意變數宣告和賦值不同，雖然提升了宣告，但賦值卻會留在原地「依序」等待被執行。
```javascript
console.log(a)
var a = 2

// 創造階段：
var a

// 執行階段：
console.log(a) // undefined
a = 2
```
除了變數宣告外，「函式陳述式」也會被提升到創造階段，並且優先載入。
```javascript
function fn () {
  console.log('apple')
}
fn()
function fn () {
 console.log('orange')
}
fn()
```
拆解：
```javascript
// 創造階段：函式優先
function fn () {
  console.log('apple')
} 
function fn () {
  console.log('orange')
}
// 執行階段：
fn() // 第一次執行：orange
fn() // 第二次執行：orange
```
如果是「函式表達式」又是另外一種狀況：在執行階段，才會把函式內容指派給變數。
```javascript
var fn = function () {
  console.log('小明')
}
fn()
function fn () {
 console.log('orange')
}
fn()
```
拆解：
```javascript
// 創造階段：函式優先
function () {
  console.log('apple')
}
function fn () {
 console.log('orange')
}
var fn

// 執行階段：
fn = function () {
  console.log('apple')
}
fn() // 第一次執行：apple
fn() // 第二次執行：apple
```
## 為什麼需要 Hoisting?
Hoisting 最初的設計是為了實現函式互相傳遞，畢竟在開發時會不只有一個函式，如果每個函式都要事先宣告的話，不僅繁瑣而且不好使用，如果有 Hoisting 的話就可以在任何地方呼叫函示。

-----

## 參考文章
* [我知道你懂 hoisting，可是你了解到多深？](https://blog.techbridge.cc/2018/11/10/javascript-hoisting/)


