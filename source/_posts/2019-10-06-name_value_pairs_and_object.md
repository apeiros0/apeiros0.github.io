---
title: Name/Value 配對和物件
date: 2019-10-06
categories: JavaScript
tags:
  - 克服 JavaScript 奇怪部分筆記
comments: true
---
---
今天來介紹 JavaScript 中重要的資料型態**物件 (Object)**，要了解物件就必須得先從 Name/Value 配對開始了解。

## Name/Value 配對 ##
Name/Value 配對，代表一個 Name 只會對應一個唯一的 Value。

在任何執行環境中，我們可以定義多個 Name，但通常來說，同樣的 Name 只會有一個，而且會有相對應的 Value。

舉例來說：
我的名字是 apeiros0 (也能自行替換成其他名字)，而我的名字也就只有一個，不太可能有多個，所以 Name/Value 配對就會是下方的樣子。
``` JavaScript
// Name = Value
MyName = 'apeiros0'
```

## 物件 (Object) ##
現在我們了解了 Name/Value 配對，但這跟物件有什麼關係呢?

> 其實在 JavaScript 中**物件是以 Name/Value 配對的方式所組合而成**，而 `{}` 能將不同 Name/Value 組合在一起。

舉例來說：
在 JavaScript 的物件是以下方的方式呈現：
``` JavaScript
// { Name: Value }
{ name: 'apeiros0' }
```
但這樣的方式是不是有點奇怪呢? 物件是以 Name/Value 配對的方式組合而成，`{ ... }` 這樣的方式單單只有 Value 而已，並沒有 Name，因此我們必須賦予 Name 給他。

所以物件會是下方這樣的形式：
``` JavaScript
// Name: Value
MyData: { name: 'apeiros0' }
```
當然，如果只有一個 Name/Value 那跟變數有什麼差別，因此我們能增加多個 Name/Value 到物件中。

``` JavaScript
MyData: {
  id: 123456789
  name: 'apeiros0',
  birthday: '2000/04/01'
}
```
**注意：物件中每個 Name/Value 必須用 `,` 做區隔。**

在上方的例子中，我們知道物件是 Name/Value 配對的組合，就像下方的形式。
``` JavaScript
Name: {
  Name: Value,
  Name: Value,
}
```
有沒有注意到什麼? 沒錯，物件中其實還能包含物件，就像下方這樣：
``` JavaScript
MyData: {
  id: 123456789
  name: 'apeiros0',
  birthday: '2000/04/01',
  // address 物件
  address: {
    city: 'XXX',
    street: 'YYY',
    zone: 'ZZZ'
  }
}
```
## 總結 ##
說了這麼多，但其實只要記住**物件是由 Name/Value 組合而成，而物件裡還能有其他的物件**就行了。
