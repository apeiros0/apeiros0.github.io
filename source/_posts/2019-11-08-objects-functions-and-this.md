---
title: 物件、函式和 this (Objects, Functions And this)
categories: JavaScript
tags:
  - 克服 JavaScript 奇怪部分筆記
comments: true
date: 2019-11-08
---

我們討論過 function 是物件，有屬性和其他的東西，今天我們來討論當 function 被呼叫時會發生哪些事情。

## Function 執行時

function 和物件不同，有 NAME 屬性和 CODE 屬性，當 CODE 屬性被呼叫時，會建立新的執行環境，放到執行堆中，這決定了程式要怎麼執行，我們來想像一下，當我們呼叫 CODE 屬性時，會發生什麼事 ?

我們知道執行環境會被建立，在創造階段，變數環境被建立，是在 function 內建立的變數所在的地方，還有可以參照到的外部環境，能夠透過範圍鏈一路往下找，也就是說，當我們要取用一個變數，它不在 function 的變數環境中，就會到外部尋找這個變數，直到全域執行環境為止，JavaScript 引擎還會給我們不曾宣告的 `this` 變數，它會指向不同的物件，取決於 function 在哪裡以及如何被呼叫。

```
      執行環境 — 創造階段 (Creation Phase)

-------------    ----------    -------------
|  變數環境  |    |  this  |    |  外部環境  |
-------------    ----------    -------------
```

## 不同的 `this`

我們用一些簡單的範例來解釋 `this` 關鍵字在不同情形下會是什麼吧 !

### 全域中的 `this`

我們知道 `this` 是可以立即取用的，在全域執行環境中也是一樣，所以當我們執行程式後，會得到全域物件 (Window 物件)，因為這裡的 `this` 是指向全域物件。

```JavaScript
console.log(this); // Window Object
```

### function 中的 `this`

讓我們再看看一些例子，我們建立一個 function，取名為 a，並輸出 `this`，當我們呼叫 function 時，會執行 CODE 屬性，也就是 function 內的程式碼，然後執行環境被建立，並產生 `this` 變數，這個 `this` 變數會是什麼呢 ?

```JavaScript
function a () {
  console.log(this); // Window Object
}

a();
```

執行後，我們同樣得到 Window 物件，所以當我們建立 function，`this` 仍會指向全域物件，那如果我們改用函式表達式來做呢 ?

```JavaScript
var b = function () {
  console.log(this); // Window Object
}

b();
```

我們仍會得到 Window 物件，所以不論我們用哪種方式建立 function，`this` 同樣會指向全域物件，而這三個案例中，都有各自的 `this` 變數，他們都指向同一個位址，指向全域物件，這代表我們能做一些事，像是：

我們能透過 `this` 來新增屬性，它會連結到全域物件，所以當我們呼叫 a 後，可以輸出 `newVariable`，因為我們使用 `.` 運算子連結 `newVariable` 到全域物件中。

```JavaScript
function a () {
  console.log(this);
  this.newVariable = 'hello';
}
a();

console.log(newVariable);
```

> 補充：任何連結到全域物件的變數或 function，都可以省略 `window.`

> 注意：如果我們不了解 `this` 指向什麼地方，可能會覺得它會連結到 function，這可能導致和全域命名空間衝突，產生許多的問題。

### 物件中的 `this`

先前我們已經認識函式表達式，現在我們要透過它在物件中建立方法，首先讓我們用物件實體語法建立物件，然後加入 name 屬性，值為 `The c object`，還有 log 方法，可以輸出 `this` 變數，而當我們呼叫 `c.log` 時，`this` 會是什麼呢 ?

```JavaScript
var c = {
  name: 'The c object',
  log: function () {
    console.log(this);
  }
}

c.log();
```

> 補充：如果物件的值是純值，那就稱為屬性；如果值是 function，那就稱為方法。

當 function 被呼叫時，新的執行環境會被建立，JavaScript 引擎會決定 `this` 要指向誰，在上述的案例會指向 Window 物件，但在這個案例中，function 是在物件中的方法，`this` 變數會指向 c 這個物件，JavaScript 引擎知道 function 連結到一個物件，所以 `this` 會指向包含 function 的物件。

