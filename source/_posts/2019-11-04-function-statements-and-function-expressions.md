---
title: function 陳述式與表達式 (Function Statements And Function Expressions)
categories: JavaScript
tags:
  - 克服 JavaScript 奇怪部分筆記
comments: true
date: 2019-11-04
---

我們知道在 JavaScript 中 function 是物件，今天我們就來了解 JavaScript 是如何運用 first-class function 的觀念，在這之前我們先來了解 function 陳述式與 function 表達式的差異。

## 表達式 (Expression)

> 表達式是程式碼形成一個值的單位 (unit)。

在 JavaScript 中 function 表達式或是其他表達式，最終會回傳一個值，而這個值不一定要儲存於變數中。

舉例來說，我們宣告一個變數 `a`，然後執行，`a` 會在記憶體中，那什麼是表達式呢 ?

```JavaScript
var a;
```

我們知道表達式會回傳一個值，現在我們有程式碼的單位 `a = 3`，基本上一行或多行的程式碼會作為一個單位，我們知道 `=` 運算子最終會回傳一個值，所以 `a = 3` 最終會回傳 `3`，因為我們把 `3` 傳入到變數 `a` 中，`=` 運算子會回傳右邊的 `3` (第二個參數)，這會被放到記憶體中 (`=` 運算子的關係)。

```JavaScript
// 表達式
a = 3; // 這行就是一個單位 (unit)
```

我們也能執行不同的表達式，`+` 運算子會接受兩個值，回傳 `3` 的結果，但這不會被放到記憶體中，因為我們沒有用 `=` 運算子設定值，這也是表達式，因為會回傳一個值。

```JavaScript
1 + 2;
```

回傳的值不論是什麼形式 (物件、數值、字串 ...) 都可以，所以我們也能把變數 `a` 賦予物件，`=` 運算子最終回傳物件的值，這也是表達式，因為它會回傳一個值。

```JavaScript
a = { greeting: 'Hi' };
```

## 陳述式 (Statements)

當我們說到陳述式，像是 `if` 陳述式，在 `if` 陳述式中會放入表達式 `a === 3`，因為表達式會形成一個值 (這裡是 `true/false`)，但 `if` 陳述式不會回傳任何值。

```JavaScript
if (a === 3) { ... }
```

我們不能將陳述式設為變數的值，這會出現錯誤，因為陳述式不會回傳任何值。

```JavaScript
var b = if (a === 3) { ... } // 這樣是不行的
```

## function 陳述式和表達式差異

在陳述式中可以擁有表達式，反之就不行，也就是說，陳述式會做一些事，而表達式會回傳值，在 JavaScript 中，function 是物件，所以可以有 function 陳述式和 function 表達式，讓我們來看看其中的差異。

### function 陳述式

我們先宣告一個 greet function，可以輸出 `hi`，這就是 function 陳述式，當它被執行時，會被放到記憶體中，不會回傳任何值，但它可以被提升 (hoisting)，可以在 greet function 之前呼叫，它仍然是個物件，NAME 屬性是 `greet`，CODE 屬性是寫在裡面的程式碼。

```JavaScript
greet();

// function 陳述式
function greet () {
  console.log('hi');
}
```

當程式執行時，function 陳述式當執行環境建立，會被放到記憶體中，會有 NAME 和 CODE 的屬性，當我們呼叫 `greet`，會連結到記憶體中函式物件所在的地方，然後呼叫 CODE 屬性。

```
  --------------------------
  |        Function        |
  --------------------------
     ↓                   ↓   invocable ()
-----------      ------------------------
|  NAME   |      |  CODE                |
|  greet  |      |  console.log('hi');  |
-----------      ------------------------
```

### function 表達式

我們宣告一個 `anonymousGreet` 變數，使用 function 表達式當作值，我們知道 JavaScript 中 function 是物件，所以這裡等於建立一個物件，透過 `=` 運算子把函式物件放到記憶體中，將 `anonymousGreet` 變數指向函式物件的位址。

```JavaScript
// function 表達式
var anonymousGreet = function () {
  console.log('hi');
}
```

當我們要呼叫 function 時，不是根據 NAME 屬性來找，變數會知道記憶體的位址，所以能透過變數來呼叫。

```JavaScript
anonymousGreet();
```

當我們執行程式時，會有 NAME 屬性和 CODE 屬性，由於沒有在 function 的 `()` 前放名稱，所以 NAME 屬性沒有任何的值，但可透過變數參照函式物件的位址，不需要用 NAME 屬性去參照，這就是匿名 function。

```
  --------------------------
  |        Function        |-----
  --------------------------    |
         ↓                      ↓   invocable ()
-----------------    ------------------------
|  NAME         |    |  CODE                |
|  (anonymous)  |    |  console.log('hi');  |
-----------------    ------------------------
```

當然，我們也可以讓匿名 function 有名稱，不過沒什麼用處，好的程式碼要清楚易懂，要盡量減少寫程式碼的數量，匿名 function 就是個好例子，可以清楚知道我們在建立函式物件。

### function 表達式不會提升

如果我們在 `anonymousGreet` 變數前呼叫，就像陳述式一樣，在 function 前呼叫，會發生什麼事呢 ?

```JavaScript
anonymousGreet();

var anonymousGreet = function () {
  console.log('hi');
}
```

當全域執行環境建立，在創造階段會把 `anonymousGreet` 變數放到記憶體中，並賦予 `undefined` 的值，所以當執行階段執行到 `anonymousGreet()` 時，其實會是 `undefined()`，所以會顯示 `Uncaught TypeError: undefined is not a function` 的錯誤訊息。

由此可知，function 表達式並不會被提升，因為被提升的是變數，直到執行到 function 表達式那行，變數才會被賦予函式物件，所以我們必須先讓變數指向函式物件的位址才能呼叫。

### First-Class Function

我們再來做一些事，先建立一個 log function，可以輸出傳入的值。

```JavaScript
function log (a) {
  console.log(a);
}
```

我們可以直接建立一些值，傳入到 function 中，

```JavaScript
log(3); // 3

log("Hello"); // Hello

log({
  greeting: "Hi"
}); // { greeting: "Hi" }
```

或是透過 function 表達式建立函式物件傳入到 log function 中，log function 會把程式碼放到 CODE 屬性，就如同數值、字串、物件一樣，我們只是建立函式物件，並傳入到另一個 function 中，這就是 first-class function — function 可以被傳入別的地方、快速建立和使用、設定成變數的值。

```JavaScript
log(function () {
  console.log("Hi~");
});
```

如果我們想輸出 `Hi~`，可透過參數 `a` 呼叫，因為參數 a 會指向這個函式物件，所以當參數 a 呼叫時，會得到 `Hi~` 的結果。

```JavaScript
function log (a) {
  a(); // Hi~
}

log(function () {
  console.log("Hi~");
});
```

<!-- ## Functional Programming

我們用 function 表達式，把 function 傳入，當作另一個 function 的參數，這個 first-class function 的概念，給 function 給另一個 function，就像變數一樣，引入一個全新的形態到程式語言中，我們稱之為 functional programming (函式程式語言) -->

## 總結
function 表達式可透過變數來指定記憶體位址，就如同純值和物件一樣，而這就是 first-class function 的概念，function 表達式可當作變數、可傳入到其它 function、也可以快速建立，而 function 表達式與陳述式的差異是，表達式不會提升，陳述式會被提升，透過了解這些概念，有助於我們更加認識 function。