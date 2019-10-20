---
title: 純值 (Primitive Types)
categories: JavaScript
tags:
  - 克服 JavaScript 奇怪部分筆記
comments: true
date: 2019-10-20
---
今天來了解在 JavaScript 中變數 (variables) 的基本資料型別吧 !

當我們宣告變數時，不用特別宣告變數的資料型別，JavaScript 會自動幫我們判定資料的型別，而在 JavaScript 中有六種純值 (primitive types，又稱基本型別)，在談這六種純值之前，我們先來聊聊什麼是純值。

## 純值 (Primitive Types)
> 純值是指一種資料的型別，表示一個單一值。

換句話說，純值並不是物件 (object)，因為物件是 Name/Value 配對的組合，而純值只是一個值 (single value)。


## 六種基本型別

### Undefined
> Undefined 表示還不存在。

這是 JavaScript 給所有變數的初始值，直到你賦予變數一個值之後，才不是 `undefined`。

> 注意：我們不應該用 `undefined` 來設定值。

``` JavaScript
var a = undefined; // 不要這樣做，a 會表示還沒設定值
```


### Null
> Null 也表示不存在。

null 適合拿來設定值，來表示變數不存在或沒有值，所以如果要表示值還不存在，不要用 `undefined`，用 `null` 比較好。

``` JavaScript
var a = null;
```


### Boolean
> Boolean 表示 `true` 或 `false`。

``` JavaScript
var a = true; // 或 false
```


### Number
> Number 表示數字。

在 JavaScript 中，唯一一種表示數字的型別，它是浮點數，代表所有的 number 都有小數點跟在後面，這與其它程式語言有很大的不同，其他程式語言會有表示整數和表示浮點數的型別，但在 JavaScript 只有一種型別，那就是 number。

``` JavaScript
var a = 1; // 實際上是浮點數，可與浮點數直接計算。
```

### String
> String 是由字元所組成，可用 `''` 和 `""` 來表示字串。

``` JavaScript
var a = 'a'; // 或 "a"
```


### Symbol
> Symbol 在 ES6 會使用到。

還在建立中，並沒有被全部瀏覽器所支援。

``` JavaScript
var a = Symbol('a');
```


## 總結
以上就是 JavaScript 六種基本資料型別，我們了解了純值，以及 JavaScript 是動態型別 (會立刻知道變數的型別是什麼)，而之後我們會談到另一個主題，了解這些主題後，對在 JavaScript 中常遇到的問題會很有幫助。
