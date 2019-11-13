---
title: 參數與展開 (Arguments And Spread)
categories: JavaScript
tags:
  - 克服 JavaScript 奇怪部分筆記
comments: true
date: 2019-11-12
---

今天我們要來談談 JavaScript 的另一個特殊關鍵字，是 JavaScript 引擎在執行 function 時自動設定好的，我們稱之為 arguments (參數)，而在下一版的 JavaScript 中，可能不會這麼常使用 arguments，我們會提到一個新的方法來處理 arguments 所做的事情，那便是 Spread，不過如果我們去看那些框架或函式庫，還是能在程式碼中看到 arguments 出現。

## Arguments 如何產生

當我們執行 function 時，會建立新的執行環境，JavaScript 引擎會幫我們設定一些東西，像是變數環境 (Variable Environment)，會保存我們的變數、外部環境 (Oiter Environment)，參照到範圍鏈、this，依據 function 的位置或如何呼叫而指向不同的物件、還有 arguments (參數)，包含所有我們傳入 function 的值。

```
              執行環境建立 (Function)

-------------   ----------   -------------
|  變數環境  |   |  this  |   |  外部環境  |
-------------   ----------   -------------
              ---------------
              |  arguments  |
              ---------------
```

## Arguments 介紹

Arguments (參數) 只是傳入到 function 的參數 (parameters) 的另一個名稱，也能稱它為 parameters，其他的程式語言都是如此稱呼，而 JavaScript 則給我們一個包含所有參數的關鍵字 arguments，所以我們在討論 arguments 的概念時，是在討論傳入到 function 的參數 (parameters)

### 程式範例

我們新增一個 greet function，參數有 `firstname`、`lastname` 和 `language`，然後分別輸出這些參數。

```JavaScript
function greet(firstname, lastname, language) {
  console.log(firstname);
  console.log(lastname);
  console.log(language);
}
```

如果我們呼叫 greet，會輸出這些參數的值，這部分就是 JavaScript 與其他語言不同的地方，我們可以直接呼叫 greet，不傳入任何的參數，這在其他程式語言會產生錯誤，它們會預期有參數傳進來，但在 JavaScript 不會出現錯誤，會出現 `undefined`，這是提升 (hoisting)，當 function 執行時，會設定好參數的值，即使我們沒有給它們值，也就是說，greet 執行時，第一件事是設定記憶體空間給 `firstname`、`lastname` 和 `language`，然後賦予 `undefined`。

```JavaScript
greet();
// firstname: undefined
// lastname: undefined
// language: undefined
```

如果我們有傳入參數，會從左到右依序對應，像是當我們只傳入一個參數時，會對應 function 的第一個參數，所以在執行下方程式碼時，`firstname` 的值會是 `apeiros`，其他兩個會因為提升，所以仍是 `undefined`。

```JavaScript
greet('apeiros');
// firstname: apeiros
// lastname: undefined
// language: undefined
```

我們也可以依序傳入不同值呼叫看看，當 function 執行時，會分別得到前兩個有值，第三個是 `undefined` 和三個全都有值的結果。

```JavaScript
greet('apeiros', '0');
// firstname: apeiros
// lastname: 0
// language: undefined

greet('apeiros', '0', 'en');
// firstname: apeiros
// lastname: 0
// language: en
```

這也就代表，我們可以省略傳入參數或是只傳入一部份的參數，這在 JavaScript 是沒有問題的。

## 預設參數

在下一版的 JavaScript，我們可以設定預設值給參數，如果我們沒有傳入值，就會以預設值為主。

```JavaScript
function greet(firstname = 'apeiros', lastname = '0', language = 'en') {
  console.log(firstname);
  console.log(lastname);
  console.log(language);
}
```

但並非所有瀏覽器都支援下一版本的預設值，不過我們還是能借用這部分的概念來設定參數，我們可以使用 `||` 運算子來設定，`undefined` 會被 `||` 運算子強制轉型成 `false`，所以最終會回傳 `||` 運算子後方的值，然後再透過 `=` 運算子賦予參數值，所以參數就會為我們所預設的值。

```JavaScript
function greet(firstname, lastname, language) {
  firstname = firstname || 'apeiros';
  lastname = lastname || '0';
  language = language || 'en';

  console.log(firstname);
  console.log(lastname);
  console.log(language);
}
```

## arugments

現在我們來看 JavaScript 會幫我們設定的特殊關鍵字 `arugments`，就如同 `this` 一樣，不用宣告就能直接取用，執行環境會確保這個被設定好，所以當我們輸出 `arugments` 時會得到什麼呢 ?

```JavaScript
function greet(firstname, lastname, language) {
  console.log(arguments);
}

greet('apeiros', '0', 'en');
```

`arugments` 會包含傳給參數的值，看起來很像陣列，但卻不是陣列，我們稱之為類陣列 (array-like)，它只有陣列一部份的功能。

### 使用的方式

如果我們想在沒有傳入參數的時候，做一些處理，可以檢查 `arugments.length` 是否為 0。

```JavaScript
function greet(firstname, lastname, language) {
  if (arugments.length === 0) {
    console.log('Missing parameters');
    return;
  }

  console.log(arguments);
}

greet();
```

> function 加上 `return;` 時，會跳出 function 不再執行，即使還沒全部執行完。

雖然 `arguments` 沒有包含參數的名稱，只有值，但我們能像陣列一樣取用，找出對應的參數值 (只有在呼叫 function 所傳入的參數才能被 `[]` 取用)。

```JavaScript
function greet(firstname, lastname, language) {
  console.log('arg0: ' + arguments[0]);
}

greet('apeiros'); // 這樣 arguments 才能透過 [] 取用
```

## ES6 Spread

隨著時間過去，`arguments` 會逐漸過時，這代表雖然它還在，但卻不是最好的方法，不過也會有新的方法產生，這新的方法是 spread parameter，如果我們有傳給 function 參數，我們可以用 `... + name` (name 可以自定義) 來增加這個參數到 function 中，當有多餘的參數傳進 function 時，透過輸出 `name` 可以顯示這些多餘的參數 (同樣是 array-like)，而如果有已命名的參數，會以它們為優先，然後才顯示剩餘的參數。

```JavaScript
function greet(firstname, lastname, language, ...others) {
  console.log(others);
}

greet('apeiros', '0', 'es', '111 Sec St.', 99);
// others: ['111 Sec St.', 99]
```

這是比較好的方式來處理一連串的參數，雖然這個方式不錯，不過現在還是有很多地方在使用 `arguments`。
