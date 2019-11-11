---
title: 陣列：任何東西的集合 (Array Collections Of Anything)
categories: JavaScript
tags:
  - 克服 JavaScript 奇怪部分筆記
comments: true
date: 2019-11-11
---

今天我們來了解 JavaScript 的陣列 (array) 是什麼吧 !

## 陣列 (Array)

> 陣列是包含許多東西的集合，可以被宣告。

我們可以使用 `new Array()` 的方式來宣告陣列。

```JavaScript
var arr = new Array();
```

或是使用更簡短的方式，使用陣列的實體語法來宣告，這與物件實體語法類似，但不是用 `{}`，而是使用 `[]`。

```JavaScript
var arr = [];
```

我們可以在當中放入一些值，使用 `,` 來隔開。

```JavaScript
arr = [1, 2, 3];
```

在 JavaScript 中，陣列的索引是以 0 為基準，這表示我們能使用 `[]` 來決定要抓出那些值，0 是抓第一個值，1 是抓第二個值，以此類推 ...

```JavaScript
arr[0];
```

### 與其他語言的不同之處

JavaScript 的陣列與其他語言的陣列有點不一樣，大部分的程式語言，陣列是一連串相同型別的集合，像是數值陣列、字串陣列、物件陣列...等，而由於 JavaScript 是屬於動態型別，可以立即知道型別是什麼，這也代表我們可以在陣列放入其他不同型別的東西，舉例來說：

我們可以放入字串、數值，布林，或是物件，還有我們知道 function 是物件，所以也能放入到陣列中。

```JavaScript
var arr = [
  'Hello',
  1,
  false,
  {
    name: 'apeiros0',
    address: '111 Main St.'
  },
  function (name) {
    var greeting = 'Hello';
    console.log(greeting + ' ' + name);
  }
];

console.log(arr);
```

現在這個陣列包含了字串、數值、布林、物件和 function，輸出後發現沒有任何錯誤，這也代表 JavaScript 的陣列可以是任何東西的集合，所以我們可能會看到一些很奇特的程式碼，像是：

```JavaScript
arr[3](arr[2].name);
```

這段程式碼的意思是，當我們要參照到 function 時，會透過 `arr[3]` 來取得，而要呼叫 function 時，我們知道要透過 `()` 來呼叫，而這個 function 需要傳入 name，所以會透過 `arr[2]` 來取得物件，再透過 `.` 運算子取得 `name` 屬性。

## 總結

陣列可以包含任何東西，加上 JavaScript 是屬於動態型別，所以我們可以參照到任何型別的東西，當然也包括 function，甚至還能呼叫它。
