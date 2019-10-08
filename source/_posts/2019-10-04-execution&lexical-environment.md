---
title: 語法解析器、執行環境與詞彙環境
date: 2019-10-04
category: JavaScript
tags: 
  - 克服 JavaScript 奇怪部分筆記
comments: true
---
---
今天是第一次寫有關部落格的文章，我想重新複習有關 JavaScript 的觀念，如果有任何錯，還請麻煩指正，謝謝!

課程來源：[JavaScript 全攻略：克服 JS 的奇怪部分](https://www.udemy.com/course/javascriptjs/)

---

要了解 JavaScript，就必須得先從他是如何解讀程式碼，以及如何運作開始了解，首先先從一些名詞解釋開始

## 語法解析器 (Syntax Parsers)
語法解析器，又稱編譯器 (compiler)，能夠讀取你的程式碼，並將程式碼轉換成電腦能看懂的指令。

他會逐行逐字讀取你的程式碼，如果你程式的語法是有效的，便會將你的程式轉換成電腦看得懂的指令。

舉例來說：
假設我今天的程式是下方的樣子
```
function a() {
  var b = 'Hello World!'
}
```

透過語法解析器 (編譯器) 解析，會轉變成類似下方的電腦指令

```
// 電腦指令(Computer Instructions)
function, variable ...
```

## 詞彙環境 (Lexical Environments)
詞彙環境指的是程式碼在程式中的位置。

以上一段的程式碼舉例：
在 function a 中有一個變數 b，也就是說變數 b 的位置在 function a 的裡面。
```
function a() {
  // b 在 function 裡面
  var b = 'Hello World!'
}
```

值得注意的是，程式被寫在哪個位置是很重要的事，這可能會影響程式執行後的結果。


## 執行環境 (Execution Contexts)
執行環境負責管理正在執行的程式，一個程式有許多不同位置的程式碼 (詞彙環境)，但哪個才是正在執行的程式碼呢? 這部分就是由執行環境所管理。

而執行環境除了管理你寫的程式碼，以及正在執行的程式碼外，也會做其他的事，這部分後面的章節便能看到。
