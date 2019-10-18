---
title: 什麼是非同步回呼 ?
categories: JavaScript
tags:
  - 克服 JavaScript 奇怪部分筆記
comments: true
date: 2019-10-17
---
我們先前有討論過 JavaScript 的同步，以及如何同步執行，今天我們就來討論什麼是非同步回呼 (asynchronous callbacks) 吧 !

在了解非同步回呼之前，我們先來了解什麼是非同步 (asynchronous)。

## 非同步 (asynchronous)
> 表示在同一個時間點不只有一個程式在執行。

也就是說，JavaScript 可以在程式執行時，執行另一段程式碼，然後又再執行別的程式碼，但這與我們之前說的 JavaScript 是同步執行 (同一時間只執行一行) 不是完全不同嗎 ?

還有像是 click 事件或是從遠端取得資料，有 callback function 可以執行，JavaScript 是如何處理這類的非同步事件而 callback function 又是什麼呢，讓我們來看看吧 !

## 如何處理非同步
首先，我們先來思考 JavaScript 引擎是位在哪裡吧，我們知道在瀏覽器中，不會只有 JavaScript 引擎在運作，還會有其他引擎在 JavaScript 外執行別的程式，

```
         瀏覽器
------------------------
|    JavaScript 引擎    |
------------------------
```

在瀏覽器中有需多不同引擎，我們先來看看比較常使用到的引擎，像是，
* Rendering 引擎：可以渲染或繪製畫面到網頁上
* HTTP Request：處理瀏覽器的 HTTP Request 和 Response，像是從伺服器取得資料
* 其他引擎 ...

JavaScript 可以和這些引擎溝通，像是與 Rendering 引擎溝通，就能改變網頁的樣子，或是向 HTTP Request 請求遠端資料，這些引擎**在瀏覽器內是非同步執行的**，而 **JavaScript 則是屬於同步執行**，也就是說，瀏覽器會在同一時間執行不同的引擎，包括 JavaScript 引擎，而在 JavaScript 內則是逐行執行程式。

> 補充：當我們在說引擎時，其實也只是別人寫的程式。

```
                                  瀏覽器 (非同步)

------------------------  
|    Rendering 引擎    |  ←-----------  溝通
------------------------             |
                                     ↓
                          -------------------------------  
                          |    JavaScript 引擎 (同步)    |
                          -------------------------------
                                      ↑ 
                                      |               ------------------------
                                溝通   ------------→  |      HTTP Request     |
                                                      ------------------------
```

那當我們非同步向外請求，或是當某個人觸發 click 事件時，會發生什麼事 ? 這就與事件佇列有關了。

### 事件佇列 (event queue)
先前我們有學過執行堆的概念，在程式執行時，會建立全域執行環境，當 function 被呼叫，執行堆會加入 function 的執行環境，然而在 JavaScript 引擎中還有個`事件佇列 (event queue)`，**負責處理可能會觸發的事件或事件的通知**。

當 JavaScript 引擎外有事件發生，在 JavaScript 中事件會被放到事件佇列裡，舉例來說，當有人在畫面上點擊時，或是我們從遠端取得資料等，在 JavaScript 內就會將這些事件加入到事件佇列中，

```
         執行堆

----------------------------
|    function b 執行環境    |
----------------------------
|    function a 執行環境    |          事件佇列 (event queue)
----------------------------      ----------------------------
|        全域執行環境       |      |  click  |  HTTP Request  |
----------------------------      ----------------------------
```

這邊要注意的是，**當執行堆清空時，JavaScript 才會注意到事件佇列**，這是什麼意思呢 ?

就是當 function b 和 a 執行結束，離開執行堆，等到全域執行環境執行完裡面的程式，執行堆為空時，JavaScript 就會去檢查事件佇列，看是否有 function 被事件觸發。

```
           JavaScript: 我來檢查囉 !

     執行堆
-----------------
|               |
|               |       事件佇列 (event queue)
|               |    ----------------------------
|               |    |  click  |  HTTP Request  |
-----------------    ----------------------------               
```

所以 JavaScript 看到 click 事件，知道有 function 需要執行，就會建立執行環境給這個 function，而當事件處理完畢，就會繼續執行下一個佇列的事件，像這樣不段的循環。

> **再次注意：事件佇列會直到執行堆為空才開始處理。**

```
         執行堆
                                      事件佇列 (event queue)
----------------------------      ----------------------------
|   clickHandler 執行環境   |      |  click  |  HTTP Request  |
----------------------------      ----------------------------
```

> 注意：這並不是真正的非同步，只是把瀏覽器非同步的東西放到事件佇列，原本的程式仍然是逐行在執行，當執行完，執行堆為空時，才會來處理事件，而事件所觸發的 function，會在執行堆中執行。

## 了解非同步回呼
> callback function 是指將 function 作為另一個 function 的參數使用，確保 function 能正常執行。

我們來用範例程式了解 JavaScript 如何處理非同步回呼吧 !

我們有 `waitThreeSeconds` function 和 `clickHandler` function (click 後觸發)，還有監聽 click 事件的 `addEventListener` (callback function)，現在如果我們在等待 3 秒期間，觸發 click 事件的話，會發生什麼事，而 console.log 出現的順序又是什麼呢 ?

``` JavaScript
function waitThreeSeconds () {
  var ms = 3000 + new Date().getTime();
  while (new Date() < ms) {} // 小於 3 秒，跑迴圈
  console.log('finished function');
}

function clickHandler () {
  console.log('click event~');
}

// 監聽 click 事件
document.addEventListener('click', clickHandler);

waitThreeSeconds();
console.log('finished execution');
```

我們先不觸發 click 事件，來看看執行結果是如何。

```
finished function
finished execution
```

執行完後，會發現結果如上方所示，由於沒有觸發 click 事件，所以會呼叫 `waitThreeSeconds` 執行，建立執行環境，等待 3 秒後，印出 `finished function`，最後回到全域執行環境，執行 `console.log('finished execution');`，印出 `finished execution` 的結果。

--- 

現在我們在等待 3 秒期間觸發 click 事件，看看會發生什麼。

```
finished function
finished execution
click event~
```

我們會發現結果分別為上方那樣，這是因為 JavaScript 引擎會等到執行堆為空，才來檢查事件佇列，所以當 `waitThreeSeconds();` 和 `console.log('finished execution');` 執行完就會執行事件佇列裡的 click 事件的 function。


## 持續檢查 (continuous check)
上述就是 JavaScript 如何同步處理在瀏覽器別處的非同步事件發生，JavaScript 會執行執行堆的程式，執行完，到事件佇列檢查，而當事件佇列也執行完，會繼續看事件佇列的 loop，這稱之為**持續檢查 (continuous check)**，當有其他事件出現在事件佇列中，就會去執行對應的 function。


## 總結
我們了解了 JavaScript 如何用同步的方式處理非同步事件，在引擎外所發生的事件會被放到事件佇列中，如果執行堆為空，JavaScript 就會來依事件發生順序來處理事件，所以非同步回呼在 JavaScript 是會發生的，只是非同步的部分是在 JavaScript 引擎外發生，而 JavaScript 會透過事件迴圈來持續檢查事件，用同步的方式處理。
