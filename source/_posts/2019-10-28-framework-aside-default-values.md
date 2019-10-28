---
title: Framework Aside：預設值 (Default Values)
categories: JavaScript
tags:
  - 克服 JavaScript 奇怪部分筆記
comments: true
date: 2019-10-28
---

今天我們要來談談那些大型的框架 (framework, e.g. Angular.js) 或函式庫 (library, e.g. jQuery) 是如何應用我們在課程中所學過的東西，我們就先針對上一篇文章的預設值來討論吧 !

## 預設值

我們可以透過先前所學的，像是預設值、執行環境和全域環境來了解那些有名的框架和函式庫中的程式碼，舉例來說：

我們有一個 index.html 檔，可以參照到 app.js 檔，我們還有 lib1.js 和 lib2.js 函式庫 (或框架) 要匯入專案。

``` HTML
<!-- index.html 檔 -->
<html>
  <head>...</head>
  <body>
    <script src="lib1.js"></script>
    <script src="lib2.js"></script>
    <script src="app.js"></script>
  </body>
</html>
```

在 lib1.js 中，我們來設定個變數 `libraryName`，賦予 `Lib 1` 的值，這是由某個人所寫出來的函式庫。

``` JavaScript
// lib1.js 檔
var libraryName = 'Lib 1';
// ...
```

而 lib2.ks 是由另一個人所寫出來的函式庫，同樣的也設定了 `libraryName` 變數，但賦予的是不同的值。

``` JavaScript
// lib2.js 檔
var libraryName = 'Lib 2';
// ...
```

在 app.js 中，我們將 `libraryName` 變數輸出，那麼，當程式執行時，會發生什麼事呢 ?。

``` JavaScript
// app.js 檔
console.log(libraryName);
```

這裡我們要先來了解，當 JavaScript 檔案執行時，三個 `<script>` 標籤不會建立新的執行環境，**實際上它們會把程式碼堆疊在對方上面，然後執行堆疊好的 JavaScript，就好像他們在同一個檔案中**，也就是說，在產生 JavaScript 環境時，會結合並最小化 JavaScript 程式碼成為一個檔案，所以這三個檔案不會互相衝突是很重要的事。

### 產生的問題

當我們執行上面的範例時，會發現結果為 `Lib 2`，因為這些變數都是全域執行環境裡的全域變數，在瀏覽器中會與 window 物件連結，所以 `lib2.js` 的變數會將 `lib1.js` 變數覆蓋，導致 `app.js` 輸出時顯示 `lib2.js` 的變數。

這在函式庫 (或框架) 中是很不尋常的事，可能會讓他們互相衝突，或是不小心取代另一個函式庫 (或框架) 。

那我們該如何解決這個問題呢 ?

### 解決辦法

我們回到 `lib2.js` 來檢查 `libraryName` 是否已經在全域物件中，可以透過 `||` 運算子和設定 `window` 來檢查，如果在 `window` 物件中有 `libraryName` 變數，就不做任何事，如果沒有，就透過 `||` 運算子設定預設值 (沒有設定預設值就會是 `undefined`)。

``` JavaScript
// lib1.js 檔
var libraryName = 'Lib 1';
// ...
```

``` JavaScript
// lib2.js 檔
window.libraryName = window.libraryName || 'Lib 2';
```

修改完後重新執行，發現結果變成 `Lib 1`，因為 `lib1.js` 的 `libraryName` 先被執行，被放到全域物件中，所以當 `lib2.js` 執行時，會透過 `||` 運算子判斷是否有 `libraryName` 變數，藉由這個方式，我們可以減少函式庫 / 框架的互相衝突。

## 總結

在大部分的函式庫 (或框架) 中，可以看到某一行程式像這樣，這其實是在設定物件或定義函式庫 function 的集合，避免影響到其他函式庫，所以當看到程式像這樣時，這是在檢查全域的命名空間 (global namespace) 或全域物件看是否有名稱重複，避免產生衝突或覆蓋其他函式庫。

``` JavaScript
window.libraryName = window.libraryName || 'Lib 2';
```
