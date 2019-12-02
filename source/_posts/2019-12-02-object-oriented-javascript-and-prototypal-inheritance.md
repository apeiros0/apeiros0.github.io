---
title: 物件導向和原型繼承 (Object Oriented JavaScript And Prototypal Inheritance)
categories: JavaScript
tags:
  - 克服 JavaScript 奇怪部分筆記
comments: true
date: 2019-12-02
---

接下來的主題我們要來了解另一個 JavaScript 的重要觀念，它能增進我們 JavaScript 的開發能力，但它也是造成許多人混亂的主題，因為 JavaScript 在這部分和其他的程式語言有很大的不同。我們就來討論 JavaScript 的物件導向和原型繼承吧 !

當我們談到 JavaScript 的物件導向時，我們主要是在關注物件的建立，因為這部份很容易搞不清楚，所以我們要先從繼承講起。

## 繼承 (Inheritance)

表示**一個物件取用另一個物件的屬性和方法**，在各個語言中，繼承的概念是不太相同的，我們只需了解基本的概念即可，也就是一個物件取用另一個物件的屬性和方法。假設我們有兩個物件，第一個物件繼承第二個物件，表示第一個物件能取用第二個物件的屬性和方法。

那當我們談到古典繼承和原型繼承是什麼意思呢 ?

## 古典繼承 (Classical Inheritance)

這是最著名和最受歡迎的東西，C# 和 Java 都有，可以分享物件的屬性和方法，這就是古典 (Classical)，表示很久以前這麼做，但認為只有這麼一種方法，認為它沒有缺點是錯的。

- 古典繼承很口語化 (Verbose)
當我們建立很多大型物件，他們能互相繼承別人的屬性和方法。我們最終會得到很大的集合、樹狀的物件相互影響，但這會變得很難搞清楚它們會如何互相影響，即便這是個不錯的方法，也會很難弄清楚它們會如何互相影響，就像我們蓋了一件錯綜複雜的房子，當我們換燈泡時，馬桶會突然沖水一樣，這就是有時使用古典繼承的感覺。

- 古典繼承還有許多不同的關鍵字，我們需要學習這些關鍵字是什麼意思，並使用它們。

  - `friend`
  - `protected`
  - `public`
  - `private`
  - `interface`
  - ...

## 原型繼承 (Prototypal Inheritance)

- 簡單 (Simple)

- 有彈性 (Flexible)

- 可擴充性 (Extensible)

- 簡單易懂 (Easy to Understand)

當我們談到 JavaScript 的原型繼承時，它和其他程式語言不同，我們需要了解它，一旦我們了解，就會知道它在共享物件的屬性和方法有多麼強大，後面的章節就讓我們來看看 JavaScript 怎麼用原型繼承做到 !