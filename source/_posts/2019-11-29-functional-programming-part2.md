---
title: 函式程式設計 (二) (Functional Programming Part2)
categories: JavaScript
tags:
  - 克服 JavaScript 奇怪部分筆記
comments: true
date: 2019-11-29
---

[Underscore.js](https://underscorejs.org/)，是非常有名的 JavaScript 函式庫，可以幫助我們處理陣列和物件，還展示如何做出那些功能，這和我們之後要講的東西有關。

我們稱這為**開源教育 (open source education)**，有免費、大量的 JavaScript 函式庫的程式碼供我們使用，這些被廣泛使用的框架和函式庫的原始碼都非常不錯，可以從中學習原始碼，不過還是得先了解 JavaScript 的進階概念才行。

## 下載

Underscore.js 可以下載 [Development Version](https://underscorejs.org/underscore.js) 和 [Production Version](https://underscorejs.org/underscore-min.js)。

我們要下載 Development Version，它含有很多的註解，可以透過註解了解程式是如何運作，或是到 [Annotated Source](https://underscorejs.org/docs/underscore.html)，它讓註解和程式碼分離，可以直接看 underscore.js 的程式碼。

underscore.js 使用很多函式程式設計的概念，我們會看到它不斷傳入 `iteratee` 或 `predicate` 讓 function 能夠運作，其他的 function 也做了許多很棒的事。

## Lodash

也有另一個函式庫叫做 [Lodash.js](https://lodash.com/)，和 underscore 類似，但它有實作額外的功能，執行速度也比 underscore 快上許多。雖然兩者的重複性高，但**比較推薦選擇 underscore 來學習，因為它的程式碼有註釋**，是相當不錯的教材。

> underscore 比 lodash 早出來，它提供原始碼給大家觀看，儘管 lodash 的程式效能比較好，但改進一個東西，比創造它還要簡單的多。

## 使用 underscore 函式庫

**先下載 underscore.js 到專案中**
在 underscore.js，我們能看到它的程式被包在 IIFE 中，所以裡面的程式碼不會和我們寫的程式產生衝突。

```JavaScript
// underscore.js
(function() {
  // ....
}());
```

**新增 index.html，引入 underscore.js\*\***
underscore 的 IIFE 包住整個 underscore 程式碼，執行後，我們在全域物件中便有物件能使用，物件和全域物件會最小化我們需要做的事，只要使用 `_` (物件的名稱，在 JavaScript 是有效的) 便能使用 underscore 的函式庫。

```HTML
<body>
  <script src="underscore.js"></script>
  <script src="app.js"></script>
</body>
```

### `_.map()`

如果我們要做類似之前的 map 功能，我們新增一個陣列 `arr6`，然後給它 `_.map()`，並傳入陣列和要運算的 function，並讓 function 回傳 `item * 5`，執行完會得到新的陣列。

> underscore 的 map 比我們上一章節寫的 map 好很多，它能在更多不同情境使用。

```JavaScript
var arr6 = _.map(arr1, function (item) {
  return item * 5;
});

console.log('arr6: ' + arr6);
```

結果如下：

```JavaScript
arr6: [5, 10, 15]
```

### `_.filter()`

我們還能使用 `_.filter()` 過濾陣列 (會回傳新的陣列)，傳入陣列和 function，function 會回傳可被 `2` 整除的項目，也就是說，會回傳 `item % 2 === 0` 為 `true` 的 `item` 回來。

```JavaScript
var arr7 = _.filter([2, 3, 4, 5, 5, 6, 6], function (item) {
  return item % 2 === 0;
});

console.log('arr7: ' + arr7);
```

結果如下：

```JavaScript
arr7: [2, 4, 6, 6]
```

## 總結

underscore 和 lodash 函式庫提供我們許多的方法，他們都有使用函式程式設計的概念，摸索一下它們，看看它們的原始碼，會學到很多的東西。我們可以應用它們的概念到自己的程式中，該如何用 first-class function 的優點寫出乾淨的程式，讓其他程式設計師更容易地重複使用，就能寫更少的程式。
