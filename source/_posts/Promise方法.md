---
title: Promise 方法
categories:
  - JS核心五分鐘
tags:
  - JavsScript
  - Promise
abbrlink: 1110903207
date: 2021-04-11 19:39:10
---
`Promise` 是用來解決 AJAX 非同步問題，優化非同步程式的 ES6 語法，會回傳「**接受**」或「**拒絕**」的結果。
> AJAX 是 JavaScript 與 XML 技術的縮寫，網頁不用重新整理，就能即時透過瀏覽器跟伺服器溝通、撈取資料。

```
function promise(num) {
  return new Promise((resolve, reject) => {
    num ? resolve(num) : reject('失敗')
  })
}
```
<!--more-->

-----

以下整理一些 Promise 方法：
## Promise.resolve 和 Promise.reject
`Promise.resolve` 和 `Promise.reject` 都是直接定義 `Promise` 物件已完成的狀態，會產生一個新的 `Promise` 物件。
```javascript
const result = Promise.resolve('result')
result.then(res => {
    console.log(res, 'success') // 成功部分可以正確接收結果
}).catch(res => {
    console.log(res, 'fail') // 失敗部分不會取得結果
})

// ---

const result = Promise.reject('result')
result.then(res => {
    console.log(res, 'success')
}).catch(res => {
    console.log(res, 'fail') // 只會取得失敗結果
})
```

## Promise.race
`Promise.race` 會透過陣列執行多個 `Promise`，但僅會回傳第一個結果，無論結果為成功或失敗。

## Promise.all
`Promise.all` 會透過陣列傳入多個 `Promise` 函式，在「**全部執行完後**」回傳陣列局果，並且順序與一開始傳入一致，適合「**多支API一起執行**」，確保全部完成後才進行其他工作。
```javascript
Promise.all([promise(1), promise(2), promise(3)])
    .then((res) => {
        console.log(res) // ["1", "2", "3"]
    })
```

## Fetch
`Fetch` 會使用 ES6 的 Promise 作回應（then、catch），回傳的資料為 `ReadableStream` 物件，需要使用不同資料類型的應對方法，才能正確取得資料。

`ReadableStream` 的資料類型有：`text()`、`json()`、`formData()`、`blob()`、`arrayBuffer()`

```javascript
const url = 'https://raw.githubusercontent.com/hexschool/hexschoolNewbieJS/master/data.json'

fetch(url)
    .then(res => {
        return res.json() // 先轉成 json 格式
    })
    .then(res => {
        console.log(res) // 再接收轉換後的資料
    })
    .catch(res => {
        console.log(res)
    })
```

-----

## 參考文章
* [JavaScript Promise 全介紹](https://wcc723.github.io/development/2020/02/16/all-new-promise/)
* [淺談JavaScript ES6入門Part2-類別、物件與建構式](https://medium.com/@brianwu291/learn-basic-javascript-es6-part2-d8fe175107c3)
* [從Promise開始的JavaScript異步生活](https://eyesofkids.gitbooks.io/javascript-start-es6-promise/content/)
* [ES6 Fetch 遠端資料方法](https://ithelp.ithome.com.tw/articles/10194388)
* [JavaScript Fetch API 使用教學](https://www.oxxostudio.tw/articles/201908/js-fetch.html)

