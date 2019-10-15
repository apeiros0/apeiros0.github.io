---
title: function, context 和 變數環境
date: {{ date }}
categories: JavaScript
tags:
  - 克服 JavaScript 奇怪部分筆記
comments: true
---
今天我們來了解另一個 JavaScript 進階的觀念：變數環境 (variables environment)。

## 變數環境 (variables environment)
### 何謂變數環境
> 變數環境 (variables environment) 是指建立變數的位置，而在 JavaScript 中，**每個執行環境都有屬於自己的變數環境**，


簡單來說，就是每個執行環境都有一個建立變數的地方。

### 了解變數環境
我們透過範例程式碼來了解變數環境吧 !

在下方的程式碼中，分別有 function a 和 function b，而在全域環境有一個 myVar 變數，a 和 b 中也分別有屬於自己的 myVar，那麼在這段範例中 myVar 的值會是什麼呢 ?

``` JavaScript
function b() {
  var myVar;
}

function a() {
  var myVar = 2;
  b();
}

var myVar = 1;
a();
```

現在程式開始執行，全域執行環境被建立，myVar 變數被加入到記憶體中 (function 也是)，而**對全域執行環境來說，變數環境就是指全域物件 (瀏覽器的 window)**，所以 myVar 變數會被加入到全域物件中，當執行到 `myVar = 1` 時，myVar 便會得到 1 的值，接下來便是 function a 被呼叫，

```
---------------------------------------
|     全域執行環境     |   myVar = 1   |
---------------------------------------
```

當 function a 被呼叫時，建立新的執行環境，變數環境也會一起生成，myVar 會被放到新建立的變數環境中，這是因為每個執行環境都有屬於自己的變數環境，接著執行到 `myVar = 2`，新的變數環境中的 myVar 便會被賦予 2 的值，接著呼叫 function b，b 也會做相同的事。

```
---------------------------------------
|     a 的執行環境     |   myVar = 2   |
---------------------------------------
|     全域執行環境     |   myVar = 1   |
---------------------------------------
```
而 function a 和 function b 唯一不同的地方是 b 的 myVar 沒有被賦予值，所以值為 `undefined`。

```
-----------------------------------------------
|     b 的執行環境     |   myVar = undefined   |
-----------------------------------------------
|     a 的執行環境     |   myVar = 2           |
-----------------------------------------------
|     全域執行環境     |   myVar = 1           |
-----------------------------------------------
```

這裡的變數都被定義在自己的執行環境中，我們會分別在不同的執行環境看到不同變數，所以即使 myVar 被宣告三次，但他們分別都在不同的執行環境中，沒有任何關聯，現在我們分別把 myVar 印出來驗證看看吧。

``` JavaScript
function b() {
  var myVar;
  console.log(myVar); // undefined
}

function a() {
  var myVar = 2;
  console.log(myVar); // 2
  b();
}

var myVar = 1; // 會被加入到全域物件中
console.log(myVar); // 1
a();
```

得到的結果分別為 `1`、`2`、`undefined`，代表每個執行環境都有自己的 myVar 變數，而且不會互相影響彼此。

## 總結
當執行環境建立時，都會生成變數環境，變數環境會存放我們所建立的變數，不同執行環境的變數即便名稱相同，也不會影響彼此，因為他們分別在不同的地方執行，自然也不會互相影響，了解變數環境的概念後，我們就能了解後面其他的 JavaScript 觀念囉。
