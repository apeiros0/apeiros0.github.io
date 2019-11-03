---
title: functions 是物件 (Functions Are Objects)
categories: JavaScript
tags:
  - 克服 JavaScript 奇怪部分筆記
comments: true
date: 2019-11-03
---

今天我們來討論 JavaScript 最基本、最重要的觀念，從只會使用 JavaScript 到深入了解它，我們就先來介紹 — first-class functions 吧 !

> 本篇重點：**在 JavaScript 中，functions 是物件**

## First-Class Functions (頭等函式)

在所有程式語言中，JavaScript 不是唯一有 First-Class Functions 的程式語言，任何對其他的型別 (像是物件、字串、數值、布林) 所做的事，都能對 function 做，像是：
- 指派 function 為變數的值
```JavaScript
var fn = function () { ... }
```
- 將 function 當作參數傳入另一個 function
```JavaScript
function fn(cb) {
  cb();
}
function fn2() { ... }

fn(fn2);
```
- 使用實體語法快速建立 function
```JavaScript
function fn() { ... }
var fn_obj = new fn();
```

First-Class Functions 改變我們寫程式的方式，讓我們用不同的方式解決問題。

### functions 是物件
當我們說 JavaScript 的 functions 是物件時，function object 是什麼樣的呢 ?

function object 就像 JavaScript 的其他物件一樣，它會在記憶體中，是一個特殊的物件，它擁有所有物件的特色，和一些其他的屬性。

```
--------------------------
|        Function        |
|  (一個特殊的 function)  |
--------------------------
```

這部分也是大家常常驚訝的一件事，function 可以有屬性和方法，因為 function 只是個物件，所以我們能連接純值 (Primitive) 到 Name/Value 配對，

```
                  --------------------------
                  |        Function        |
      ----------  |  (一個特殊的 function)  |
      |           --------------------------
      ↓
---------------
|  Primitive  |
---------------
```

也可以連結到物件，

```
                  --------------------------
                  |        Function        |
      ----------  |  (一個特殊的 function)  |
      |           --------------------------
      ↓                ↓
---------------   ------------
|  Primitive  |   |  Object  |
---------------   ------------
```

和連接到其他 function，

```
                  --------------------------
                  |        Function        |
      ----------  |  (一個特殊的 function)  |
      |           --------------------------
      ↓                ↓               ↓
---------------   ------------   --------------
|  Primitive  |   |  Object  |   |  function  |
---------------   ------------   --------------
```

function object 還有一些隱藏的特殊屬性，這當中有兩個最為重要，其中一個是 **NAME 屬性**，JavaScript 的 function 不一定要有名稱，可以是匿名的 (anonymous) ，

```
                  --------------------------
                  |        Function        |
      ----------  |  (一個特殊的 function)  |-------------------
      |           --------------------------                  |
      ↓                ↓               ↓                      ↓
---------------   ------------   --------------   -------------------------
|  Primitive  |   |  Object  |   |  function  |   |  NAME  (可以是匿名的)  |
---------------   ------------   --------------   -------------------------
```

另一個是 **CODE 屬性**，我們在 function 中所寫的程式碼都會放在這個屬性中，會成為 function object 的特殊屬性，所以我們所寫的程式並非 function 本身，只是 function object 其中的一個屬性，而  CODE 屬性最特別的是，它是能**呼叫的 (invocable)**，代表我們能執行這個 function 程式碼，

```
                  --------------------------
                  |        Function        |
      ----------  |  (一個特殊的 function)  |---------------------------------------
      |           --------------------------                  |                   |  invocable ()
      ↓                ↓               ↓                      ↓                   ↓
---------------   ------------   --------------   -------------------------   ----------
|  Primitive  |   |  Object  |   |  function  |   |  NAME  (可以是匿名的)  |   |  CODE  |
---------------   ------------   --------------   -------------------------   ----------
```

### 範例程式

我們可以把 function 想像成物件，而程式碼就是那個物件的屬性之一，我們就透過範例來了解吧 !

我們有一個 greet 的 function，可以輸出 `hi`，我們知道在 JavaScript 中，function 是物件，所以我們可以用 `.` 運算子來建立屬性。

```JavaScript
function greet () {
  console.log('hi');
}

greet.language = 'english';
```

我們新增一個屬性給 function，在其他的程式語言中，這是不可能辦到的事情，但在 JavaScript，function 是物件，而我們寫的程式碼只是物件的屬性之一。

如果我們輸出 `greet`，會得到我們寫的 function 的文字，在這情況中，沒有什麼用，

```JavaScript
console.log(greet);
```

```JavaScript
// 輸出後會得到這段文字
function greet () {
  console.log('hi');
}
```


但如果我們用 `.` 運算子找出 function 的屬性，就可以輸出 `language` 這個屬性，它就存在於記憶體中，是 function 的屬性，就像我們可以在物件中擁有屬性一樣。

```JavaScript
console.log(greet.language);
```

```
// 得到 english
english
```

所以當我們建立 greet function，它會被放到記憶體中，然後有 NAME 屬性，值是 `greet`，是我們幫 function 命名的；還有 CODE 屬性，包含我們所寫的程式碼，也就是 function 的內容，如果我們使用 `()` 來呼叫 greet，就會呼叫 CODE 屬性，讓它執行，建立執行環境，以此類推，就像下方這樣，

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

## 總結
function 在記憶體中有特定的位置，可以有自己的屬性和方法，因為在 JavaScript 中，functions 是物件。