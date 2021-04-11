---
title: 閉包 Closure
categories:
  - JS核心五分鐘
tags:
  - JavsScript
  - Closure
abbrlink: 1553745186
date: 2021-04-11 19:37:35
---
讓函式擁有自己的變數，使執行環境不會互相干擾。

-----

我們都知道閉包，但什麼是必包？主要功能是什麼呢？
先從字面上拆解，Closure 可以拆成 `Close` 和 `-ure` 字尾，`-ure` 接在動作的字根後面，表示動作本身、動作造成的結果、或是執行動作的人事物。
所以閉包本身就是一個關閉的動作，把程式碼片段關閉在一個函式內，限制作用域，建立自己的環境與外界隔絕。

<!--more-->

## 限制作用域
用一個錢包範例先來比較一下差異：
假設每個錢包都有1000元，大明花了300元，小明花了500元，計算每個人錢包裡的餘額
```javascript
var balance = 1000
function wallet (cost) {
  return balance = balance - cost
}
var MING = wallet(300)
var ming = wallet(500)
console.log(MING, ming) // 700 200
```
快速計算一下，大明錢包裡應該要有700元，小明應該還剩500元，但是結果小明卻是剩下200元，難道大明偷拿小明錢包裡的錢嗎？
一切都是誤會呀！因為現在沒有限制作用域，目前還是兩個人共用同一個錢包的狀態，如果想要讓他們他們各自花各自的錢，就要給他們一人一個錢包。
```javascript
var balanceA = 1000
var balanceB = 1000
function walletA (cost) {
  return balanceA = balanceA - cost
}
function walletB (cost) {
  return balanceB = balanceB - cost
}
var MING = walletA(300)
var ming = walletB(500)
console.log(MING, ming) // 700 500
```

## 函式工廠
如果今天想要再計算別的錢包，就要再重複寫一次，這樣就會變成一樣的事情重複寫，不但看起來很複雜，也很沒有效率。這個時候就可以加入閉包限制作用域，建立一個「**函式工廠**」。
**讓每個功能相同的 function，對應到每個獨立的環境變數，且互不相干擾。**
```javascript
function wallet(cost) {
  var balance = 1000
  function counter () {
    balance = balance - cost
    console.log(balance)
  }
  counter()
}
var userA = wallet(300) // 700
var userB = wallet(500) // 500
var userC = wallet(200) // 800
```
這時候就可以讓每個人都可以獨立計算錢包餘額，也可以加入變數，一開始分別放入不同金額，管理各自的錢包。
```javascript
function wallet(initBalance, cost) {
  var balance = initBalance
  function counter () {
    balance = balance - cost
    console.log(balance)
  }
  counter()
}
var userA = wallet(1000, 300) // 700
var userB = wallet(2000, 500) // 1500
var userC = wallet(1500, 200) // 1300
```

-----

參考文章：
* [重新認識 JavaScript: Day 19 閉包 Closure](https://ithelp.ithome.com.tw/articles/10193009)
* [快速上手JavaScript的閉包Closure](https://yixuntseng-bruce.medium.com/%E4%BA%94%E5%88%86%E9%90%98%E5%AD%B8%E5%89%8D%E7%AB%AF-%E5%BF%AB%E9%80%9F%E4%B8%8A%E6%89%8Bjavascript%E7%9A%84%E9%96%89%E5%8C%85closure-c54321434e9f)