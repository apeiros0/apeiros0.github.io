---
title: Dangerous Aside：自動插入分號 (Automatic Semicolon Insertion)
categories: JavaScript
tags:
  - 克服 JavaScript 奇怪部分筆記
comments: true
date: 2019-11-15
---

當我們遇到 Dangerous Aside 的單元，代表我們要警告一些程式語言真正危險的地方，沒有程式語言是完美的，我們已經看過幾個 JavaScript 需要注意的地方，但這個單元要講的是真正危險的地方，會非常容易犯錯，也很難追蹤，是我們一定要避免的情況，此次的 Dangerous Aside 與 JavaScript 的語法解析器和它的自動插入分號 (Automatic Semicolon Insertion) 有關。

## 自動插入分號 (Automatic Semicolon Insertion)

JavaScript 的語法解析器會盡可能幫我們做一些事，也許有人已經發現 `;` 在 JavaScript 不是必要的，我們不一定需要 `;`，為什麼呢 ?

這是因為 JavaScript 引擎幫我們做一些事，會知道程式語言在期待什麼，會知道語法該長什麼樣子，而當我們結束一行程式碼，按下 Enter 時，會出現 `↓` (carriage return)。

```JavaScript
return↓
```

> `↓` 是我們看不見的字元，但它確實存在，所以語法解析器看到它，知道它是什麼

在遇到 `↓` (carriage return) 時，語法解析器會自動幫我們插入分號，任何語法解析器預期有 `;` 的地方，會幫我們補上，我們可以不用自己打出來，不是真的不需要分號，而是 JavaScript 引擎會幫我們在該放的地方補上。

```JavaScript
return;
```

雖然自動加入分號很方便，但這個單元的主要想提醒**永遠要記得打 `;`**，因為我們不會想讓 JavaScript 引擎幫我們決定，要確保程式碼是自己想要的樣子，否則會引發一些問題。

### 引發的問題

以 `return` 為例，自動插入分號會引起很大的問題，舉例來說：

我們建立一個 getPerson function，它會回傳一個簡單的物件，並呼叫 getPerson，輸出回傳的結果。

```JavaScript
function getPerson() {
  return
  {
    firstname: 'apeiros0',
  }
}

console.log(getPerson());
```

執行後，我們發現結果為 `undefined`，為什麼會這樣 ?

這是因為自動插入分號，JavaScript 引擎在 `return` 關鍵字後發現 `↓` (carriage return)，它會自動加上 `;`，所以當執行 getPerson 時，會是下方的樣子，語法解析器改變我們的程式碼，由於物件被放在新的一行上，導致 JavaScript 引擎在 `return` 後加上 `;`，`return;` 會直接回傳，離開 getPerson，然後我們就會得到 `undefined`。

```JavaScript
function getPerson() {
  return; // ← 自動插入分號了
  {
    firstname: 'apeiros0',
  }
}
```

### 解決辦法

如果要解決這部分的問題，我們要告訴語法解析器，我們正在做什麼，預防它自動幫我們插入分號，所以我們可以直接在 `return` 後方使用物件實體語法，這樣語法解析器就會知道我們要使用物件實體語法，看到 `{` 後的 `↓` (carriage return，按下 Enter 會產生) 就不會自動插入 `;`。

```JavaScript
function getPerson() {
  return { // 把 { 放到 return 後方，就不會自動插入分號
    firstname: 'apeiros0',
  }
}
```

> 補充：可能有人注意到，我們每次都會把 `{}` 放在 function、for 迴圈、if 陳述式的同一行中，這不是必要的，但這樣做可以保證不會有任何錯誤產生。

## 總結

當我們按下 Enter 後會產生 `↓`，而語法解析器就會自動幫我們插入分號，雖然方便，但會產生一些問題，所以我們要自己加上分號，盡量避免問題產生，這很危險也很難追蹤。