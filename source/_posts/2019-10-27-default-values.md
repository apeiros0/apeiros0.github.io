---
title: 預設值 (Default Values)
categories: JavaScript
tags:
  - 克服 JavaScript 奇怪部分筆記
comments: true
date: 2019-10-27
---
今天我們回顧一下運算子的強制轉型，並利用這個特性來做一些事吧 !

我們先建立一個 `greet` function，並設定一個 name 的參數，接著呼叫 `greet` function，傳入一些值，將結果輸出。

``` JavaScript
function greet (name) {
  console.log('Hello, ' + name);
}

greet('apeiros0');
```
程式執行後，我們知道結果為 `Hello, apeiros0`，那如果呼叫 `greet` function 時沒有傳入參數呢 ?

``` JavaScript
function greet (name) {
  console.log(name);
  console.log('Hello, ' + name);
}

greet();
```

JavaScript 和其他程式語言不同，它會當作 name 沒有任何東西，所以會輸出 `Hello, undefined`，這是因為 function 被呼叫時，執行環境建立，而 name 變數在 function 內被創造，先被賦予 `undefined`，之後執行階段再賦予值。

而在 `console.log` 中有使用 `+` 運算子，當字串與 name 作為參數時，name 會被強制轉型成字串，所以 `undefined` 會被轉成字串連接在一起，最後輸出 `Hello, undefined`。

但我們總不可能將 `undefined` 作為值輸出，該怎麼做才能解決這個問題呢 ?

## 預設值 (Default Values)
我們可以給 name 參數一個預設值，關於這部分有兩個方法可以使用：

### ES6 語法
我們可以直接賦予 name 參數一個值當作預設值。

``` JavaScript
function greet (name = 'apeiros0') {
  console.log('Hello, ' + name);
}

greet();
```

### `||` (OR) 運算子
使用 `||` 運算子是比較常見的做法，`||` 運算子不只會回傳 `true` 或 `false`，還可以回傳被強制轉型成 `true` 的值，如果兩個值都為 `true`，會回傳第一個為 `true` 的值，這也是為什麼能用 `||` 運算子來設定預設值。

``` JavaScript
false || true;     // true

undefined || 'Hi'; // 回傳 Hi

'Hello' || 'Hi';   // 回傳 Hello

0 || 1;            // 1

null || 'Hi';      // 回傳 Hi

'' || 'Hi';        // 回傳 Hi
```


所以，如果沒有傳入參數給 function，name 參數是 `undefuned`，而透過 `||` 運算子會將為 `true` 的 `apeiros0` 賦予給 name，最後輸出 `Hello, apeiros0`。

``` JavaScript
function greet (name) {
  name = name || 'apeiros0';
  console.log('Hello, ' + name); // Hello, apeiros0
}

greet();
```

如果有傳入參數，會將第一個為 `true` 的值賦予給 name，所以這裡的 `Stark` 會先輩賦予給 name。

> 補充：`||` 運算子的優先性 (5) 比 `=` 運算子 (3) 的優先性高，所以 `||` 會優先執行。

``` JavaScript
function greet (name) {
  name = name || 'apeiros0';
  console.log('Hello, ' + name); // Hello, Stark
}

greet('Stark');
```

> 注意：我們得注意數字 0，如果我們傳入 0，會被轉成 `false`，不會輸出 0。

## 總結
我們可以透過 ES6 或是 `||` 運算子這兩種方式來設定預設值，而 `||` 運算子的方法是透過強制轉型將為 `true` 的值回傳給你，但我們仍須注意數字 0，因為 0 會被轉為 `false`，不會顯示原本的值。