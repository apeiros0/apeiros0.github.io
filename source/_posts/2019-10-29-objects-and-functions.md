---
title: 物件和 function (Objects And Functions)
categories: JavaScript
tags:
  - 克服 JavaScript 奇怪部分筆記
comments: true
date: 2019-10-29
---

今天我們要進入到全新的主題 — 物件和 function，雖然在其他程式語言它們是兩個不同的東西，但在 JavaScript 中它們可是有著非比尋常的關係，而在很多情況中，它們幾乎是相同的，所以就讓我們來談談物件和 function 吧 !

## 物件和 `.` (Objects And The Dot)

在這之前，我們先來了解物件和 `.` 的關係，我們都知道物件是由 Name/Value 所組合而成，而 Value 也可以是另一個 Name/Value 的組合，那物件是如何存在於記憶體中的呢 ? 物件是 Name/Value 組合，這個 Value 有哪些呢 ?

```JavaScript
var obj = {
  name: {
    name: value
  }
}
```

### 物件的 Value 有哪些

我們常聽到**物件有屬性 (property) 和方法 (methods)**，其中有：

- **純值屬性 (Primitive property)**：boolean, number 和 string...等。
- **物件屬性 (Object property)**：連結另一個物件。
- **方法 (Methods)**：是指 function，當 function 和物件產生連結，我們稱之為「方法」。

```
                    ---------------------
              ----> |  Object property  |
              |     ---------------------
              |
------------  |     ------------------------
|  Object  | -----> |  Primitive property  |
------------  |     ------------------------
              |
              |     ---------------------
              ----> |  Function method  |
                    ---------------------
```

### 物件是如何存在於記憶體中

在記憶體中，核心的物件會有記憶體位址，它可以參照到其他位址的屬性和方法，並找到這些屬性和方法，如果覺得很抽象的話，我們可以想像物件在記憶體中有個位置，而它可以找到和它相關的屬性和方法的記憶體位置。

```
                              -------------------------------
                        ----> |  Object property  |  0x002  |
                        |     -------------------------------
                        |
----------------------  |     ----------------------------------
|  Object  |  0x001  | -----> |  Primitive property  |  0x003  |
----------------------  |     ----------------------------------
                        |
                        |     -------------------------------
                        ----> |  Function method  |  0x004  |
                              -------------------------------
```

### 如何找到物件的屬性和方法位址

我們宣告一個物件，它會存在於記憶體中，接著我們來新增一些屬性和方法，我們可透過 `[]` 運算子，放入屬性名稱，便能將屬性存在記憶體中，但現在還不存在，因為我們還沒賦予值，賦予值之後就會在記憶體中建立屬性與對應的值，物件就能參照到該屬性的記憶體位址。

> **補充：`[]` 運算子是取用計算成員 (Computed Member Access)，除了新增屬性和方法外，也能找出物件的屬性和方法。**

```JavaScript
var obj = new Object();

// 在物件中新增 Name/Value 的起手式 (這樣做才會新增記憶體給屬性/方法)
obj["propertyName"] = "value";

obj["propertyName"]; // Bad，這樣是不會新增到記憶體中的
```

讓我們舉一個例子，我們建立一個 person 物件，這樣記憶體便會有 person 的物件存在，接著透過 `[]` 運算子加入 `firstName` 屬性，並賦予 `apeiros` 的值，這樣就會在記憶體中增加 `firstName` 的屬性，並與 person 物件連結，也能加入其他屬性玩玩看。

```JavaScript
var person = new Object();

person["firstName"] = "apeiros"; // 這樣就能在記憶體中增加屬性
person["lastName"] = "0";
```

> 補充：這裡使用 `new Object()` 宣告是為了更好理解，我們還有其他更好的方法建立物件。

#### `[]` 運算子優點

`[]` 運算子好用的地方在於，我們可以透過變數來取用屬性，像是下方這樣，它會把物件當作一個參數，字串 (變數) 當作另一個參數，然後找到物件裡的屬性，回傳對應的值。

```JavaScript
var person = new Object();

person["firstName"] = "apeiros";
person["lastName"] = "0";

var firstNameProperty = "firstName";
console.log(person[firstNameProperty]); // apeiros
```

#### `.` 運算子

除了透過 `[]` 運算子來新增屬性和方法外，也有更簡單易懂的方式取用屬性和方法，有時我們會在函式庫 (或框架) 中常看到，那就是 `.` 運算子，讓我們來修改上面的範例。

```JavaScript
var person = new Object();

person.firstName = "apeiros0";
person.lastName = "apeiros0";

console.log(person.firstName); // . 會從 person 參照，找到 firstName 的記憶體位址

person."firstName" = "apeiros0"; // 這樣取用是錯誤的
```

除了設定純值的屬性外，我們也能透過 `.` 運算子來不斷新增屬性給子物件

> 補充：`.` 運算子的相依性為從左到右。

```JavaScript
var person = new Object();

person["firstName"] = "apeiros";
person["lastName"] = "0";

person.address = new Object();
person.address.street = '111 Main St.';
person.address.city = 'New York';
person.address.state = 'NY';

// . 取得屬性的方式
console.log(person.address);
console.log(person.address.state);

// [] 取得屬性的方式
console.log(person["address"]);
console.log(person["address"]["state"]);
```

**運算子相依性運作方式**
```
person.address.street
-> person.address // JavaScript 看到 person 物件，找出在記憶體中的 address 物件
-> person.address.street // 找到 address 物件後，接著尋找 street 屬性 (沒有找到會自動建立 street 屬性，並賦予值給它)
```

## 總結

物件可以不斷包含其他物件，而物件本身、屬性和方法都會在記憶體中，我們可透過 `.` 和 `[]` 這兩種方式新增或取出物件屬性對應的值，雖然方式不同，但他們做的都是相同的事，他們都只是 function，只是一個取出資訊的方式。

> 補充：用 `.` 運算子來取用屬性或方法是比較好的方式，因為它簡單易懂、方便、容易除錯，除非想用動態字串的方式取用屬性 (`[]` 運算子)，否則使用 `.` 運算子會比較好。
