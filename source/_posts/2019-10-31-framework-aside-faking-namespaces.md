---
title: Framework Aside：偽裝名空間 (Faking Namespaces)
categories: JavaScript
tags:
  - 克服 JavaScript 奇怪部分筆記
comments: true
date: 2019-10-31
---
今天讓我們來談談該如何應用我們所學的知識，了解那些知名函式庫或框架常出現的東西，在經過前面的運算子和物件實體語法後，我們要來討論偽裝名空間 (Faking Namespaces)。

先讓我們來認識什麼是命名空間 (Namespaces) 吧 !

## 命名空間 (Namespaces)
> 命名空間是指變數和 function 的容器，用來維持相同名稱的變數和 function 分離。

但這裡有個問題，JavaScript 沒有命名空間，如果不知道什麼是命名空間，我們將會帶你認識；如果知道什麼是命名空間，會發現 JavaScript 可以透過物件做偽裝，不需要命名空間的功能。

舉例來說：
我們有兩個 greet 變數，值分別是 `Hello` 和 `Hola`，當我們輸出 greet 時，會有什麼結果呢 ?

``` JavaScript
var greet = 'Hello';
var greet = 'Hola';

console.log(greet);
```

我們會知道在創造階段 greet 變數會被設為 `undefined`，然後在執行階段，程式碼會被依順序逐行執行，所以結果為 `Hola`。

但如果這兩個 greet 變數分別在不同的 JavaScript 檔案、函式庫或框架中，就會產生一些問題，由於我們分別設定 greet 變數，加上是在全域執行環境中，會導致後一個變數覆寫前一個變數。

而透過命名空間可以幫助我們解決這些問題，因為命名空間有容器可以分開裝著這些 greet 變數，但問題是 JavaScript 沒有命名空間，我們該怎麼辦 ?

## 偽裝命名空間 (Faking Namespaces)
我們可以藉由建立一個物件，成為屬性、方法和我們想使用的東西的容器，來偽裝命名空間。

以上面的範例為例，我們使用物件實體語法分別給 english 和 spanish 變數建立物件，把這些物件當作容器，分別設定 greet 屬性給他們，雖然它們的屬性相同，但不會產生衝突、不會覆寫。

``` JavaScript
// english 和 spanish 變數成為一個容器，不會與其他 JavaScript 檔案和變數因全域命名空間而產生衝突
var english = {}; // english 的容器
var spanish = {}; // spanish 的容器

english.greet = 'Hello';
spanish.greet = 'Hola';
```

### 保持不同物件的層級
如果我們想在 english 物件中加入 greetings 的命名空間，在 greetings 物件中我想新增不同的問候語，但我不能直接使用 `english.greetings.greet` 加入，這會出現 `Uncaught TypeError: Cannot set property ... of undefined` 的錯誤訊息。

這是因為 `.` 運算子的關係，由於 `.` 運算子是左相依性，所以會先執行 `english.greetings`，由於我們沒有建立 `english.greetings`，所以會回傳 `undefined`，最後變成 `undefined.greet`，而我們都知道 `undefined` 並非物件，所以才會出現錯誤訊息。

``` JavaScript
var english = {}; // english 的容器
var spanish = {}; // spanish 的容器

english.greetings.greet = 'Hello'; // 這樣是不可以的
spanish.greet = 'Hola';
```

所以我們必須先建立 `english.greetings` 的物件，才能使用。

``` JavaScript
var english = {}; // english 的容器
var spanish = {}; // spanish 的容器

english.greetings = {};  // 必須先建立物件才行

english.greetings.greet = 'Hello'; // 這樣是不可以的
spanish.greet = 'Hola';
```

或是我們直接在 `english` 物件初始化 `greetings`。
``` JavaScript
var english = {
  greetings: { // 或是直接初始化物件
    greet: 'Hello'
  }
}; // english 的容器
var spanish = {}; // spanish 的容器

spanish.greet = 'Hola';
```

## 總結
偽裝命名空間我們可以想像成一個容器，裝著不同的的屬性 (e.g. 純值、物件或方法)，透過獨立每個容器，我們可以避免與其他程式產生衝突。