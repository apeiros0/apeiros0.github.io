---
title: 比較運算子 (Comparison Operators)
categories: JavaScript
tags:
  - 克服 JavaScript 奇怪部分筆記
comments: true
date: 2019-10-24
---
先前我們已經了解運算子的優先性和相依性、強制轉型等觀念，現在我們統合這些觀念，來了解 JavaScript 一些奇怪的行為。

讓我們先看一些例子，像是：

``` JavaScript
console.log(1 < 2 < 3);
```

以我們的觀念來看，會覺得結果是 `true`，而執行程式後，結果也是 `true`，代表 JavaScript 同意這個陳述句 (statement) 是正確的。

但如果我們反過來，結果會是如何 ?

``` JavaScript
console.log(3 < 2 < 1);
```

我們會覺得答案是 `false`，那 JavaScript 也同意我們嗎 ?

不 ! 結果是 `true`，我們可以從先前所學的知識 —— **運算子的優先性和相依性**來解答這個問題。

## 運算子的優先性和相依性

我們有三個相同的 `<`，代表運算子的優先性是一樣的，所以我們得依靠相依性來決定從哪邊開始執行，而 `<` 的相依性是從左到右，所以左邊的 `3 < 2` 會先執行，其結果為 `false`，現在的陳述句就變成 `false < 1` 這個樣子。

``` JavaScript
3 < 2 < 1
-> 3 < 2 // false
-> false < 1
```

在 `false < 1` 的陳述句中，`<` 運算子被呼叫，傳入參數 `false` 和 `1`，此時，就是我們學到的 —— **強制轉型**上場的時刻了。

## 強制轉型
當我們給的參數型別是不一樣時，會將其中一方強制轉型，以這個範例來說，boolean 會被轉型成數字，所以 `false < 1` 會變成 `0 < 1`，結果就為 `true`，雖然以我們的角度來看是錯的，但以 JavaScript 的角度來看是正確的。

``` JavaScript
3 < 2 < 1
-> false < 1
-> 0 < 1 // true
```

> 補充：我們可以用內建 function `Number()` 來看強制轉型的結果 (不推薦使用內建 function，之後我們會談到)，像是使用 `Number("3")` 結果為數字 3，而 boolean 的 `Number(true)` 和 `Number(false)` 會分別轉型成數字 1 和 0。

現在我們知道這背後的運作方式，我們回到第一個範例，再重新看看，

``` JavaScript
console.log(1 < 2 < 3);
```

首先，`1 < 2` 為 true，所以會變成 `true < 3`，

``` JavaScript
1 < 2 < 3
-> 1 < 2 // true
-> true < 3 
```

而在 `true < 3` 中，`true` 會強制轉型成 1，所以會是 `1 < 3`，結果就為 true。

``` JavaScript
1 < 2 < 3
-> true < 3
-> 1 < 3 // true 
```

從上面的範例我們能知道，強制轉型會導致一些奇怪的結果，但在 JavaScript 的角度是合理的，然而並非每個轉型的結果都是可預期的，舉例來說，我們能把 boolean 和字串轉換成數字 (字串必須是數字才行)，但如果轉換 `undefined`，會出現 `NAN`，代表 `undefined` 不能轉換成數值；而轉換 `null` 的結果會是 0。

補充：NAN (Not a Number) 代表我有東西想轉換成數字型態，但它不是數字，所以無法轉換成數值。

``` JavaScript
Number(true); // 1
Number("3");  // 3

Number(undefined);  // NAN (Not a Number)
Number(null); // 0
```

雖然強制轉型的威力強大，但它同時也很危險，那我們要如何避免強制轉型發生呢 ?

## 比較運算子
讓我們到 JavaScript [運算子的優先性表格](https://developer.mozilla.org/zh-TW/docs/Web/JavaScript/Reference/Operators/Operator_Precedence)看看，我們可以看到在優先性 10 中，有 `==` 的比較運算子，代表兩樣東西是否相等，我們常在 `if` 條件式看到，它會回傳 `true` 或 `false` 給你，而 `==` 運算子一樣會發生強制轉型，舉例來說：

``` JavaScript
3 == 3   // true

"3" == 3 // true
```

既然會發生強制轉型，那就會發生一些奇怪的情況，導致我們很難 debug，舉例來說：

``` JavaScript
false == 0   // true (false 被強制轉型成 0)

null == 0    // false (null 會轉型成 0，但卻是 false)

"" == 0      // true

"" == false  // true
```

有許多的特殊情況，像是 `undefined` 和 `null` 不會按照我們預期的呈現，造成我們很多的困惑和問題，這其實被視為 JavaScript 的缺點，因為比較運算子中的 `==` 會導致一些未預期的行為，產生奇怪的錯誤。

> 補充：`null` 雖然會轉型成 0，但不會是在 `==` 的情況下轉換，在 `>` 或 `<` 這種情形就會轉型成 0。
``` JavaScript
null == 0  // false (null 會轉型成 0，但卻是 false)
null < 1   // true (null 轉型成 0) 
```

### 解決方式
回到[運算子的優先性表格](https://developer.mozilla.org/zh-TW/docs/Web/JavaScript/Reference/Operators/Operator_Precedence)，我們可以看到在 `==` 的下方會有 `===`，它**表示完全相等，不會強制轉換你的值**，如果兩個值的型態不同，會回傳 `false`，舉例來說：

``` JavaScript
3 === 3       // true (型別相同)

"3" === "3"   // true (型別相同)

"3" === 3     // false (型別不同)
```

所以使用 `===` 可以避免潛在的錯誤發生，我們可以用一些例子測試，在 `a == b` 的情況中，會輸出 `他們相等!` 的結果，但如果使用 `a === b` 則會輸出 `他們不相等!` 的結果。

``` JavaScript
var a = 0;
var b = false;

// a == b -> 他們相等!

if (a === b) {
  console.log("他們相等!");
} else {
  console.log("他們不相等!");
}
```

所以在判斷兩個值是否相同時，我們盡量使用同型別的比較，然後在**大部分的時候都使用 `===` 來比較是否相等**，除非你想讓你的值轉型，才使用 `==`，當然，我們也能 `!==` 來判斷是否不相同。

而我們也不必害怕這些運算子，因為我們知道他們其實是 function，只是在做不同的事，像是：

``` JavaScript
function ==(a, b) {}  // 會強制轉型

function ===(a, b) {} // 不會強制轉型
```

如果對比較相等性有興趣，我們可以到 [MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Equality_comparisons_and_sameness)，在下方有表格展示當你用 `==` 和 `===` 會有什麼結果，表格中也有下一版的 `Object.is()` 的比較方法，可以參考看看。

## 總結
我們從運算子的優先性和相依性與強制轉型了解到 JavaScript 一些奇怪的事情，而當我們在做運算子的比較時，盡量都使用 `===` 或 `!==` 的方式，這樣能比較好 debug。