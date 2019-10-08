---
title: 全域執行環境和全域物件
date: 2019-10-07
category: JavaScript
tags:
  - 克服 JavaScript 奇怪部分筆記
comments: true
---
關於執行環境我們在之前已經有一小部分的認識，今天要來了解執行環境中的全域執行環境及其所建立的全域物件。

在開始了解全域執行環境及全域物件時，我們先來了解什麼是全域? 
## 什麼是全域 (Global) ?
這裡的全域 (Global)，指的是**可以在任何地方取用它**的意思，更簡單的說，如果**不在 function 的範圍中那就是指全域**。

舉例來說：
在下方的程式碼中，變數 b 在 function a 中，所以變數 b 不是全域，而 function a 就是在全域中。
``` JavaScript
function a() {
  var b = "local";
}
```
反之，變數 b 在 function a 外，所以變數 b 是全域，function a 也同樣在全域中。
``` JavaScript
var b = "global";
function a() {}
```

## 全域執行環境 (Global Execution Context)
又稱 基礎執行環境 (base execution context) 會在每次 JavaScript 程式執行時產生，它會自動生成 **全域物件 (Global Object)** 和 **this 變數**給你，而你的程式會被包在全域執行環境中。


## 全域物件 (Global Object)

現在打開瀏覽器的開發者工具 (按下 `F12` 或是 `Ctrl + Shift + i`)，點擊 Console 視窗，觀察全域執行環境裡有什麼，剛剛有提到全域執行環境會生成全域物件和 this 變數，先輸入 `this`，會發現一個名為 Window 的物件，如下所示：
``` JavaScript
Window {parent: Window, postMessage: ƒ, blur: ƒ, focus: ƒ, close: ƒ, …}
```
由於這是在瀏覽器執行 JavaScript，所以這個 Window 物件指的是瀏覽器的視窗，有趣的是，當我輸入 `window` 時，也會出現一樣的結果。

那麼，這個 Window 物件是什麼呢?

> Window 物件是**瀏覽器的全域物件 (Global Object)**，在 JavaScript 執行時，永遠只會有一個全域物件。

- 補充：如果在伺服器上執行 node.js (JavaScript 的伺服器) 的話，這個全域物件指的將不是 Window 物件，而是名為 Global 的物件。

- 補充：如果我在瀏覽器開啟新的分頁，那便會產生新的全域物件，也就是說，每個分頁的全域物件都是獨立的，都擁有屬於自己的全域執行環境。

---

現在我們認識到全域執行環境會建立全域物件 (Window 物件)，只要是在全域執行環境內，任何程式都能取用，同時，全域執行環境還建立了 this 變數，**在全域 (Global Level) 中，this 變數等同於全域物件 (Window 物件)**。

現在來透過程式更加了解全域物件，
在這個的例子中，提到 function a 和 變數 b 都是在全域中，這是因為 JavaScript 在執行時，我建立的變數 b 和 function a 不在 function 裡面，他們便會和全域物件連結在一起，
``` JavaScript
var b = "global";
function a() {}
```

所以，當我在 Console 視窗輸入 `b` 的話，會顯示 `"global"`，而我再輸入 `window.b` 也同樣會顯示 `"global"`，因為變數 b 被加入到全域物件中。

---
## 總結
當 JavaScript 程式執行時，會建立全域執行環境，其中包括：
* **全域物件** (Global Object，由 JavaScript 建立，是執行環境的一部份，如果程式碼不在 function 內，那就是在全域物件中)
* **this 變數** (在全域執行環境中，this 等於全域物件)
* **外部環境** (Outer Environment，後面便會看到，在執行 function 時相當重要)
* **你的程式碼**