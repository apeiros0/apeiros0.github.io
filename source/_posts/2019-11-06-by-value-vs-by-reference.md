---
title: By Value vs By Reference
categories: JavaScript
tags:
  - 克服 JavaScript 奇怪部分筆記
comments: true
date: 2019-11-06
---

今天我們來了解 by value 和 by reference，這是 JavaScript 重要觀念之一，有助於我們開發、除錯 JavaScript 程式和認識後面重要的觀念。

> 注意：這章節我們都是在討論變數。

## By Value

我們透過一些例子來了解吧，現在有一個變數 a，賦予它一個純值 (像是數值、布林、字串...)，它會指向純值在記憶體中的位址。

```
-------         --------------------
|  a  |  ---->  |  0x001  |  純值  |
-------         --------------------
```

然後我們設定新變數 b，執行 `b = a`，如果是在 JavaScript 中的純值，變數 b 會指向新的位址，然後將複製的純值放到那個位置上，變數 b 會得到 a 的純值 copy，a 和 b 就會有相同的值，這就是 **by value，藉由 copy 一個值到另一個不同的記憶體位置**。

```
-------         --------------------
|  a  |  ---->  |  0x001  |  純值  |
-------         --------------------

b = a

-------         ----------------------------
|  b  |  ---->  |  0x002  |  複製 a 的純值  |
-------         ----------------------------
```

### 程式範例

我們建立 a 和 b 兩個變數，設定 `a = 3`，b 不用設定任何東西，然後我們設定 `b = a`，會發生什麼事呢 ?

```JavaScript
var a = 3;
var b;

b = a;
```

我們知道 a 和 b 的值皆為 3，因為 3 是純值，會是 by value，當然，其他的純值也是一樣的，這是因為當 b 被設定為 a 時，`=` 運算子會建立一個新的記憶體位址給 b，會 copy a 的值，然後設定到 b 的位址中，所以 a 和 b 都會是 3，會在不同的記憶體位址。

這也代表，當我們改變 a 的值時，不會影響到 b，因為它們在不同的記憶體位址，這就是 by value。

```JavaScript
a = 10;

console.log(a); // 10
console.log(b); // 3
```

## By Reference

我們有個物件 (包含 function 和其他特殊類型的物件)，當我們設定變數給物件時，變數一樣會得到記憶體位址，知道物件在哪裡，並參照到物件。

```
-------         ----------------------
|  a  |  ---->  |  0x001  |  Object  |
-------         ----------------------
```

我們同樣設定新變數 b，執行 `b = a`，但與 by value 不同的是，b 不會得到新的記憶體位址，而是會指向與 a 相同的記憶體位址，沒有建立新的物件，也沒有物件的 copy 被建立，兩個變數都指向同一個位址，這就是 by reference。

```
-------         ----------------------
|  a  | ----->  |  0x001  |  Object  |
-------   |     ----------------------
          |
b = a     |
          |
-------   |
|  b  | ---
-------
```

### 程式範例

我們用物件實體語法建立一個物件，讓變數 c 指向它，而變數 d 我們設定 `d = c`，讓我們來看看執行後會發生什麼事 ?

```JavaScript
// by reference 包含物件和 functions
var c = { greeting: 'Hi' };
var d;

d = c;
```

在 `d = c` 中，`=` 運算子會知道 c 是物件，所以不會建立新的記憶體位址給 d，而是把 d 指向和 c 相同的記憶體位址，所以我們輸出後會看到相同的值，它們不是對方的 copy，它們是指向相同的記憶體位址，也就是說，當我們改變 c 的值時，d 也會跟著改變，就像下方這樣。

```JavaScript
c.greeting = 'Hello'; // mutate object

console.log(c); // Object { greeting: 'Hello' }
console.log(d); // Object { greeting: 'Hello' }
```

> 補充：mutate 是指改變 (change) 某些東西，當看到 mutate an object、mutate a value，就是改變它們的意思。

> 補充：Immutable 是指不可改變的。

### 透過 function 參數傳遞也是 by reference

當我們使用 function 的參數傳遞時也是一樣的，會透過 by reference 傳入，舉例來說：

我們建立一個 changeGreeting function，並將參數命名 obj，表示傳入物件，之後透過參數來改變物件的內容，然後我們呼叫 changeGreeting function，並傳入變數 d，d 就會被傳入到 function 中，然後改變他的值。

```JavaScript
function changeGreeting (obj) {
  obj.greeting = 'Hola'; // mutate object
}

changeGreeting(d);

console.log(c); // Object { greeting: 'Hola' }
console.log(d); // Object { greeting: 'Hola' }
```

我們將 c 和 d 的結果輸出後，會發現我們傳入物件到 function，就如同我們設定 `d = c` 一樣，是透過 by reference 傳入，所以 obj 會指向 d 的記憶體位置，而 d 是指向 c 的記憶體位置，所以當我們透過 function 改變時，會更新物件記憶體位址裡的值，這就是 by reference。

就像這樣：

```
-------           ----------------------
|  c  |  ------>  |  0x001  |  Object  |
-------    |      ----------------------
           |
-------    |
|  d  | ----
-------    |
           |
---------  |
|  obj  | --
---------
```

## `=` 運算子會設定新的記憶體空間

當我們將 c 指向新的物件時，`=` 運算子會設定新的記憶體空間給 c，並放進這個物件，所以 c 和 d 是指向不同的記憶體位址，這是特殊的例子，不是 by reference，因為 `=` 運算子看到 `{ greeting: 'Howdy' }` 還不存在於記憶體中，這是新的物件，是透過物件實體語法所建立，所以會建立新的記憶體位址給這個新物件，c 和 d 就指向不同的記憶體位址。

```JavaScript
c = { greeting: 'Howdy' };

console.log(c); // Object { greeting: 'Howdy' }
console.log(d); // Object { greeting: 'Hola' }
```

就像這樣：

```
-------           --------------------------
|  d  |  ------>  |  0x001  |  old Object  |
-------           --------------------------

-------           --------------------------
|  c  |  ------>  |  0x002  |  new Object  |
-------           --------------------------
```

## 總結

了解 by value 和 by reference 的差異相當的重要，在其他的程式語言中，甚至可以用語法決定要 pass by value 還是 pass by reference，但在 JavaScript 中沒有選擇，所有的純值都是 by value，而所有的物件都是 by reference，如果我們不了解這個觀念，會導致一些錯誤或 bug，一旦我們了解了，就會知道當物件被改變 (mutate) 時，所有指向相同記憶體位址的東西都會被改變，或是改變純值時，沒辦法改變最初的值，因為它是 by value，理解這些會有助於我們除錯，和了解後面重要的觀念。

## 參考資料

[JavaScript 是如何工作的：JavaScript 的記憶體模型](https://www.itread01.com/content/1555298404.html)
