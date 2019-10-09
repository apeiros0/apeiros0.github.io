---
title: JavaScript 和 undefined
date: 2019-10-09
categories: JavaScript
tags:
  - 克服 JavaScript 奇怪部分筆記
comments: true
---
在上一回中，我們有提到 `undefined`，今天我們來詳細了解 `undefined` 是什麼吧 !

## undefined
> undefined 是 JavaScript 內建的一個**特殊的值**，表示變數還尚未被設定，它實際上會佔據記憶體的空間，而在創造階段，會設定給變數的值。

我們可以用下方的範例來驗證 `undefined` 是 JavaScript 內建的值，當 if 條件式判斷時，其執行結果為 `a is undefined`，這也代表 a 與 `undefined` 都是在指 `undefined` 這個特殊值。

``` JavaScript
var a;
// === 代表比較嚴謹的判斷
if (a === undefined) {
  console.log('a is undefined');
} else {
  console.log('a is defined');
}
```
> 注意：這裡的 undefined 並非字串，是一個特殊的值。

如果我們賦予值給變數 a，會發現結果為 `a is defined`，代表現在的 a 並不是 `undefined`。

``` JavaScript
var a = "Called a";
if (a === undefined) {
  console.log('a is undefined');
} else {
  console.log('a is defined');
}
```

> **注意：永遠不要在宣告變數時，設定值為 `undefined`，這會很難分辨是 JavaScript 幫你設定的，還是是你自己設定的。**

``` JavaScript
var a = undefined; // Bad
var a; // Good，永遠保持這樣設定
```

## is not defined
如果我們把上面範例的 `var a;` 拿掉，執行時會顯示 `Uncaught ReferenceError: a is not defined` 的錯誤訊息。

這是因為在創造階段時，沒有找到 `var a`，所以 a 沒有被加入到記憶體中，而在執行階段時，便會跳出「參照不到這個值：a 沒有被定義」的錯誤訊息。

``` JavaScript
if (a === undefined) {
  console.log('a is undefined');
} else {
  console.log('a is defined');
}
```

## undefined 與 is not defined 有什麼不同 ?
* `undefined` 是 JavaScript 中一個特殊的值。
* `is not defined` 代表變數沒有被加入到記憶體中，導致在執行階段跳出錯誤訊息。