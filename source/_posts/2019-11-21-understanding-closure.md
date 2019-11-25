---
title: 了解閉包 (一) (Understanding Closures Part 1)
categories: JavaScript
tags:
  - 克服 JavaScript 奇怪部分筆記
comments: true
date: 2019-11-21
---

如果我們想深入了解 JavaScript，Closure (閉包) 是一個重要的觀念，我們需要了解一些東西的運作原理、了解 first-class function、了解執行堆和執行環境，這些東西都會幫助我們認識 JavaScript 的 Closure。

## 程式範例

我們先寫一些程式來展現 Closure 的威力，然後我們會檢視背後的運作方式。

我們建立一個 greet function，並設定 `whattosay` 作為參數 (可傳入 `Hello`、`Hola` ...)，我們不在 function 裡做任何事，直接回傳函式表達式，然後利用範圍鏈 (Scope Chain) 取得 `whattosay`。

```JavaScript
function greet(whattosay) {
  return function (name) {
    console.log(whattosay + ' ' + name);
  }
}
```

當我們呼叫 greet 時，會得到一個值，不是字串或數值，而是能再次的呼叫的 function，因為 function 是物件，我們能把它當作一個值回傳，這會產生有趣的語法，像是：

```JavaScript
greet('Hello')('Apeiros0');
```

雖然看起來很奇怪，但 `greet("Hello")` 執行後，會回傳 function ，所以我們能再次呼叫回傳的 function，執行後便會輸出 `Hello Apeiros0`。

### 奇怪的事情

然而，這裡有奇怪的事情發生了，為了讓這部分能明確點，我們稍稍改寫一下程式。

我們設定一個變數為 function 的回傳值，現在 `sayHello` 會是呼叫 greet 後回傳的 function，然後我們呼叫 `sayHello`，並傳入 `Apeiros0`，執行後也會是相同的結果。

```JavaScript
var sayHello = greet('Hello');
sayHello('Apeiros0');
```

有發現奇怪的點嗎 ? 為何 `sayHello` 仍知道 `whattosay` 變數是什麼 ?

`whattosay` 變數是在 greet function 被呼叫時所建立，當 greet 回傳 function 後，greet 就會離開執行堆，而當我們呼叫 `sayHello`，我們竟然能取得 `whattosay` 的值，為什麼呢 ?

```JavaScript
function greet(whattosay) {
  return function (name) {
    console.log(whattosay + ' ' + name);
  }
}

var sayHello = greet('Hello');
sayHello('Apeiros0');
```

### 運作方式

這是因為 Closure (閉包) 的關係，讓我們來看看程式執行時，發生了什麼事。

當程式開始執行後，我們會有全域執行環境，當執行到 `var sayHello = greet('Hello')` 時，會呼叫 `greet`，建立新的執行環境，傳遞給 `whattosay` 的參數會設定到變數環境中，然後回傳函式物件 (會立刻建立 function 並回傳)。

```
----------------------------------------------------------
|            greet()           |  whattosay              |
|     (Execution Context)      |  'Hello'                |
----------------------------------------------------------
|         全域執行環境          |  greet                  |
|  (Global Execution Context)  |  sayHello → undefined   |
----------------------------------------------------------
```

回傳後，`greet` 的執行環境會離開執行堆，但這裡會有個問題，每個執行環境都會有個記憶體空間，function 內所建立的變數和 function 都會存在於在這裡，那當執行環境離開後，這個記憶體空間會如何呢 ?

```
                               -------------------
                               |  whattosay      |
                               |  'Hello'        |
--------------------------------------------------
|         全域執行環境          |  greet          |
|  (Global Execution Context)  |  sayHello → ()  |
--------------------------------------------------
```

> 在一般的情況下，JavaScript 引擎會清除記憶體空間，這稱為垃圾回收 (Garbage Collection)。

在這個範例中，執行環境結束後，記憶體空間仍會存在。

現在我們回到全域執行環境，接著會呼叫 `sayHello` 所指向的匿名 function，然後會建立新的執行環境，賦予 name 記憶體空間，並將 `Apeiros` 設給 `name` 變數，當執行到 `console.log(whattosay + ' ' + name)`，JavaScript 引擎看到 `whattosay` 時，會透過範圍鏈、外部詞彙環境參照。

也就是說，會向外層的 function 尋找變數，因為在內部的 function 找不到，即便 `greet` 的執行環境已經離開執行堆，sayHello 的執行環境仍可以參照到 `whattosay` 變數 (在外部環境的記憶體空間)，換句話說，即使 `greet` 已經離開執行堆，它裡面的 function 仍可以參照到 `greet` 的執行環境的記憶體空間。

```
--------------------------------------------------
|              ()              |  name           |  ----
|     (Execution Context)      |  'Apeiros'      |     |
--------------------------------------------------     |
                               |  whattosay      |     |
                               |  'Hello'        |  <---
--------------------------------------------------
|         全域執行環境          |  greet          |
|  (Global Execution Context)  |  sayHello → ()  |
--------------------------------------------------
```

## Closure (閉包)

JavaScript 引擎確保我們的 function 仍可以在範圍鏈中找到，即使不在執行堆中，換句話說，**執行環境會把參照到的變數 (外部變數) 關住 (close)，這個包住所有可取用的變數的現象，我們稱為 Closure (閉包)**。

```
                        Closure
--------------------------------------------------------------
|  --------------------------------------------------        |
|  |              ()              |  name           |  ----  |
|  |     (Execution Context)      |  'Apeiros'      |     |  |
|  --------------------------------------------------     |  |
|                                 |  whattosay      |     |  |
|                                 |  'Hello'        |  <---  |
|                                 -------------------        |
--------------------------------------------------------------
--------------------------------------------------
|         全域執行環境          |  greet          |
|  (Global Execution Context)  |  sayHello → ()  |
--------------------------------------------------
```

這不是我們建立、不是我們叫 JavaScript 引擎做的事，Closure 只是 JavaScript 語言的特色，我們何時呼叫 function 並不重要、不必擔心 function 的外部環境是否仍在運作，JavaScript 引擎永遠會確保不論我們在執行哪個 function，它都能取用到應該要取用的變數。

這是 JavaScript 語言重要且強大的特色，可以讓我們寫一些有趣的程式模式 (Coding Patterns)，了解背後的運作方式，有助於我們了解 Closure，這只是一個功能，確保執行 function 時，能取用到外部的變數，它不在乎外部的 function 是否結束執行，所以當我們說我們建立 Closure，其實是 JavaScript 引擎建立 Closure，我們只是在利用它的特色而已。

在下一張篇文章，我們會看到經典的例子，當看到程式碼時，可能會很驚訝，不過只要了解 Closure 的運作方式，就會發現這意外的好懂。
