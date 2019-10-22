---
title: 運算子的優先性 (Precedence) 和相依性 (Associativity)
categories: JavaScript
tags:
  - 克服 JavaScript 奇怪部分筆記
comments: true
date: 2019-10-22 08:53:28
---
昨天我們已經了解什麼是運算子，今天我們來認識運算子的優先性 (precedence) 和相依性 (associativity) 吧 !

## 優先性 (precedence)
> 優先性表示哪個運算子會被優先呼叫。

在同一行程式中，如果不只有一個運算子時，運算子會依優先性高低被呼叫，而 JavaScript 會先處理高優先性的運算子，然後依序到低優先性的運算子。


### 範例
在下方的範例中，我們知道數學有一套計算的順序，當然，JavaScript 同樣也有，這邊呼叫兩個運算子的 function ( `+` 和 `*` )，而我們知道 JavaScript 是同步執行，但不會同時計算它們，JavaScript 會先呼叫一個 function，再來呼叫另一個 function，但哪個會先被呼叫呢 ?

``` JavaScript
var a = 3 + 4 * 5;
console.log(a);
```

首先，JavaScript 會先用運算子的優先性判斷，在判斷之前，我們需要先知道 JavaScript 運算子的優先性是如何，這部分我們可以在 [MDN](https://developer.mozilla.org/zh-TW/docs/Web/JavaScript/Reference/Operators/Operator_Precedence) 找到運算子的優先性，在表格中，數字越高代表會優先被呼叫。

> 補充：在表格中有 `...` 這是指程式碼 (operator 的參數) 的意思。

所以，在範例中有 `+` 和 `*` 的運算子，`*` 的優先性為 14，而 `+` 的優先性為 13，我們知道高優先性會先被呼叫，因此 `*` 會優先被呼叫，然後回傳一個值，再與 `+` 做運算。

> 補充：其實 `=` 也是運算子，主要是將值設定到變數中

``` JavaScript
// 4 * 5 會優先被呼叫，回傳 20，然後 3 + 20 再被呼叫，回傳 23。
var a = 3 + 4 * 5; // step 1

var a = 3 + 20; // step 2

var a = 23; // step 3

console.log(a);
```


## 相依性 (associativity)
> 相依性表示運算子被呼叫的順序，有分為左相依性 (從左到右的相依性) 和右相依性 (從右到左的相依性)。

當同一行程式中，我們有多個相同優先性的運算子時，我們就要看相依性是如何，來判斷是從左到右還是從右到左呼叫。

### 範例
下方的範例中我們有多個優先性相同的運算子，他們最後都會相等，但這三個的值會分別是什麼呢 ?

``` JavaScript
var a = 3, b = 4, c = 5;

a = b = c;

console.log(a);
console.log(b);
console.log(c);
```
執行後，會發現結果都是 `5`，這與 `=` 的相依性有關，我們回到 [MDN](https://developer.mozilla.org/zh-TW/docs/Web/JavaScript/Reference/Operators/Operator_Precedence) 的表格，可以看到相依性 (Associativity) 的欄位，相依性有分為從左至右，代表從左邊開始執行，和從右至左，代表從右邊開始執行，而我們使用的是 `=` 運算子，他的相依性為右相依 (從右到左)。

這裡有個有趣的點，我們知道 `+` 和 `*` 這類的運算子會傳入左右的參數，運算後，回傳結果給你，但 `=` 是指派一個值給變數，它會回傳給你什麼呢 ?

在下方的範例，`b = c` 的 b 會被設定為 c 的值，然後回傳右邊的參數給你，所以我們知道 `=` 的左邊會被賦予值，右邊是值，然後 `=` 執行時會回傳右邊的值給你。

``` JavaScript
var a = 3, b = 4, c = 5;

console.log(b = c); // return 5，可以這樣想像 b = c = 5

// ----
// = 是右相依
a = b = c; // step 1，賦予 b = 5，並覆蓋 b 的值，回傳 5 回來

a = 5; // step 2，賦予 a = 5，並覆蓋 a 的值

console.log(a); // 5
console.log(b); // 5
console.log(c); // 5
```

> 注意：在 [MDN](https://developer.mozilla.org/zh-TW/docs/Web/JavaScript/Reference/Operators/Operator_Precedence) 表格最上面，有稱作 Grouping 的運算子，我們可以放一些東西在 `( ... )` 中，與其他運算子不同的是，它負責分出一個群組，並優先執行，所以在 `( ... )` 中的東西永遠都會最優先被執行，因為它的優先性是最高的。

## 總結
運算子的優先性可以幫助我們決定誰要先被呼叫，相依性也是，在多個運算子且有相同優先性時能發揮很好的作用。