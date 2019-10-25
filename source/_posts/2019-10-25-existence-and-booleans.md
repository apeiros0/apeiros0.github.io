---
title: 存在和布林 (Existence And Booleans)
categories: JavaScript
tags:
  - 克服 JavaScript 奇怪部分筆記
comments: true
date: 2019-10-25
---
上一篇文章中，我們了解到 JavaScript 動態型別和強制轉型不好的地方，那他們的用處是在哪呢 ? 讓我來談談存在  (Existence) 和布林 (Booleans)。

## 布林 (Booleans)
我們先將一些值透過 Boolean 這個內建 function 來轉換，像是：

> 注意：不推薦使用內建 function，但很適合拿來展示用。

``` JavaScript
Boolean(undefined); // false

Boolean(null); // false

Boolean("");   // false
```

`undefined`、`null`、`""` 這些都表示不存在，會轉型成 `false`，既然知道有這個特性，我們能好好利用，讓我們寫一些程式來舉例：

我們建立變數 a，透過 if 條件式來判斷，如果 a 有值就輸出 `Something is there !`。

> 補充：在 `if ()` 條件式中，在 `()` 內的東西會轉換成 boolean (`true` 或 `false`)。

``` JavaScript
var a;

// 這裡的 a 會被轉換成 boolean
if (a) {
  console.log('Something is there !');
}
```

當程式執行完，什麼都不會出現，這是因為執行環境的創造階段會賦予變數 `undefined`， `undefined` 會轉型成 `false`，所以我們可透過強制轉型來檢查 a 是否存在，如果 a 等於 `null` 或 `""` 同樣會被轉型成 `false`。

那如果 a 有值呢 ?
a 會轉型成 `true`，所以會顯示 `Something is there !`。

``` JavaScript
var a;

a = "Hi"; // 轉形成 true，可以用 Boolean("Hi") 來測試

// 這裡的 a 會被轉換成 boolean
if (a) {
  console.log('Something is there !');
}
```

我們可以利用強制轉型的特性來檢查變數是否有值，除了 `undefined`、`null`、`""` 的值之外，但現在有個問題，當我們使用數字 0 來判斷時，0 會轉型成 `false`，因此我們可以加上 `||` (or) 來多一個條件判斷，這樣就能避免 0 顯示 `false` 的結果。

``` JavaScript
var a;

a = 0; // 會轉型成 false

// 加入 a === 0 來判斷
if (a || a === 0) {
  console.log('Something is there !');
}
```

> 補充：`||` 相依性為 5；`===` 的相依性為 10，所以 `===` 會優先執行。

``` JavaScript
var a;

a = 0; // 會轉型成 false

(a || a === 0)
-> (a || true) 
-> (false || true) // || (or) 只要一方為 true，結果就為 true
```

## 總結
我們可以透過強制轉型來檢查東西是否存在，與條件式搭配使用。