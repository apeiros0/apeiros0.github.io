---
title: 型別和 JavaScript
categories: JavaScript
tags:
  - 克服 JavaScript 奇怪部分筆記
comments: true
date: 2019-10-18
---
JavaScript 與其他語言相當的不同，特別是在變數的資料和型別的部分，和 JavaScript 處理他們的方式。

我們先來了解 JavaScript 處理型別的方式的名詞。

## 動態型別 (Dynamic Typing)
> 你不用告訴 JavaScript 你的變數是何種型別，不需要在程式裡寫出來，JavaScript 會在程式執行時知道你的變數型別是什麼。

所以當我們執行程式時，一個變數能在不同時候有不同的型別，這是因為型別是在執行時才知道，我們甚至不用用任何關鍵字宣告變數的型別。

舉例來說，
我只要宣告 `var`，當程式執行時 JavaScript 就會自動決定變數的型別，也可以隨時更改型別。

``` JavaScript
var isNew = true; // no errors
isNew = 'Hello World';
isNew = 123;
```

## 靜態型別 (Static Typing)
當你在其他程式語言時，像是 Java 或 C# 等，他們是使用**靜態型別**的方式處理，**代表一開始就要告訴編譯器你的變數型別是什麼**。

舉例來說，
我用 boolean 宣告變數，那值就必須是 `true` 或 `false`，如果放如像是字串或數字，那就會出現錯誤。

``` Java
boolean isNew = 'true'; // error 
```