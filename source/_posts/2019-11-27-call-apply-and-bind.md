---
title: Call(), Apply() And Bind()
categories: JavaScript
tags:
  - 克服 JavaScript 奇怪部分筆記
comments: true
date: 2019-11-27
---

今天我們討論三個 function 內建的方法 — `call()`, `apply()` 和 `bind()`。

## 執行環境與 `this`

在執行環境中有變數環境、外部環境參照、this 變數，我們已經在一些情況中看過 this 會指向全域物件，而在其他的情況下，如果 function 是物件中的方法，會指向包含 function 的物件。

```
Function Execution Context
——————————————————————————————————————————
|  Variable     |  this  |  Outer        |
|  Environment  |        |  Environment  |
——————————————————————————————————————————
```

## 如何控制 `this` 指向誰

JavaScript 有內建的 `call()`, `apply()` 和 `bind()` 方法能做到，我們先前有討論過 this，在 [物件、函式和 this](https://apeiros0.github.io/JavaScript/2019/11/08/objects-functions-and-this/) 的章節中，但對 function 的語法並沒有完整的介紹，因為需要先了解 first-class function 的概念，接下來我們就來介紹 `call()`, `apply()` 和 `bind()` 吧 !

## `call()`, `apply()`, `bind()` 介紹

我們已經知道 function 是特殊形態的物件，包含：

- NAME 屬性：沒有 NAME 就代表是匿名的
- CODE 屬性：包括程式碼，能被呼叫 (Invocable) 並執行程式。

```
——————————————
|  Function  |
——————————————
     ↓        ↘ Invocable ()
——————————    ——————————
|  NAME  |    |  CODE  |
——————————    ——————————
```

而 JavaScript 的 function 都能取用一些特別的方法，因為 function 是物件，可以有自己的屬性和方法，也就是說，所有的 function 都有 `call`, `apply` 和 `bind` 方法，這三個方法都與 this 以及傳入的參數有關。

```
                               ——————————————
                —————————————  |  Function  |  ————————————
             ／       |        ——————————————        |      ＼
          ↙          ↓                ↓             ↓        ↘ Invocable ()
————————————    —————————————    ————————————    ——————————    ——————————
|  call()  |    |  apply()  |    |  bind()  |    |  NAME  |    |  CODE  |
————————————    —————————————    ————————————    ——————————    ——————————
```

## 程式範例

我們有一個 person 物件，包含 `firstname` 和 `lastname` 屬性，還有 `getFullName` 方法，會回傳 `fullname` 變數，值是透過 this 取得物件的屬性。

由於 this 是物件的方法，所以 this 會指向 person 物件。

```JavaScript
var person = {
  firstname: 'apeiros',
  lastname: '0',
  getFullName: function () {
    var fullname = this.firstname + ' ' + this.lastname;
    return fullname;
  }
};
```

那 `call()`, `apply()` 和 `bind()` 要怎麼使用呢 ?

假設我們有 `logName` 的函式表達式在 person 物件的外面，它接受兩個參數，然後輸出 `this.getFullName()`。

```JavaScript
var logName = function (lang1, lang2) {
  console.log('Logged: ' + this.getFullName());
}

logName();
```

### `bind()`

我們建立新的函式表達式，叫做 `logPersonName`，然後使用 `logName` function 取得 `bind` 方法，這裡沒有先呼叫 `logName` 再設定 `bind`，是因為 `bind` 方法會回傳一個值，我們這裡是把 function 當作物件使用，然後呼叫物件的方法，所以這裡 `logName` 是函式物件，`bind` 是所有 function 內建的方法。

`bind` 方法的第一個參數需要傳入 this 指向的物件，它會回傳新的 function，會複製 `logName`，然後設定 person 為 this 指向的物件。

```JavaScript
var logPersonName = logName.bind(person);
```

當呼叫 `logPersonName` 後，執行環境被建立，JavaScript 引擎會看到這是使用 `bind` 建立，然後在背後做一些設定，當引擎決定 this 變數時，會認定 this 是 person，這個被傳入給 `bind` 方法的物件。

```JavaScript
logPersonName();
```

我們呼叫 `logPersonName`，是在呼叫透過 `bind` 建立的 copy function，當要使用 this 時，將會是 `person` 物件，所以呼叫 `logPersonName` 便能成功輸出結果。

```JavaScript
Logged: apeiros 0
```

我們也能在 `logName` 後直接使用 `bind` 方法。

```JavaScript
var logName = function (lang1, lang2) {
  console.log('Logged: ' + this.getFullName());
}.bind(person);

logName();
```

> **`bind` 方法會建立任何呼叫它的函式物件的 copy，而 this 變數會指向我們所傳入的物件 (是 by reference)**。

#### 傳入參數

我們也能透過 `bind` 傳入參數，先讓我們在 `logName` 將參數輸出

```JavaScript
var logName = function (lang1, lang2) {
  console.log('Logged: ' + this.getFullName());
  console.log('Arguments: ' + lang1 + ' ' + lang2);
  console.log('------------------');
}

var logPersonName = logName.bind(person);
logPersonName();
```

呼叫後，我們會得到下方的結果，因為我們還沒傳入參數。

```JavaScript
Logged: apeiros 0
Arguments: undefined undefined
------------------
```

我們在 `logPersonName` 傳入參數 `en`，我們這裡只是複製 `logName` function，它仍會接受一樣的參數。

```JavaScript
logPersonName('en');
```

呼叫後，我們會得到下方的結果。

```JavaScript
Logged: apeiros 0
Arguments: en undefined
------------------
```

### `call()` 和 `apply()`

接著我們來看 `call()` 和 `apply()`。

#### `call()`

我們可透過 `call` 方法來呼叫 function，和透過 `()` 直接呼叫是一樣的，`call` 也能決定 this 變數指向的物件，而傳入 `call` 的第一個參數是 this 指向的地方。

```JavaScript
// 兩者是相同的
logName.call(person);
logName();
```

我們也能傳入參數。

```JavaScript
logName.call(person, 'en', 'es');
```

當程式執行時，結果如下：

```JavaScript
Logged: apeiros 0
Arguments: en es
------------------
```

> **與 `bind` 會建立 function 的 copy 不同，`bind` 不會執行 function，而 `call` 會直接執行 function，然後決定 this 變數會是什麼，剩下的則是傳給 function 的參數**。

#### `apply()`

除了參數的部分，其餘與 `call` 相同。

如果我們像 `call` 方法一樣直接傳入參數，會得到 `Uncaught TypeError: CreateListFromArrayLike called on non-object` 錯誤。

```JavaScript
// Error
logName.apply(person, 'en', 'es');
```

因為 **`apply` 方法是接受陣列作為參數**，這是 `call` 和 `apply` 唯一的差別。

> 陣列在數學運算方面比較實用

```JavaScript
logName.apply(person, ['en', 'es']);
```

輸出的結果如下：

```JavaScript
Logged: apeiros 0
Arguments: en es
------------------
```

> **依據 function 的情況，我們有 `call` 和 `apply` 兩種選擇**。

#### `call` 和 `apply` 的 IIFE

我們可以透過 `call` 或 `apply` 來實現 IIFE，所有的 function 都能使用這兩個方法，只要告訴它 this 和傳入的參數是什麼即可。

```JavaScript
(function (lang1, lang2) {
  console.log('Logged: ' + this.getFullName());
}).apply(person, ['en', 'es'])
```

執行的結果如下：

```JavaScript
Logged: apeiros 0
Arguments: en es
------------------
```

## 如何應用

我們透過兩個實例來了解 `call()`, `apply()` 和 `bind()` 如何應用。

### Function Borrowing (函式借用)

以上面的範例為例，我們再建立一個物件 person2，這與 person 物件很像，只是這個物件沒有 `getFullName` 方法，我們要透過 `call` 或 `apply` 方法來實作 Function Borrowing。

```JavaScript
var person2 = {
  firstname: 'Ben',
  lastname: 'Stark'
};
```

我們先取得 person 物件的 `getFullName` 方法，然後透過 `apply` 方法將 this 指向 person2 物件。

```JavaScript
person.getFullName.apply(person2);
```

輸出的結果如下：

```JavaScript
Ben Stark
```

> 這裡是**透過 `apply` 借用 function**，`call` 方法也能做到相同的事，只要物件具有相似的屬性，就能從其他物件取得方法來使用。

### Function Currying

function currying 與 `bind` 方法有關，因為會建立 function 的 copy，如果我們傳入參數給 `bind` 方法，會發生一些有趣的事。使用 `call` 或 `apply` 方法，僅僅只是傳遞參數，但使用 `bind` 方法可以建立 function 的 copy。

我們先來建立新的 function 叫做 multiply，它會回傳兩個參數相乘的結果。

```JavaScript
function multiply(a, b) {
  return a * b;
}
```

再建立新的 function，叫做 multipleByTwo，然後使用 `bind` 方法建立 multiply 的 copy，這裡我們用不到 this，所以不用在意第一個參數是什麼，但我們要給它一個參數值。

```JavaScript
var multipleByTwo = multiply.bind(this, 2);
```

我們知道 `bind` 方法不會執行 function，我們給它的參數會設定為 copy function 的永久參數值，所以我們設定 `bind` 第一個參數為 `2`，在這個 copy function 中，第一個參數永遠為 `2` (參數 `a` 永遠是 `2`)。

這就像取得這個 function，設定 this 一樣，這裡直接設定 `a = 2`，這就是這裡在做的事。

```JavaScript
// 示意的函式
function multipleByTwo(b) {
  var a = 2;
  return a * b;
}
```

`bind` 方法會設定 `2` 給參數 `a`，然後我們呼叫 multipleByTwo，傳入 `3`。

```JavaScript
console.log(multipleByTwo(3));
```

輸出的結果如下：

```JavaScript
6
```

這裡是取得 multiply function 的 copy，this 會被設定好，然後設定第一個參數 (a) 的定值，當我們呼叫這個 copy function，無論我們傳遞什麼，都會是第二個參數 (b) 的值。

如果將兩個參數都傳給 bind，會將兩個參數都設為定值，不論我們傳什麼給 copy function，結果都不會改變。

```JavaScript
var multipleByTwo = multiply.bind(this, 2, 5);
console.log(multipleByTwo(123456789));
```

結果如下：

```JavaScript
10
```

如果我們不傳入參數給 `bind`，參數 `a` 和 `b` 就不會受到限制。

```JavaScript
var multipleByTwo = multiply.bind(this);
console.log(multipleByTwo(6, 6));
```

結果如下：

```JavaScript
36
```

在這個範例中，我們是故意設定參數為定值，copy function 會有特定的預設參數，我們用一個 function 建立一個新 function，並設定預設參數，這就叫做 Currying。

> **Function Currying 是建立一個新的 copy function，帶有一些預設參數，這在數學運算非常實用**。

如果有函式庫需要做很多數學運算，能透過基本的 function，根據不同的預設參數建構不同的 function。

## 總結

這就是 `call`, `apply` 和 `bind` 方法。

- `call`, `apply` 方法會呼叫 function，並讓我們設定 this，這兩個方法是以不同方式傳遞參數。
- `bind` 方法會建立 function 的 copy，並讓我們設定 this，還能讓我們設定預設參數 (永久的預設參數)。
