---
title: Framework Aside：IIFE 和安全程式碼 (IIFEs And Safe Code)
categories: JavaScript
tags:
  - 克服 JavaScript 奇怪部分筆記
comments: true
date: 2019-11-21
---

先前我們已經了解 IIFE 是什麼，會使用在大部分的 JavaScript 框架和函式庫中，今天我們就來談談 IIFE 和安全程式碼，以及為何如此實用吧 !

## IIFE 和安全程式碼

這是我們先前建立的 IIFE，我們透過 `()` 建立一個函式表達式，呼叫它並傳入參數，這樣裡面的 CODE 屬性就會被執行，結果便會是 `Hello Apeiros0`。

```JavaScript
(function (name) {
  var greeting = 'Hello '
  console.log(greeting + name);
}('Apeiros0'));
```

### 背後發生的事

當程式載入時，我們會有全域執行環境，裡面不會有東西，因為我們沒有變數和函數陳述式被提升，當執行到 IIFE 時 (在執行階段)，在函式表達式的部分，會建立函式物件的記憶體，是匿名的，只是有程式碼在裡面的物件，然後是呼叫 function 的部分，新的執行環境就會被建立，裡面的程式碼會逐行執行，在經歷創造階段和執行階段後，`greeting` 變數會得到記憶體空間，值便是 `Hello`，這個變數會進到 function 的執行環境的變數環境中。

```
---------------------------------------------------------
|        () 的執行環境          |  greeting              |
|     (Execution Context)      |  'Hello'               |
---------------------------------------------------------
|         全域執行環境          |  ()                    |
|  (Global Execution Context)  |  (anonymous function)  |
---------------------------------------------------------
```

也就是說，**任何在 function 內的宣告的變數，都會在 function 內建立，並不會接觸到全域執行環境**，而這對寫程式是很實用的方法。

### 程式範例

我們分別有 `greet.js` 和 `app.js` 檔，並將 `greet.js` 放到 `app.js` 上，我們知道這不會建立新的東西，只會將堆在對方上面，

```HTML
<body>
  <script src="greet.js"></script>
  <script src="app.js"></script>
</body>
```

而我們在 `greet.js` 中有 `greeting = 'Hola'`，而在 `app.js` 中有 `greeting = 'Hello '`，執行後會發現在 `app.js` 輸出的 `greeting` 顯示 `Hola`，沒有和 `greeting = 'Hello '` 產生衝突，為什麼呢 ?

```JavaScript
// greet.js
var greeting = 'Hola';

// app.js
(function (name) {
  var greeting = 'Hello '
  console.log(greeting + name);
}('Apeiros0'));

console.log(greeting); // Hola
```

#### 執行堆如何運作

當執行後，全域執行環境建立，`var greeting = 'Hola'` 會加入到全域執行環境的變數環境中，然後 IIFE 被呼叫，建立新的執行環境，會加入 `var greeting = 'Hello '` 到新的執行環境的變數環境中，它們兩個分別在不同的執行環境中、不同的記憶體位址，這也代表，**我們能將程式碼包在 IIFE 中，確保不會和其他匯入 (include) 的程式碼產生衝突**，我們的程式碼會很安全。

```
----------------------------------------------------
|        () 的執行環境          |  greeting         |
|     (Execution Context)      |  'Hello'          |
----------------------------------------------------
|         全域執行環境          |  greeting  |  ()  |
|  (Global Execution Context)  |  'Hola'    |      |
----------------------------------------------------
```

### 取用全域物件

在許多的函式庫或 function，如果打開這些的原始碼，第一個看到的便會程式碼被包覆在 IIFE 中，我們可以學著這樣做，確保不會和其他程式互相衝突、不會不小心把東西放到全域物件中。

但如果我們想取用全域物件，只要透過參數傳入 function 即可，因為物件是 by Reference，就像下方的範例這樣，我們傳入 window 物件，讓 global 接收，這樣便能參照到全域物件，這裡命名 global，是因為伺服器的全域物件不會是 window 物件，所以命名 global，這樣就不用擔心全域物件是什麼。

```JavaScript
(function (global, name) {
  var greeting = 'Hello '
  console.log(greeting + name);
}(window, 'Apeiros0'));
```

我們可以故意讓 greeting 產生衝突。

```JavaScript
// greet.js
var greeting = 'Hola';

// app.js
(function (global, name) {
  var greeting = 'Hello ';

  global.greeting = 'Hello'; // 產生衝突

  console.log(greeting + name);
}(window, 'Apeiros0'));

console.log(greeting);
```

如果在 function 中建立物件，希望其他程式能取用時，我們可以將物件與全域物件連結，這是故意影響全域物件。

## 總結

我們了解了 IIFE 的應用方式，也認識了那些大型框架或函式庫常用的技巧，透過這個方式能避免不小心汙染到全域物件，當然，如果想取用全域物件的話，我們能使用傳遞參數的方式將全域物件帶入。