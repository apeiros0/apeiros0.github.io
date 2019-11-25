---
title: Framework Aside：Function Factories
categories: JavaScript
tags:
  - 克服 JavaScript 奇怪部分筆記
comments: true
date: 2019-11-25
---

在 JavaScript 中，Closure 是非常實用的，今天我們就來談談如何應用 Closure。

先前我們有介紹過一些 JavaScript 程式語言的功能，像是讓 function 在不同情形下被不同預設參數呼叫 (在 [Function Overloading](https://apeiros0.github.io/JavaScript/2019/11/13/function-overloading/) 的章節中)，而在這章節我們要展示如何用 Closure 寫出更有彈性的程式。

## Function Factory

我們要建立 factory，**factory 會回傳幫我們做事的 function**，所以 makeGreeting 會回傳 function，並設定 `language` 參數，然後回傳函式表達式，內容與 Function Overloading 章節中寫的 greet function 一樣，不同的地方是我們不把 `language` 傳到函式表達式，而是傳給 makeGreeting，然後回傳內部的 function，所以 `language` 會被包在 Closure 中，當我們要參照到它時，只要執行內部的 function，就會透過範圍鏈尋找，即使 makeGreeting 離開執行堆，我們仍可取用 `language`。

```JavaScript
function makeGreeting(language) {
  return function (firstname, lastname) {
    if(language === 'en') {
      console.log('Hello ' + firstname + lastname);
    }

    if(language === 'es') {
      console.log('Hola ' + firstname + lastname);
    }
  }
}
```

### 建立 `greetEnglish` 和 `greetSpanish` 變數

我們建立 `greetEnglish` 和 `greetSpanish` 變數，並呼叫 makeGreeting，傳入 `en` 和 `es` 參數，執行後我們會得到什麼呢 ?

第一次呼叫 makeGreeting，傳入 `en`，會建立自己的執行環境，並回傳一個函式表達式，它的外部參照會指向 makeGreeting 執行時 `language` 的值，所以回傳的 function 會指向 `language` 變數，值為 `en`。

在第二次呼叫 makeGreeting 會有自己的執行環境，`language` 的值為 `es`，會在不同的記憶體位置。

即使這兩個的詞彙環境是一樣的 (都在 makeGreeting 中)，但它們還是指向不同的記憶體位置，因為它們在不同的執行環境被建立，會分別有不同的 Closure 在這兩個執行環境中。

`greetEnglish` 是函式物件，它的 Closure 指向 `language = en`；`greetSpanish` 是另一個函式物件，Closure 指向不同的執行環境，`language` 會是 `es`。

> 雖然他們是相同的 function，但每次執行時，會建立新的執行環境，是新的記憶體空間，不論我們呼叫多少次。

```JavaScript
var greetEnglish = makeGreeting('en');
var greetSpanish = makeGreeting('es');
```

### 呼叫 `greetEnglish` 和 `greetSpanish`

現在我們來呼叫 `greetEnglish` 和 `greetSpanish`，傳入 `firstname` 和 `lastname`，他們會到範圍鏈尋找 `language`，第一次會找到 `en`，第二次則是 `es`，所以結果會是 `Hello apeiros0` 和 `Hola apeiros0`。

```JavaScript
greetEnglish('apeiros', '0'); // Hello apeiros0
greetSpanish('apeiros', '0'); // Hola apeiros0
```

### 解說

makeGreeting 充當了 factory function，透過 Closure 設定 makeGreeting 裡回傳的 function 的參數值，也就是說，我們建立一個可以永遠取用初始參數 (initial parameter) 的 function，而我們的 `greetEnglish` 和 `greetSpanish` 無法直接取用 `language` 的值，它會被藏起來，而無法在全域環境 (global context) 中使用，但在執行時，能透過 Closure 取用。

## 背後發生的事

這是我們全部的程式碼，function factory。

```JavaScript
function makeGreeting(language) {
  return function (firstname, lastname) {
    if(language === 'en') {
      console.log('Hello ' + firstname + lastname);
    }

    if(language === 'es') {
      console.log('Hola ' + firstname + lastname);
    }
  }
}

var greetEnglish = makeGreeting('en');
var greetSpanish = makeGreeting('es');

greetEnglish('apeiros', '0');
greetSpanish('apeiros', '0');
```

當程式開始執行，全域執行環境建立，裡面會有 makeGreeting function, `greetEnglish` 和 `greetSpanish` 變數。

```
—————————————————————————————————————————————————————————————
|  Global Execution Context  |  makeGreeting()              |
|                            |  greetEnglish, greetSpanish  |
—————————————————————————————————————————————————————————————
```

### 建立 `greetEnglish` 和 `greetSpanish` 變數

當執行到 `greetEnglish = makeGreeting('en')` 時，會呼叫 makeGreeting，建立新的執行環境，傳入 `language` 參數，值為 `en`，然後回傳函式表達式，存入到 `greetEnglish` 變數。

```
—————————————————————————————————————————————————————————————
|  makeGreeting()            |  language = 'en'             |
—————————————————————————————————————————————————————————————
|  Global Execution Context  |  makeGreeting()              |
|                            |  greetEnglish, greetSpanish  |
—————————————————————————————————————————————————————————————
```

之後 makeGreeting 離開執行堆，執行環境的記憶體空間仍存在。

```
                             ————————————————————————————————
                             |  language = 'en'             |
—————————————————————————————————————————————————————————————
|  Global Execution Context  |  makeGreeting()              |
|                            |  greetEnglish, greetSpanish  |
—————————————————————————————————————————————————————————————
```

然後執行到 `greetSpanish = makeGreeting('es')`，再次呼叫 makeGreeting，這與 [了解閉包 (二)](https://apeiros0.github.io/JavaScript/2019/11/24/understanding-closure2/) 的範例是不同的，那時我們是在陣列中加入 function，只呼叫外部的 function 一次，所以陣列中的 function 都會指向同一個記憶體空間，但在這個範例中，我們呼叫兩次的 function，當呼叫第二個 function，會得到新的執行環境，每當我們呼叫 function，都會產生新的執行環境，無論我們呼叫都少次，而這個執行環境會有自己的變數環境，在這個範例中 `language` 便是 `es`。

```
—————————————————————————————————————————————————————————————
|  makeGreeting()            |  language = 'es'             |
—————————————————————————————————————————————————————————————


                             ————————————————————————————————
                             |  language = 'en'             |
—————————————————————————————————————————————————————————————
|  Global Execution Context  |  makeGreeting()              |
|                            |  greetEnglish, greetSpanish  |
—————————————————————————————————————————————————————————————
```

然後回傳新的函式表達式，並離開執行堆，現在我們有兩個記憶體位置，分別是不同的執行環境。

```
                             ————————————————————————————————
                             |  language = 'es'             |
                             ————————————————————————————————


                             ————————————————————————————————
                             |  language = 'en'             |
—————————————————————————————————————————————————————————————
|  Global Execution Context  |  makeGreeting()              |
|                            |  greetEnglish, greetSpanish  |
—————————————————————————————————————————————————————————————
```

### 呼叫 `greetEnglish` 和 `greetSpanish`

當我們呼叫 `greetEnglish` 時，是在呼叫回傳的函式表達式，會建立新的執行環境，`firstname` 是 `apeiros`，`lastname` 是 `0`。

我們知道外部環境參照需要指向 makeGreeting 建立的其中一個執行環境，因為函式表達式的詞彙就在 makeGreeting 內，JavaScript 引擎會知道 `greetEnglish` 的 function 是被建立在第一個 makeGreeting 的執行環境中，所以會指向它，這就是 Closure，最後 greetEnglish 會給我們 `Hello` 的結果。

```
                             ————————————————————————————————
                             |  language = 'es'             |
                             ————————————————————————————————

                             Closure
————————————————————————————————————————————————————————————————————————
|                                                                      |
|  —————————————————————————————————————————————————————————————       |
|  |  (), greetEnglish          |  firstname = 'apeiros'       |  ———  |
|  |  Execution Context         |  lastname = '0'              |    |  |
|  —————————————————————————————————————————————————————————————    |  |
|                               |  language = 'en'             |  <——  |
|                               ————————————————————————————————       |
————————————————————————————————————————————————————————————————————————


—————————————————————————————————————————————————————————————
|  Global Execution Context  |  makeGreeting()              |
|                            |  greetEnglish, greetSpanish  |
—————————————————————————————————————————————————————————————
```

當執行到第二個 greetSpanish 時，同樣產生自己的執行環境，但 greetSpanish 是第二次呼叫被建立，它的外部參照會指向第二個 makeGreeting 建立的執行環境的記憶體空間，也有自己的 Closure，所以當 greetSpanish 執行時，`language` 會是 `es`，然後給我們 `Hola` 的結果

```
                             Closure
————————————————————————————————————————————————————————————————————————
|                                                                      |
|  —————————————————————————————————————————————————————————————       |
|  |  (), greetSpanish          |  firstname = 'apeiros'       |  ———  |
|  |  Execution Context         |  lastname = '0'              |    |  |
|  —————————————————————————————————————————————————————————————    |  |
|                               |  language = 'es'             |  <——  |
|                               ————————————————————————————————       |
————————————————————————————————————————————————————————————————————————


                             Closure
————————————————————————————————————————————————————————————————————————
|                                                                      |
|  —————————————————————————————————————————————————————————————       |
|  |  (), greetEnglish          |  firstname = 'apeiros'       |  ———  |
|  |  Execution Context         |  lastname = '0'              |    |  |
|  —————————————————————————————————————————————————————————————    |  |
|                               |  language = 'en'             |  <——  |
|                               ————————————————————————————————       |
————————————————————————————————————————————————————————————————————————


—————————————————————————————————————————————————————————————
|  Global Execution Context  |  makeGreeting()              |
|                            |  greetEnglish, greetSpanish  |
—————————————————————————————————————————————————————————————
```

## 總結

當我們呼叫 function 時，會得到自己的執行環境，而在裡面建立的 function 會指向那個執行環境，指向記憶體空間，這是 JavaScript 語言的特色，所以我們可以透過這點製作 function factory，不需要每次都傳入相同的參數，我們可以回傳新的 function，透過 Closure 建立預設的參數。
