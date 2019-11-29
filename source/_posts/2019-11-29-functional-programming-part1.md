---
title: 函式程式設計 (一) (Functional Programming Part1)
categories: JavaScript
tags:
  - 克服 JavaScript 奇怪部分筆記
comments: true
date: 2019-11-29
---

在討論完 first-class function 和其他的 JavaScript 特色後，我們要來討論函式程式設計 (Functional Programming)。

雖然 JavaScript 聽起來和 Java 有關，但它其實和函式程式語言比較有關，像是 Lisp, Scheme, ML，這些語言有 first-class function 的特色，function 就是物件，可以作為參數傳入，可以從 function 中回傳，所以有 first-class function 的 JavaScript，代表可以實作函式程式設計，能將程式碼都當成 function。

函式程式設計引入新的思考方式來實作程式設計，讓我們透過一些範例來了解吧 !

## 範例

我們先從簡單的陣列開始，我們在裡面放一些值，並輸出陣列。

```JavaScript
var arr1 = [1, 2, 3];
console.log('arr1: ' + arr1);
```

我們再新增一個陣列，然後遍歷 `arr1` 的每個項目，透過 `push()` 將 `arr1` 每個值乘上 `2` 加入到 `arr2` 中，並輸出 `arr2`。

```JavaScript
var arr2 = [1, 2, 3];

for(var i = 0; i < arr1.length; i++) {
  arr2.push(arr1[i] * 2);
}

console.log('arr2: ' + arr2);
```

我們會得到：

```JavaScript
arr1: [1, 2, 3]
arr2: [4, 5, 6]
```

## 函式程式設計 (Functional Programming)

作為程式設計師，我們需要簡化我們的工作，盡可能減少重複的事情，所以我們會將任務放入到 funtion 中，減少重複的頻率發生，而在沒有 first-class function 的語言中，能將程式碼切割到多小會受到限制，透過 first-class function，我們能做到不同的事情。

舉例來說：

我們修改上面的範例，建立新的 function，命名為 mapForEach，參數是陣列和 function。

```JavaScript
function mapForEach (arr, fn) {}
```

mapForEach 做的事和上面相似，我們新增一個陣列和迴圈，迴圈能對傳入的陣列做計算，並 `push` 到 `newArr` 裡，由於我們引入函式程式設計，所以在 `push` 時，呼叫傳入的 function，加入到陣列中，最後回傳 `newArr`。

```JavaScript
function mapForEach (arr, fn) {
  var newArr = [];
  for(var i = 0; i < arr.length; i++) {
    newArr.push(
      fn(arr[i])
    );
  }

  return newArr;
}
```

然後我們改寫 `arr2`，呼叫 mapForEach，傳入 `arr1`，並用函式表達式建立 function，它接受 `item` 參數，表示我們要傳入的東西，然後回傳被處理過的 `item`。

```JavaScript
var arr2 = mapForEach(arr1, function (item) {
  return item * 2;
});

console.log('arr2: ' + arr2);
```

輸出的結果是：

```JavaScript
arr2: [4, 5, 6]
```

我們得到相同的結果，這是將程式碼切割到 function 中，並提供任務讓 function 處理陣列的每個項目，這表示我們可以做完全不同的事。

像是：

建立第三個陣列 `arr3`，我們給的任務是將陣列符合的東西找出來，告訴我們是否有大於 `2` 的項目，並回傳 boolean 值。

```JavaScript
var arr3 = mapForEach(arr1, function (item) {
  return item > 2;
});

console.log('arr3: ' + arr3);
```

結果如下：

```JavaScript
arr3: [false, false, true]
```

> 我們重複使用 mapForEach 做不同的任務，透過傳遞 function，傳遞針對陣列的任務，就可以讓陣列完成不同的任務，這是函式程式設計的經典例子，我們使用 first-class function 分割我們的程式碼，讓我們能寫出更簡潔的程式碼。

---

讓我們再看看其他例子。

我們用 mapForEach 檢查是否有超過限制，建立新的 function，可以傳入 `limiter` (限制值)，和 `item` (陣列傳進來的值)，檢查 `item` 是否大於 `limiter`，這與上面的範例類似，我們只是用變數取代固定的數字，檢查是否超過傳入的限制值。

```JavaScript
var checkPastLimit = function (limiter, item) {
  return item > limiter;
}
```

但現在有個問題，checkPastLimit 接受兩個參數，但傳給 mapForEach 的 function 只需要一個參數即可，所以我們要先設定 `limiter` 參數，之後傳遞 `item` 進來。

```JavaScript
function mapForEach (arr, fn) {
  var newArr = [];
  for(var i = 0; i < arr.length; i++) {
    newArr.push(
      fn(arr[i]) // 只需要一個參數
    );
  }

  return newArr;
}
```

所以我們建立 `arr4`，傳入 `arr1` 陣列和 checkPastLimit，然後對 checkPastLimit **使用 `bind`**，不必在意 this 是什麼，然後設定參數 `1` 給 `limiter`。

`checkPastLimit.bind(this, 1)` 建立了 function 的 copy，並用 `1` 作為 `limiter`。

> 如果將參數放到 `bind`，會將參數設為 copy function 的預設值

```JavaScript
var arr4 = mapForEach(arr1, checkPastLimit.bind(this, 1));

console.log('arr4: ' + arr4);
```

輸出的結果如下：

```JavaScript
arr4: [false, true, true]
```

> 我們建立 checkPastLimit，傳入 `limiter`，檢查陣列值是否大於它，而 `item` 參數會對陣列每個項目做處理，並回傳 boolean 值，接著呼叫 mapForEach，傳入陣列和透過 `bind` 做出的 checkPastLimit copy (會設定 `limiter` 為 `1`)。

---

如果覺得每次呼叫 `bind` 很麻煩，只想傳入 `limiter` 作為唯一的參數，然後回傳 `limiter` 設定好的 function。

我們可以建立 checkPastLimitSimplified function，它只會接受 `limiter` 作為參數，並回傳函式表達式 (會透過 `bind` 將 `limiter` 設定好，回傳 copy function)，所以當我們執行 checkPastLimitSimplified，會回傳 `bind` 處理好的 function。

```JavaScript
var checkPastLimitSimplified = function (limiter) {

  return function (limiter, item) { // 這裡的 limiter 會被 bind 設定好
    return item > limiter;

  }.bind(this, limiter); // limiter 會傳到這裡

};

var arr5 = mapForEach(arr1, checkPastLimitSimplified(2));

console.log('arr5: ' + arr5);
```

結果如下：

```JavaScript
arr5: [false, false, true]
```

這不只是將程式碼分割成 function，我們可以思考如何讓 function 或回傳的 function 更加簡化，一開始可能會很困難，但習慣後，會是很直覺的事情，分割程式碼到 function 中，讓 function 互相傳遞，能讓任務更快速地完成。

> 這裡要注意的是，我們的 function，尤其是小 function，當我們在移動或傳遞它們時，盡量不要改變 (mutate) 資料，這可能會遇到一些不好的情況，當要改變資料時，最好能夠在層級高的 function 改變資料，或是不要更動原本的資料，回傳新的資料回來。

下一章節我們要花點時間來討論，JavaScript 中經常使用的函式程式設計例子。