我們也能透過 `this` 來改變物件的 name 屬性。

```JavaScript
var c = {
  name: 'The c object',
  log: function () {
    this.name = 'Updated c object'
    console.log(this);
  }
}

c.log();
```

當呼叫 `c.log` 後，會看到物件的 name 屬性被改變，也就是說，我們可以使用 `this` 和物件中的方法來改變物件。

## JavaScript 的 Bug

我們以上面的例子來修改，我們在 log 方法新增一個名叫 `setName` 的函式表達式，透過它來改變物件的 name 屬性，然後在 log 方法中呼叫 `setName`，傳入 `Updated again! The c object` 這個參數，接著輸出 `this`，我們就來看看結果是什麼吧 !

```JavaScript
var c = {
  name: 'The c object',
  log: function () {
    this.name = 'Updated c object'
    console.log(this);

    var setName = function (newName) {
      this.name = newName;
    }

    setName('Updated again! The c object');
    console.log(this);
  }
}

c.log();
```

當 setName 執行時，會建立新的執行環境，當中的 `this` 照理說會指向包含它的物件，所以 setName 應該會指向 c 物件，讓我們能改變 name 屬性，而第二個輸出結果應該會看到 `Updated again! The c object`。

```JavaScript
{name: "Updated c object", log: ƒ}
{name: "Updated c object", log: ƒ}
```

但人算不如 JavaScript 算，兩次輸出的 name 屬性都是 `Updated c object`，這代表 setName 沒有被呼叫嗎 ?

不，讓我們在 Console 視窗輸入 `window`，往下找會發現多出一個 name 屬性，其值為 `Updated again! The c object`，也就是說，setName 的執行環境被建立時，`this` 變數是指向全域物件，即使它在我們所建立的物件中，很多人都認為這是錯的，但在這情況中，JavaScript 就是這樣運作的。

> 補充：不管是函式表達式，還是函式陳述式都是相同的結果。

### 解決辦法

透過物件的 by reference 特性，建立 self 變數在 function 中，設定它為 `this`，確保取用的物件是我們所想要的，避免造成意外的錯誤。

```JavaScript
var self = this;
```

由於 `this` 是指向物件，有 by reference 的特性，所以 self 會指向和 `this` 相同的記憶體位址，會指向整個物件，當我們要用 `this` 時，都改用 self 變數，在子 function 中也要，這樣我們就不用考慮是否指向對的物件，而當我們改變 name 屬性時，就會更新成正確的東西。

而 setName 執行時，由於 self 沒有在 setName 內宣告，所以執行時會透過範圍鏈找到外部的 self 變數，而 self 變數仍指向整個物件，所以我們能在 setName 改變物件的屬性。

```JavaScript
var c = {
  name: 'The c object',
  log: function () {
    var self = this;

    self.name = 'Updated c object'
    console.log(self);

    var setName = function (newName) {
      self.name = newName;
    }

    setName('Updated again! The c object');
    console.log(self);
  }
}

c.log();
```

當然，結果就如同我們所預期的：

```JavaScript
{name: "Updated c object", log: ƒ}
{name: "Updated again! The c object", log: ƒ}
```

而 `this` 和 self 的指向就像這個樣子：

```
----------           ------------------------
|  this  |  ------>  |  0x001  |  c Object  |
----------     |     ------------------------
               |
----------     |
|  self  |  ----
----------
```

### 其他解決方法

我們也能使用 `let` 來解決這個問題，雖然現在幾乎所有的瀏覽器都支援 `let`，不過還是得依據專案來考慮，如果不需要使用舊瀏覽器的話，我們就能使用 `let` 來解決。

## 總結

我們已經了解 `this` 變數，當我們在全域執行環境建立 function，`this` 會是全域物件，而當我們透過物件所連結的方法呼叫時，`this` 會指向該物件，而在物件方法中的 function 都會有一些問題，我們可透過物件的 by reference 的特性設定變數，讓 `this` 指向正確的物件。
