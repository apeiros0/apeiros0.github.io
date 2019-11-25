---
title: 了解閉包 (二) (Understanding Closures Part 2)
categories: JavaScript
tags:
  - 克服 JavaScript 奇怪部分筆記
comments: true
date: 2019-11-24
---

今天我們要來介紹 Closure 的經典例子。

## Closure 經典例子

我們先建立一個 buildFunctions function，並在當中建立 arr 的空陣列和 for 迴圈，在迴圈中我們使用 `.push()` 加入 function 到陣列。

而在 function 中我們輸出 `i`，`i` 會透過範圍鏈參照到，這樣會執行三次，並新增三個函式表達式 (函式物件) 給陣列，這樣陣列就會有三個相同的 function，最終 buildFunctions 會回傳 arr。

> 陣列是任何東西的集合，我們可以加入 function 到陣列中。

```JavaScript
function buildFunctions() {
  var arr = [];
  for (var i = 0; i < 3; i++) {
    arr.push(
      function () { console.log(i); }
    );
  }
  return arr;
}
```

現在我們呼叫 buildFunctions 來取得陣列，賦予到 fs 的變數中，而在陣列中有 function，我們能透過陣列來呼叫它們，來猜猜這三個 function 輸出的 i 分別會是什麼 ?

> 我們要記得，在 `arr.push` 時，不是在呼叫 function，只是在建立它，然後放到陣列中，function 是在別處呼叫的。

```JavaScript
var fs = buildFunctions();

fs[0]();
fs[1]();
fs[2]();
```

當 function 呼叫後，會去尋找 `i`，會透過範圍鏈參照到 `i`，而這裡是讓大家都驚訝的部分，當我們看這個程式碼，會預期輸出 `0`, `1`, `2`，但結果全都是 `3`，為什麼參照到的 `i` 都是 `3` ?

### 產生的問題

這是我們的程式碼，我們有 buildFunctions function，會把函式表達式加入到陣列中，然後在下面呼叫 function，所以我們會有陣列包含三個函式表達式，每個函式表達式會輸出 `i`，當我們執行都會輸出 `3`。

```JavaScript
function buildFunctions() {
  var arr = [];
  for (var i = 0; i < 3; i++) {
    arr.push(
      function () { console.log(i); }
    );
  }
  return arr;
}

var fs = buildFunctions();

fs[0]();
fs[1]();
fs[2]();
```

### 發生什麼事

我們來看看執行堆發生什麼事。

我們知道全域執行環境會最先被建立，它包含 buildFunctions function 和 fs 變數。

```
-----------------------------------------------------
|  Global Execution Context  |  buildFunctions | fs |
-----------------------------------------------------
```

當執行到 `fs = buildFunctions()` 時，會呼叫 buildFunctions，建立新的執行環境，裡面會有兩個變數，分別是 `i` (在 for 迴圈建立) 和 `arr` (在 function 中宣告)。

然後當 for 迴圈執行時，`i` 一開始會是 0，會將函式表達式放到陣列中，這部分就是大家常常搞混的地方，我們要知道這裡的 function 是不會執行的，只是建立函式物件而已，之後繼續執行，`i` 會變成 `1`，因為 `i++` 的關係，然後新增新的函式表達式到陣列中，之後 `i` 變成 `2`，又執行一次，最後 `i++` 執行，`i` 為 `3`，JavaScript 引擎看到 `i < 3`，發現 `i` 不小於 `3`，離開 for 迴圈。

這裡 `i` 離開時值是 `3`，當執行到 `return arr` 時，在記憶體中，`i` 為 `3`，陣列裡會有三個 function (匿名的，用 `f0`, `f1`, `f2` 會比較好辨認)。

```
-----------------------------------------------------
|   buildFunctions()         |  i  |  arr           |
|  Execution Context         |  3  |  [f0, f1, f2]  |
-----------------------------------------------------
|  Global Execution Context  |  buildFunctions | fs |
-----------------------------------------------------
```

然後我們回到全域執行環境中，buildFunctions 就會離開執行堆，還記得我們學過的 Closure 嗎 ?

JavaScript 會幫我們保留可以取用到的變數，記憶體仍存在這兩個變數。

```
                             ------------------------
                             |  i  |  arr           |
                             |  3  |  [f0, f1, f2]  |
-----------------------------------------------------
|  Global Execution Context  |  buildFunctions | fs |
-----------------------------------------------------
```

