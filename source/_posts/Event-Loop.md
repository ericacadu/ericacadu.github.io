---
title: Event Loop
abbrlink: 712608437
date: 2021-04-16 13:46:04
categories:
  - JS核心五分鐘
tags:
  - JavaScript
---
## 執行堆疊 Call Stack
JavaScript 是一種單執行緒的程式語言，也就是一次只能做一件事情，程式碼也是一行一行執行。
當呼叫函式時會產生執行環境，如果函式執行環境內還有其他函式被呼叫，就會產生另外一個執行環境，形成「**執行堆疊**」(Call Stack)，在上層執行環境結束之前，下層包含全域的執行環境內的程式碼就會被暫停。

<!--more-->

而堆疊的函式會以「後進先出」順序執行，當最上層的函式最後被堆疊上去，就會被最先執行，執行完後就會被移出堆疊環境，接著才繼續執行下一層函式。
![Image](https://i.imgur.com/SORNjlE.png?70)

因此如果堆疊太多執行環境，或某個堆疊執行過久，就有可能影響整個執行環境的運行，這種狀況稱為「**阻塞**」(Blocking)。
以下方範例來說，如果在函式內不斷呼叫自己，就會呈現一個無止盡的 callback hell 波動拳，`foo(foo(foo(...))))`，這個時候瀏覽器為了避免阻塞太久，就會先暫停執行並且跳出錯誤提示 `Uncaught RangeError: Maximum call stack size exceeded`
```javascript
function foo () {
    foo()
}
foo()
```

## 事件佇列 Event Queue
JavaScript 一次只能執行一件事情，我們可以同時執行很多件事情，是因為瀏覽器不只提供執行環境，還提供很多「**WebAPI**」可以使用。
為了避免阻塞發生，瀏覽器會先把非同步事件透過 WebAPI 先放到「**事件佇列**」中，等到堆疊的程式碼都執行結束後，才會把事件佇列裡的任務再放進 stack 裡執行。
* 同步：一次一件事情，依序執行
* 非同步：很多件事情同時開始，不同時間結束

![Image](https://i.imgur.com/ECdJoKe.png?70)

## Event Loop
那究竟什麼是 Event Loop 呢？
可以把 Event Loop 想像成是運行整個執行環境流程的執行程式，他負責檢查 stack 裡的事件是否被清空，如果是空的，就會到 Event Queue 檢查任務，再放到 Stack 裡去執行。

---

## 參考文章
* [Javascript 的事件循環 (Event Loop)、事件佇列 (Event Queue)、事件堆疊 (Call Stack)：排隊](https://medium.com/itsems-frontend/javascript-event-loop-event-queue-call-stack-74a02fed5625)
* [JS 原力覺醒 Day13 - Event Queue & Event Loop 、Event Table](https://ithelp.ithome.com.tw/articles/10221944)

