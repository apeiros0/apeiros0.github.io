---
title: Framework Aside：空格 (Whitespace)
categories: JavaScript
tags:
  - 克服 JavaScript 奇怪部分筆記
comments: true
date: 2019-11-18
---

今天我們來談談 JavaScript 的空格是什麼吧 !

## 空格 (Whitespace)

> 空格是指可以在程式碼中建立文字空間 (Literal Space) 的無形字元 (Invisible Character)，像是 Carriage returns (Enter), tabs, spaces，它們可讓我們的程式碼有更高的可讀性，但不會被真正的執行。

我們可以透過這些空格來幫助自己，而 JavaScript 的語法解析器對空格是相當自由的，我們可以利用這個特性來設計程式，舉例來說：

我們宣告幾個變數 `firstname`, `lastname`, `language`，再建立一個 person 物件，然後輸出這個物件。

```JavaScript
var firstname, lastname, language;

var person = {
  firstname: 'Apeiros',
  lastname: '0'
}

console.log(person);
```

當我們執行時，會得到物件的輸出結果，然後我們的程式碼也有一些變數，但如果我們隔一段時間回來看，或是其他開發者要使用這段程式時，我們該怎麼解釋這段很久之前寫的程式碼呢?

### 註解 (Comments)

我們可以加上註解，方便其他人更容易理解我們寫的程式碼，也能讓我們隔一段時間後能了解程式在做什麼，而 JavaScript 對空格是非常自由的，所以我們能將註解放在任何地方，像是：

放在 `var` 和 `firstname` 之間，當**語法解析器看到 `//` 時，會忽略 `//` 後的東西，到下一行繼續執行**。

```JavaScript
var
   // first name
   firstname,
   lastname,
   language;
```

我們也可以在不同變數間加上註解，或加上多行的註解。

```JavaScript
var
   // first name
   firstname,

   // last name
   lastname,

   // language
   // can use 'en', 'es' ...
   language;
```

由此可知，我們可以自由地使用空格來描述我們的程式碼，當然我們也能在物件中使用。

```JavaScript
var person = {
  // first name
  firstname: 'Apeiros',

  // last name
  // (required)
  lastname: '0'
}
```

### 註解的重要性

當我們打開那些知名的框架或函式庫，會發現它們使用很多的空格和註解，雖然一開始可能會不太習慣，可能會隔好幾行才看到另一行程式碼，而這中間會有註解在解釋，但不要覺得麻煩，不要把他們砍掉，這是程式設計師在利用 JavaScript 的空格來告訴我們這段程式在做什麼，所以當看到像這些複雜的 JavaScript 程式時，要懷抱感謝的心情，這代表有很多註解在幫助我們了解這段程式，不要被這些空格嚇到，我們應該要盡可能**使用註解來讓我們的程式碼更容易閱讀**。

## 總結

今天我們了解了空格可以如何利用，以及我們能在任何地方加上註解，而在我們寫程式時，應當要加上註解，方便我們和其他人更能容易閱讀程式。
