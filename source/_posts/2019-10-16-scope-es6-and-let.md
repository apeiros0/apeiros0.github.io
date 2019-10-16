---
title: Scope, ES6 和 let
categories: JavaScript
tags:
  - 克服 JavaScript 奇怪部分筆記
comments: true
date: 2019-10-16
---
到現在，我們已經認識執行環境、變數環境、詞彙環境和外部環境，而這些最終定義了範圍鏈 (Scope Chain，每個正在執行的 function 都有的外部參照)。

講了這麼多，你知道 Scope 是什麼嗎 ?

## Scope
> Scope 表示變數可以被取用的範圍。

如果有相同的 function 呼叫兩次，會各自產生執行環境，即便裡面的變數相同，但在記憶體中，他們是不同的變數。

舉例來說，在下方的程式碼中，function a 會分別建立各自的執行環境，而裡面的變數是不會互相影響，因為他們分別在不同的記憶體中。

``` JavaScript
function a () {
  var myVar = 1;
}

a();
a();
```

## let
ES6，又稱 ECMAScript 6 或 ECMAScript 2015，引用了新的宣告變數方法 `let`。

### 與 `var` 不同之處
`let` 可以像 `var` 一樣使用，但並沒有取代 `var`，它讓 JavaScript 引擎使用一個叫做**區塊範圍 (block scoping)** 的東西，而在宣告變數時，能像 `var` 一樣使用，變數仍會被放到記憶體中，設值為 `undefined`，不同的是，在執行階段直到 `let` 那一行被執行時，才能使用 `let` 所宣告的變數，如果在 `let` 之前取用便會出現錯誤 (他仍在記憶體中，只是 JavaScript 引擎 不讓你取用)。

在下方的範例中，如果我在 `let c` 之前就取用 c 的話，此時便會出現錯誤。

``` JavaScript
console.log(c); // error
let c = true;
```
### 在區塊中宣告
當我們在區塊 (block) 中宣告 `let` 時，表示變數只能在這個區塊中使用，而在區塊外取用的話，便會出現錯誤訊息。如果我們有迴圈正在執行，使用 `let` 宣告變數，每次在迴圈中，**變數在記憶體中都是不同的**。

``` JavaScript
if (true) {
  let a = 0;
}
console.log(a); // error: a is not defined
```

> 補充：區塊指的是 `{ ... }`，像是在 `if () { ... }`、`for () { ... }`、`function () { ... }` 裡面。
