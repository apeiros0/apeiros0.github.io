---
title: 運算子 (Operators)
categories: JavaScript
tags:
  - 克服 JavaScript 奇怪部分筆記
comments: true
date: 2019-10-21
---
在上一篇中，我們已經探討過基本型別，今天我們來深入了解另一個觀念，可以幫助我們順利除錯 (debug) 和了解其他因為動態型別而產生的問題，我們就來聊聊什麼是運算子 (operators) 吧 !

## 運算子 (Operators)
> 運算子是一個特殊的 function，和我們自己寫的 function 有很大的不同，通常來說，運算子需要參數來回傳一個結果。

讓我們來看看範例，我們宣告一個變數 a，讓他等於 3 + 4，再來印出結果。

``` JavaScript
var a = 3 + 4;
console.log(a);
```

我們都知道結果為 `7`，但 JavaScript 是怎麼知道我們要把 3 和 4 相加呢 ?

這是因為語法解析器看到 `+` 後，會把兩個數字加起來，而這個 `+` 就是運算子，更準確地說，是加法運算子，是一個特殊的 function。

就像下方的範例這樣，用 `+` 當作是 function 名稱，傳入兩個參數，再將兩數相加，回傳結果。
``` JavaScript
function + (a, b) {
  return // 兩數相加
}
```

下方範例就是 JavaScript 在處理運算子所做的事，但和我們平常呼叫 function 是不同的，因為這樣做會很麻煩，我們要打出 function 的名稱，再傳入參數呼叫，而且也不直觀。

``` JavaScript
function + (a, b) {
  return // 兩數相加
}

// 平常呼叫 function 的方式，但運算子不是這樣呼叫
+(3, 4);
```

所以，JavaScript 就和其他程式語言一樣，用中綴表示法來表達運算方式，但本質仍是一個傳入兩個參數的 function，並回傳一個值給你。

> 表示法有分成三種：前綴、中綴、後綴表示法，中綴表示法是將運算符號 (function) 放在兩個參數中間，需要加上 `()` 來表指示運算順序。

``` JavaScript
function + (a, b) {
  return // 兩數相加
}
3 + 4; // 中綴，讀起來直觀易懂
+ 3 4;   // 前綴
3 4 +;   // 後綴 (舊式計算機的運算方式)
```

當然，運算子不只有一個，還有 `-`、`*`、`/` ... 等許多運算子，其中也有幾個運算子比較特殊，像是 `>` 和 `<`，它們會回傳 boolean 值給你。

``` JavaScript
var a = 3 > 4;
console.log(a); // false
```

## 總結
說了這麼多，我們只要謹記一個觀念
> **運算子只是一個特殊的 function，參數會被傳入到 function，然後回傳一個結果給你。**