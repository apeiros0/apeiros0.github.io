---
title: 物件和物件實體語法 (Objects Snd Object Literals)
categories: JavaScript
tags:
  - 克服 JavaScript 奇怪部分筆記
comments: true
date: 2019-10-30
---

上一篇文章中，我們有提到用 `new Object()` 來建立物件不是最好的方法，今天就讓我們來談談物件和物件實體語法 (Objects Snd Object Literals) 吧 !

## 物件實體語法 (Object Literals)

在 JavaScript 中，做一件事有很多種的方法，除了使用 `new Object()` 來建立物件外，還有更簡單的方法能建立物件，那就是使用物件實體語法 — `{}` 來建立物件。

> 注意：`{}` 不是運算子，當 JavaScript 解析語法時，看到 `{}`，而你又沒有與 function, if 或 for 迴圈一起使用時，它會認為你在建立物件。

```JavaScript
var person = {};
```

除了建立物件外，我們還能初始化物件，可以同時建立屬性和方法在 `{}` 中，如果要增加一組 Name/Value，我們能使用 `:` 來建立，就像 `name: value` 這樣，而要增加多組 Name/Value，我們能使用 `,` 隔開，來增加另一組 Name/Value，舉例來說：

```JavaScript
var person = { firstName: 'apeiros', lastName: '0' };

console.log(person);
```

這和使用 `new Object()` 來建立物件，並透過 `=` 來賦予值是一樣的。

```JavaScript
var person = new Object();

person.firstName = 'apeiros';
person.lastName = '0'

console.log(person);
```

在 JavaScript 中，不管是空白鍵、tab 鍵或 enter 鍵，都會將你的程式碼視為同一行，所以我們能改寫上方的範例，提高程式碼的易讀性。

```JavaScript
var person = {
  firstName: 'apeiros',
  lastName: '0'
};

console.log(person);
```

除了純值之外，我們也能在物件中增加新的物件，方式與使用物件實體語法一樣。

> 補充：`{}` 比使用 `new Object()` 還快上許多，也是建議使用的方式來快速建立物件。

```JavaScript
var person = {
  firstName: 'apeiros',
  lastName: '0',
  address: {
    street: '111 Main St.',
    city: 'New York',
    state: 'NY'
  }
};

console.log(person);
```

### 任何地方都能建立物件
我們已經了解 `{}` 是如何運作，`{}` 能快速建立物件和變數，也能在 function 呼叫時快速建立，就像我們傳入字串和數字一樣，舉例來說：

我新增一個 `apeiros` 個人資訊的物件，並建立一個 greet function 能打招呼，我們的 function 需要知道物件的 `firstName` 才能打招呼，所以當我傳入 `apeiros` 物件時，會回傳我 `Hi, apeiros` 的結果。

另外，我們也可以在呼叫 function 的同時，建立物件並當作參數傳入，就如同傳入字串或數字一樣。

```JavaScript
var apeiros = {
  firstName: 'apeiros',
  lastName: '0',
  address: {
    street: '111 Main St.',
    city: 'New York',
    state: 'NY'
  }
};

function greet (person) {
  console.log('Hi, ' + person.firstName);
}

greet(apeiros);

greet({ firstName: 'Mary', lastName: 'Doe' }); // 呼叫的同時會快速建立物件

greet("apeiros"); // function 呼叫時，字串也建立了
```

我們也能混合 `.` 運算子和 `{}` 物件實體語法來快速建立物件。

```JavaScript
apeiros.address2 = {
  street: '222 Main St.',
  city: 'New York',
  state: 'NY'
}
```

為什麼 `.` 運算子和 `{}` 物件實體語法能運作呢 ?

因為我們所寫的程式碼不是直接被處理，而是會被 JavaScript 轉換成電腦懂得指令，所以不論是 `{}` 或 `.` 運算子都是一樣的，它們在 JavaScript 底層就是在建立物件和找到物件的屬性或方法，所以我用什麼語法對 JavaScript 沒有任何差別，都是在建立同樣的東西。

## 總結
了解物件的實體語法，我們能用更快速、簡單易懂的方式建立物件。