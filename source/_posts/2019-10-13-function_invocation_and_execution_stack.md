---
title: function 呼叫和執行堆
date: 2019-10-13
categories: JavaScript
tags:
  - 克服 JavaScript 奇怪部分筆記
comments: true
---
今天我們要來了解 function 是如何被呼叫，以及是如何執行的，這對了解 JavaScript 進階的觀念有很大的幫助。

## function 呼叫 (function invocation) 
在來了解 function 呼叫前，我們先來談談什麼是**呼叫 (invocation)**。

> 呼叫 (invocation) 表示執行 function。

也就是說當我們說 function 呼叫或是呼叫 function，都在表示執行這個 function，而在 JavaScript 中，我們是**用 `()` 來執行 function**，只要 function 名稱 + `()`，就是在執行 function。

舉例來說，我有一個 function a，我想要呼叫它，可以這麼做。
``` JavaScript
function a () {
  console.log('Called a');
}

// 呼叫 function a，開始執行
a();
```

## 執行堆 (execution stack)
在了解 function 是如何呼叫後，我們來看看在 JavaScript 中，呼叫 function 後會發生什麼事。

我們用簡單的例子來了解 function 的執行過程。在這裡我分別有 function a 和 function b，function b 不會做任何事，而 function a 則會去呼叫 function b 執行。
``` JavaScript
function b () {}

function a () {
  b();
}

a();
```

**現在我們來了解 function 的執行過程吧 !**

### 執行過程
在執行上面的程式碼後，我們知道程式會被語法解析器解析，再透過編譯器編譯，然後全域執行環境會被建立，全域執行環境建立時，會建立全域物件、this 變數、外部環境，而程式中的 function a 和 b 會加入到記憶體中，之後再逐行執行程式碼，

```
----------------------------
|       全域執行環境        |
----------------------------
```
當程式執行到 `a();` 時，會去呼叫 function a，建立一個新的執行環境，並加入到執行堆 (execution stack) 中，也就是說，當我們**每次呼叫 function 時，都會建立新的執行環境，並加入到執行堆中**，
```
 執行堆 (execution stack) ↓

----------------------------
|   function a 的執行環境   |
----------------------------
|       全域執行環境        |
----------------------------
```
補充：堆 (stack) 是由一層一層所堆疊起來的東西，在最上層的是我們正在執行的東西。

新的執行環境被建立，就像全域執行環境一樣，有屬於自己 this 變數、外部環境和記憶體空間放置變數和 function，一樣會經歷創造階段和執行階段。如果呼叫新的 function，便會停止正在執行的程式，再建立新的執行環境。

我們在 function a 中有呼叫 function b，所以 b 的執行環境被建立，而 a 的程式現已經停止執行。

```
 執行堆 (execution stack) ↓

----------------------------
|   function b 的執行環境   |
----------------------------
|   function a 的執行環境   |
----------------------------
|       全域執行環境        |
----------------------------
```
上述的便是 JavaScript 在呼叫 function 後的執行過程。

### 執行堆離開順序
當 function 執行結束後，如果是在執行堆的最上層，會最先離開執行堆，之後依照順序離開，直到回到最下層的全域執行環境。

當 function b 執行完時，會離開 (pop) 執行堆，之後是 function a 離開，最後回到全域執行環境。
```
----------------------------
|   function b 的執行環境   |  執行完離開執行堆
----------------------------
             .
             .
             .
 執行堆 (execution stack) ↓
----------------------------
|   function a 的執行環境   |
----------------------------
|       全域執行環境        |
----------------------------
```

### 程式順序不影響執行順序
我們把上面的範例修改成以下這樣，來驗證程式順序並不影響執行的順序，在下方的範例中，我們知道在全域執行環境的創造階段會將 function a, b 和變數 d 加入到記憶體中，之後再依序執行程式碼。

``` JavaScript
function a () { // 移到 function b 上面
  b();
  var c; // 加入 var c;
}

function b () {
  var d; // 加入 var d;
}

a();
var d;   // 加入 var d;
```

當執行到呼叫 function a 時，建立 a 的執行環境，之後執行 a 的內容，遇到呼叫 b，建立 b 的執行環境，再執行 b，function b 執行完後，離開執行堆，之後回到 a 的執行環境，從還沒執行到的那一行再開始執行 (`var c;`)，a 執行完畢，離開執行堆，回到全域執行環境，再繼續執行 `a();` 的下一行 (`var d;`)。

## 總結
當我們呼叫 function 時，會建立執行環境，加入到執行堆中，並在創造階段建立屬於自己的 this 變數、外部環境和記憶體空間放置變數和 function，而在執行階段，會依照順序從上到下執行，當 function 執行完畢後，便會離開執行堆。

補充：即使 function 是呼叫自己，執行環境也會建立。
