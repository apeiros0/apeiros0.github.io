---
title: 單執行緒和同步執行
date: 2019-10-11
categories: JavaScript
tags:
  - 克服 JavaScript 奇怪部分筆記
comments: true
---
今天來了解 JavaScript 中的單執行緒 (Single Threaded) 和同步執行 (Synchronous Execution)。


## 單執行緒 (Single Threaded)
> 單執行緒 (Single Threaded) 表示一次執行一個指令，簡單來說，就是 JavaScript 一次只做一件事。

在我們的程式碼中有許多的指令 (commands)，單執行緒便是一次只執行一個指令。舉例來說，假設我有一個 function a 和 function b，不論我是先呼叫 a，還是先呼叫 b，JavaScript 一次都只會執行一個 function，不會同時執行兩個 function。
``` JavaScript
function a () {
  console.log('Called a');
}

function b () {
  console.log('Called b');
}

// 先呼叫 a，再呼叫 b
a();
b();

// 先呼叫 b，再呼叫 a
b();
a();
```
補充：在瀏覽器中，不單單只有 JavaScript 引擎，還會有其他不同的引擎在同時執行，所以瀏覽器是屬於多執行緒 (Multi-thread)，一次能做許多不同的事，這與後面會談到的非同步有很大的關係。

## 同步執行 (Synchronous Execution)
> 同步執行 (Synchronous Execution) 表示會依照程式碼的順序，從上到下一次執行一行程式碼。

同步 (Synchronous) 在程式語言中表示一次執行一行而且會依照順序。

## 總結
從單執行緒和同步執行，我們了解到 JavaScript 會依照順序執行，而且一次只會做一件事。