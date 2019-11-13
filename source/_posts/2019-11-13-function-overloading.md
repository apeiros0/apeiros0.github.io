---
title: Framework Aside：Function Overloading
categories: JavaScript
tags:
  - 克服 JavaScript 奇怪部分筆記
comments: true
date: 2019-11-13
---
今天我們再次來討論如何將我們所學的應用到框架或函式庫中，我們先花一些時間來了解 JavaScript 所沒有，但其他程式語言有的，以及為何對 JavaScript 不重要的東西，這個東西是 function overloading (重載函式)。

## Function Overloading (重載函式)

在其他的程式語言 (C#, C++, Java) 都有 function overloading 的概念，這表示我們能讓相同名稱的 function 能夠擁有不同數量的參數，這在 JavaScript 是不行的，因為 function 是物件 (即是參數的數量不同，還是會參照到同一個位址)，所以 JavaScript 沒有這類處理 function 的功能。

不過沒有 function overloading 也不用擔心，因為 first-class function 給了 JavaScript 許多的選擇，但如果我們想用不同數量參數的方式呼叫同一個 function，我們該怎麼做 ?

## JavaScript 模仿 Function Overloading

我們有一個 greet function，如果不想每次都傳入 `language`，在上一章節我們知道可以使用預設參數，讓在 function 裡的程式碼來決定要在何種情境下使用哪一種 `language`，所以我們先給 `language` 預設值 `en`，如果 `language` 為 `en` 時，我們就輸出 `Hello + name`；為 `es` 時，就輸出 `Hola + name`。

```JavaScript
function greet (firstname, lastname, language) {
  language = language || 'en';

  if (language === 'en') {
    console.log('Hello ' + firstname + lastname);
  }

  if (language === 'es') {
    console.log('Hola ' + firstname + lastname);
  }
}
```

當我們要呼叫時，分別傳入不同的 `language`，執行時便會出現不同的結果。

```JavaScript
greet('apeiros', '0', 'en');
// Hello apeiros0

greet('apeiros', '0', 'es');
// Hola apeiros0
```

但我們可能會想讓 function 有其他不同版本，讓我們呼叫時，不必傳入過多的參數，這該怎麼辦呢 ?

我們可以新增兩個 function，greetEnglish 和 greetSpanish，分別帶有不同的默認參數 (`en`, `es`)，並透過呼叫 greet 帶入默認參數來做到，透過呼叫這兩個 function，我們就不必考慮要傳入什麼 `language`，直接呼叫這兩個 function 即可，他們可以傳入我們想要的參數。

```JavaScript
function greetEnglish (firstname, lastname) {
  greet(firstname, lastname, 'en')
}

function greetSpanish (firstname, lastname) {
  greet(firstname, lastname, 'es')
}

greetEnglish('apeiros', '0');
greetSpanish('apeiros', '0');
```

這是一個簡單處理 function 呼叫的方式，我們之後還會看到另一種，如果了解 JavaScript 的框架或函式庫，我們有時會看到像這類的方式，讓框架或函式庫更為好用。

## 總結

在 JavaScript 中沒有 function overloading 的概念，但我們可以新增其他的 function 傳入不同的參數到相同的 function 中，來模仿 function overloading。