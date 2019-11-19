---
title: 立即呼叫的函式表達式 (Immediately Invoked Function Expressions (IIFE)s)
categories: JavaScript
tags:
  - 克服 JavaScript 奇怪部分筆記
comments: true
date: 2019-11-19
---

今天我們要來談談 JavaScript 一個常見的，而且經常使用的簡潔觀念 — 立即呼叫的函式表達式 (Immediately Invoked Function Expression)，又稱 IIFE。

## 函式陳述式和函式表達式的差異

先前我們已經了解函式陳述式和函式表達式的差異，讓我們稍微複習一下。

### 函式陳述式

函式陳述式是一個陳述式，function 會在新的一行中作為第一個字出現，或是在 `;` 後面，舉例來說：

```JavaScript
// 函式陳述式
function greet(name) {
  console.log('Hello ' + name);
};

greet('Apeiros0');
```

函式陳述式在創造階段會加入到記憶體中，而執行階段不會執行，需要呼叫才會執行。

### 函式表達式

函式表達式，我們可以設置給變數，舉例來說：

```JavaScript
// 函式表達式
var greetFunc = function (name) {
  console.log('Hello ' + name);
};

greetFunc('Apeiros0');
```

函式表達式在創造階段不會放到記憶體中，而是在執行階段會快速建立函式物件，我們可透過變數來呼叫，變數會指向函式物件的記憶體位址。

## 立即呼叫的函式表達式 (IIFE)

我們可以對 function 做一些事，讓它成為特殊的物件，我們知道 function 有 CODE 屬性，我們可以呼叫 CODE 屬性，在函式表達式的範例中，我們有透過 `greetFunc()` 來呼叫，其實我們也可以不用另外呼叫 function，可以在創造 function 時 (在執行階段的時候)，直接呼叫 function，這就是立即呼叫的函式表達式，又稱 IIFE。

```JavaScript
// 立即呼叫的函式表達式 (IIFE)
var greeting = function (name) {
  console.log('Hello ' + name);
}(); // 在這裡呼叫
```

### 傳入參數

我們稍微改寫一下上面的範例，讓它回傳 `'Hello ' + name`，greeting function 會得到什麼值呢 ?

```JavaScript
var greeting = function (name) {
  return 'Hello ' + name;
}();

console.log(greeting);
```

首先，函式物件會先被函式表達式建立，然後呼叫，當值回傳後，它會被設為 greeting 的值，所以當我們輸出 greeting 時，會得到 `Hello undefined`。

因此，我們可以傳入參數給給這個 IIFE，這裡我們一共做了三件事，建立 function、呼叫 function、傳入參數。

```JavaScript
var greeting = function (name) {
  return 'Hello ' + name;
}('Apeiros0');

console.log(greeting);
```

> 這裡要注意，greeting 得到的是一個字串，不是 function，如果我們試著呼叫它會出現 `Uncaught TypeError: greeting is not a function` 的錯誤訊息。

順帶一提，如果我們沒有立即呼叫，greeting 會輸出整個 function。

```JavaScript
var greeting = function (name) {
  return 'Hello ' + name;
};

console.log(greeting);

// ƒ (name) {
//   return 'Hello ' + name;
// }
```

### 獨立的 IIFE

有趣的是，我們能將 IIFE 獨立出來，先讓我們看看一些例子：

我們能在程式碼放上一個數值、字串、布林等純值，加上分號是確保語法解析器知道這是一行程式碼，這是有效的 JavaScript 表達式，JavaScript 可以執行表達式，不需要儲存在變數或其他東西上，它只是執行表達式，不會做其他的事情。

```JavaScript
3;

'I am a string';
```

物件也是一樣的，它只是在那裡，執行後不會有任何錯誤。

```JavaScript
{
  name: 'apeiros0'
};
```

那 function 是如何呢 ?

如果讓函式表達式快速建立，執行後會出現 `Uncaught SyntaxError: ...` 的錯誤，這是因為語法解析器看到 function 這個單字在最前面，或是在分號後，它會預期這是函式陳述式，要有 function 的名稱，不能是匿名的，所以這個語法有問題，那我們該如何讓這個函式表達式像 `3;` 或是 `'I am a string';` 這樣，該如何讓語法解析器了解我們不想讓它變成函式陳述式呢 ?

```JavaScript
function (name) {
  return 'Hello ' + name;
};

// Uncaught SyntaxError: Function statements require a function name
```

我們只要**確保 function 這個字不是在每一行最前面的字即可**，只要不是第一個字，就不會有任何問題，語法解析器會知道這不是函式陳述式，這有不同種的做法，而最被容易接受、容易閱讀、打最少字的就是把 function 包進 `()` 中。

```JavaScript
(function (name) {
  return 'Hello ' + name;
});
```

JavaScript 引擎知道在 `()` 中的東西一定是表達式，它會假設你寫的是函式表達式，知道你在建立函式物件，如果我們執行，不會有任何錯誤。

> `()` 在 JavaScript 是一個運算子，會用在一些表達式上，像是 `(3+4)*5;`，或是把一些東西群組 (Grouping) 起來，這是群組運算子 (Grouping Operator)，我們不會將 `()` 使用在陳述式上，像是 `(if () { ... })`，它永遠會與表達式一起使用，且會回傳一個值回來。

#### 呼叫獨立的 IIFE

我們稍微修改上面的範例，我們有個 function 能輸出一些東西，然後執行，它會存在於記憶體中，但會被釋放掉，因為我們沒有對它做任何事。

```JavaScript
(function (name) {
  var greeting = 'Hello '
  console.log(greeting + name);
});
```

我們能呼叫它，在 function 後，這也是 IIFE 最常見的樣子，我們在同一時間建立 function，並執行它。

```JavaScript
(function (name) {
  var greeting = 'Hello '
  console.log(greeting + name);
}());
```

當然，我們也能傳遞參數進去。

```JavaScript
var firstname = 'Apeiros0';

(function (name) {
  var greeting = 'IIFE: Hello '
  console.log(greeting + name);
}(firstname));
```

我們把程式碼包進 function 中，並立即建立它，然後呼叫，這是 IIFE，這對 JavaScript 的開發者是一個很棒的工具，我們幾乎會在所有大型的框架和函式庫看到。

#### 呼叫的差異

還有一件事，我們不僅能在表達式內呼叫，也能在表達式外呼叫，這沒有什麼差別，單純看使用習慣，無論想選哪一種方式，只要選擇一個，保持一致即可。

```JavaScript
var firstname = 'Apeiros0';

(function (name) {
  var greeting = 'IIFE: Hello '
  console.log(greeting + name);
}(firstname)); // () 內呼叫

// ***********************

(function (name) {
  var greeting = 'IIFE: Hello '
  console.log(greeting + name);
})(firstname); // () 外呼叫
```

## 總結

除了使用函式表達式來建立 IIFE 外，我們也能將表達式獨立出來，透過 `()` 運算子將表達式寫在裡面，而要呼叫時，我們不僅能在表達式內呼叫，也能在表達式外呼叫，這單純看個人的習慣，總之，今天對 IIFE 有一定程度的了解，相信能讓我們在參考大型框架或函式庫時能更加看懂裡面的程式碼。