然後到第一個呼叫 function 的地方，執行 `fs[0]`，新的執行環境建立，由於沒有變數 `i`，所以透過範圍鏈尋找，到 buildFunctions 的執行環境的記憶體內找到 `i`，`i` 為 `3`，所以就輸出 `3`，然後離開執行堆。

```
-----------------------------------------------------
|                     fs[0]()                       |  ---
|                Execution Context                  |    |
-----------------------------------------------------    |
                             |  i  |  arr           |    |
                             |  3  |  [f0, f1, f2]  |  <--
-----------------------------------------------------
|  Global Execution Context  |  buildFunctions | fs |
-----------------------------------------------------
```

接著執行 `fs[1]`，執行環境建立，與 `fs[0]` 參照相同的外部環境，因為和 `fs[0]` 同樣都建立在 buildFunctions 內，所以當 `fs[1]` 尋找 `i`，`i` 也是 `3`，所以會輸出 `3`，當然，最後的 `fs[2]` 也是一樣的。

```
-----------------------------------------------------
|                     fs[1]()                       |  ---
|                Execution Context                  |    |
-----------------------------------------------------    |
                             |  i  |  arr           |    |
                             |  3  |  [f0, f1, f2]  |  <--
-----------------------------------------------------
|  Global Execution Context  |  buildFunctions | fs |
-----------------------------------------------------
```

這就像父母有三個小孩一樣，你問他們的父母幾歲，他們不會因出生的時候不同，而回答不一樣的年齡，他們會給你相同的答案，同樣的這三個 function，當我們執行它們時，會告訴我們外部環境參照到父環境 (parent context) 在記憶體中的值，所以這三個輸出的值都是一樣的，因為它們參照到相同的記憶體位置。

另外，當我們執行 function，仍可以取得它的外部變數，也稱為自由變數 (free variable)。

> 自由變數 (Free Variable) 是在 function 外，但仍能取用的變數。

### 輸出 `0`, `1`, `2`

1. 透過 ES6 的 `let` 變數

`let` 的範圍是在 `{}` 中，每次 `for` 迴圈執行時，`let` 在記憶體中會是新的變數，所以當 function 被呼叫時，在執行環境中 `let` 會是不同的記憶體位置，每次都會指向不同的記憶體，它們本質上是不同的變數。

```JavaScript
function buildFunctions2() {
  var arr = [];
  for (var i = 0; i < 3; i++) {
    let j = i;
    arr.push(
      function () { console.log(j); }
    );
  }
  return arr;
}

var fs = buildFunctions2();

fs[0]();
fs[1]();
fs[2]();
```

2. ES5 的 IIFE

為了保存 `i` 的值，我們需要將 `push` 到陣列的函式表達式的執行環境分開，需要父作用域 (Parent Scope) 保留迴圈執行時當前的 `i` 值，而唯一獲得執行環境的方法是執行 function，那我們該如何立即執行 function 呢 ?

**沒錯，就是 IIFE**

當我們在 `push` 時，立即呼叫 function，將 `i` 作為參數傳入到 IIFE 中，而 IIFE 則透過 `j` 來接收 `i` 的值，每次迴圈執行時，會立即執行 function，建立自己的執行環境，`j` 會分別存在這三個執行環境中，所以會分別有 `j = 0`, `j = 1`, `j = 2` 的執行環境，即使執行環境只執行一行便結束，我們仍有 Closure 會保留 `j` 的值。

透過 IIFE 回傳函式表達式，這個函式表達式會被 `push` 到陣列中，而當他們被呼叫時，不需要到迴圈中找值，只要到執行環境中找便可，`j` 的值會在迴圈執行時儲存，這是利用 Closure 的優勢，確保呼叫時能取得正確的值。

```JavaScript
function buildFunctions2() {
  var arr = [];
  for (var i = 0; i < 3; i++) {
    arr.push(
      (function (j) {
        return function () { console.log(j); }
      }(i))
    );
  }
  return arr;
}

var fs = buildFunctions2();
fs[0]();
fs[1]();
fs[2]();
```

## 總結

我們應用了之前所學的所有知識，像是 first-class function、Closure 這些概念，如果我們了解 Closure 是如何運作，就等於了解進階 JavaScript 程式設計的重要部分，而在其他方面 function Closure 也是相當實用的。
