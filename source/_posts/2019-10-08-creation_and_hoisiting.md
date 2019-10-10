---
title: 創造和提升 (Hoisiting)
date: 2019-10-08
categories: JavaScript
tags:
  - 克服 JavaScript 奇怪部分筆記
comments: true
---
今天我們要來了解 JavaScript 在建立執行環境時做了什麼。

## 提升 (Hoisiting)
首先我們來做在別的程式語言沒辦法做到，但在 JavaScript 中卻能辦到的行為，以下是今天的範例程式碼。
``` JavaScript
var b = "Called b";
function a() {
  console.log('Called function a');
}

a();
console.log(b);

// console.log(...) 可以將 (...) 裡面的程式碼顯示在 Console 視窗中
// a() 是呼叫 function a 執行
```

我們把 `a()` 和 `console.log(b)` 移到程式的最上方，觀察 Console 視窗會出現什麼結果。

``` JavaScript
a();
console.log(b);

var b = "Called b";
function a() {
  console.log('Called function a');
}
```

一般而言，在別的程式語言這樣做會產生錯誤，因為在執行時，變數 b 還尚未宣告，所以會出現錯誤，但在 JavaScript 中，會顯示 `Called function a` 和 `undefined`，神奇的是，竟然沒有產生任何錯誤。

透過這個例子，我們了解到，當 JavaScript 執行時，即使在 function 前面呼叫，function 仍會執行，而變數也會執行，只是它得到的結果是 `undefined`。

> **這種將尚未宣告的 function 或變數移到前方執行，程式仍可運作的現象，我們稱做 提升 (Hoisiting)。**

補充：如果我們移除 function a 或變數 b，執行時會出現 `a is not defined` 或是 `b is not defined` 的錯誤。

``` JavaScript
// error: a is not defined
a();
console.log(b);

var b = "Called b";
```

``` JavaScript
a();
// error: b is not defined
console.log(b);

function a() {
  console.log('Called function a');
}
```

## 如何產生提升 (Hoisiting) ?
現在有了 提升 (Hoisiting) 的概念後，我們來了解它是如何產生的。

JavaScript 這種沒有先宣告，就能取用的情況，是因為執行環境分為**創造階段 (Creation Phase)**和**執行階段 (Execution Phase)**

### 創造階段 (Creation Phase)
在這個階段，會建立：
* 全域物件
* this 變數
* 外部環境
* 你的程式碼 (正在被轉換)

你的程式碼在這個階段會透過語法解析器轉換，它會知道你在哪裡建立變數和 function，並設定到記憶體中，而這個步驟就叫 提升 (Hoisiting)。

也就是說，在執行程式碼前，JavaScript 引擎已經為變數和 function 建立空間給它們，所以在這個階段它們已經存在於記憶體中，因此在逐行執行時，便能找到變數和 function。

我們從上面的範例可以知道，function 已經全部被設定在記憶體中，因此我們能直接取用，然而，變數的情況卻有些不同，變數會透過 `=` 來賦予值，但這要在執行階段才會賦予，因此**在創造階段會先賦予 `undefined` 作為值的替代**。

補充：`undefined`表示**我還不知道它的值是什麼**，如果我一開始沒有設值給變數，也會得到一樣的結果，這也代表**所有的 JavaScript 變數一開始都會被設定為 `undefined`**。
``` JavaScript
var b;
console.log(b); // undefined
```

然而，依賴提升 (Hoisiting) 並非一件好事，這可能會造成程式執行上的錯誤，以下方的例子來說，假設我要透過 console.log 查看 d 的值，但由於提升的特性，會顯示 `undefined`，並不會顯示 `Called d`，所以寫程式時，我們**要盡量把變數和 function 寫在最上面**。
``` JavaScript
var b = "Called b";

function a() {
  console.log('Called function a');
}

a();
console.log(b);
```

### 執行階段 (Execution Phase)

執行階段 (Execution Phase) 會逐行編譯、轉換你的程式碼，詳細情形之後我們便會談到。