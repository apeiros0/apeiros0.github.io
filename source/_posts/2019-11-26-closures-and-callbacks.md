---
title: 閉包和回呼 (Closures And Callbacks)
categories: JavaScript
tags:
  - 克服 JavaScript 奇怪部分筆記
comments: true
date: 2019-11-26
---

我們在寫 JavaScript 程式時，可能已經使用過 Closure, first-class function 和執行環境這些概念，像是 `setTimeout()` 或 jQuery 的事件，今天我們就來談談閉包和回呼 (Closures And Callbacks) 吧 !

## 範例程式

如果曾寫過像下方的範例程式碼，代表已經曾使用過 Closure 或函式表達式。

### `setTimeout()`

我們先建立一個簡單的 function，叫做 sayHiLater，並在裡面新增 `greeting` 變數，值為 `Hi`，然後我們使用 JavaScript 內建的函式 `setTimeout()`，`setTimeout()` 會將 function 作為參數，並設定等待多少毫秒。

```JavaScript
function sayHiLater() {
  var greeting = 'Hi';

  setTimeout(function () {
    console.log(greeting);
  }, 5000);
}
```

然後呼叫 sayHiLater。

```JavaScript
sayHiLater();
```

在等待 5 秒後，會執行作為參數的 function，輸出 `Hi` 的結果。

```JavaScript
Hi
```

#### 背後的函式表達式和 Closure

- **函式表達式**

`setTimeout()` 可以接受兩個參數，一個是**函式表達式 (函式物件)，我們將它作為參數傳入，這是在使用 first-class function 的概念，利用函式表達式建立 function**；另一個參數是我們等待的時間。

- **Closure**

當 sayHiLater 執行完，先前我們曾討論過 JavaScript 的非同步和 event loop，`setTimeout()` 會在 event table 計時 5 秒，5 秒過後，函式表達式會在 event queue 等待執行，JavaScript 引擎會檢查有沒有 function 在等待執行，然後就會執行函式表達式。

執行函式表達式後，會發現 `greeting` 不在 function 中，因為 sayHiLater 已經執行結束，所以就輪到 Closure 出場，`greeting` 會透過範圍鏈尋找，而 **Closure 幫我們保留 `greeting` 變數**，所以我們 5 秒後仍可取用到 `greeting` 變數，即便 sayHiLater 已經執行完一段時間。

### jQuery 事件

如果曾寫過這樣的程式碼：

```JavaScript
$("button").click(function () {});
```

`click()` 是一個 function，在 jQuery 的程式碼中，它接受另一個 function 在事件觸發時執行，這裡我們就使用了 first-class function，將 function 作為參數傳遞，並透過函式表達式來宣告 function。

## Callback (回呼)

當 `setTimeout()` 執行結束後，會執行作為參數的 function，這稱為 callback (回呼)，callback function (回呼函式)，換句話說，當 function 執行結束後，會呼叫作為參數的 function。

```JavaScript
setTimeout(function () {
  console.log(greeting);
}, 5000);
```

> Callback Function 是當某個 function 執行完，會執行我們給它的 function，舉例來說，我們呼叫 functionA，給它 functionB，當 functionA 結束，會呼叫 functionB 給我們，functionA 呼叫 functionB 回來執行，這就是 Callback Function

### 建立 Callback Function

我們建立一個 tellMeWhenDone 的 function，它接受 `callback` 參數，然後在裡面設定一些變數，當它結束時，會呼叫 `callback` 執行。

```JavaScript
function tellMeWhenDone(callback) {
  var a = 1000;
  var b = 1000;

  callback();
}
```

之後我們呼叫 tellMeWhenDone，給它 callback function (呼叫後會執行的 function)。

```JavaScript
tellMeWhenDone(function () {
  console.log('I am done!');
});

tellMeWhenDone(function () {
  console.log('I also done!');
});
```
