---
title: 程式執行
date: 2019-10-10
categories: JavaScript
tags:
  - 克服 JavaScript 奇怪部分筆記
comments: true
---
之前我們有提到，在 JavaScript 產生執行環境時，會有創造階段與執行階段，創造階段是將變數 和 function 加入到記憶體中，那執行階段呢?

今天我們就來談談執行階段會做什麼。

## 執行階段 (Execution Phase)
先前我們知道，創造階段會建立全域物件、this 變數和外部環境，而你的程式碼中的變數和 function 會被加入到記憶體中 (提升 Hoisiting)，並且變數會被賦予 `undefined` 的值。

等這些都建立完後，就到執行階段開始執行你式碼，你的程式碼會被**逐行**編譯、轉換成電腦的指令，我們用之前在[創造和提升 (Hoisiting)](https://apeiros0.github.io/JavaScript/2019/10/08/creation_and_hoisiting/)中的範例來驗證。

``` JavaScript
// 保持良好習慣，不要去依賴提升 (Hoisiting)
function a() {
  console.log('Called function a');
}

a();

console.log(b);

var b = "Called b";

console.log(b);
```

上方結果依序為 `Called function a`、`undefined` 和 `Called b`。

這裡我們知道在執行階段程式會逐行編譯、轉換，所以程式從最上面的 function a 開始往下執行，呼叫 a，印出 `Called function a`，到了第一個印出變數 b 的時候，由於在創造階段 b 被賦予 `undefined` 的值，所以會印出 `undefined`，接著 b 被賦予值，在最後一行便會印出 `Called b`。

## 總結
在執行階段 (Execution Phase) 會逐行編譯、轉換程式碼，並會賦予真正的值給變數。