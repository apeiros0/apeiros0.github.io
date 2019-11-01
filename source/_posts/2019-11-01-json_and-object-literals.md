---
title: JSON 和物件實體語法 (JSON And Object Literals)
categories: JavaScript
tags:
  - 克服 JavaScript 奇怪部分筆記
comments: true
date: 2019-11-01
---

今天我們花點時間來談談物件實體語法常見的錯誤，以及使用 JSON 時所遇到的問題。

我們就先來了解什麼是 JSON 吧 !

## JSON (JavaScript Object Notation)

JSON 是被物件實體語法 (object literals syntax) 所啟發，所以才稱為 JavaScript Object Notation，它看起來與物件實體語法很像，所以常會有人搞混，導致遇到一些錯誤，先讓我們回顧一下物件要怎麼建立。

我們可以建立一個物件的實體，有許多不同的屬性，這在 JavaScript 是有效的，可以被輸出，那 JSON 是怎麼演化而來的呢 ?

```JavaScript
// Object
var objectLiterals = {
  firstName: 'Mary',
  isAProgrammer: true
}

console.log(objectLiterals);
```

### JSON 由來

在幾年前，網路的資料傳輸格式有很多種，而曾有一段時間是透過 XML 格式來傳送，就像下方這樣，資料的兩旁有標籤，而伺服器會的到這些資訊，並解析。

```XML
<object>
  <firstName>Mary</firstName>
  <isAProgrammer>true</isAProgrammer>
</object>
```

但問題是，在處理下載的時候，下載速度會受到速度、資料量、頻寬的影響，那些標籤，會讓你的資料量變得龐大，我們只是傳送一小部分資料，就要寄送相同的屬性名稱兩次，如果處理的資料量一大，就會影響到下載的頻寬。

之後 JSON 出現，大家都認為它是一種不錯的資料傳輸方式，就像下方這樣，所以現在我們幾乎都是使用 JSON 格式來傳送資料。

```JavaScript
{
  firstName: "Mary",
  isAProgrammer: true,
}
```
### 屬性必須在 `""` 中
JSON 只是一串資料，與物件實體語法類似，除了有一小部分的差異外，舉例來說，屬性必須包在 `""` 裡，這對物件實體語法也是有效的。

在物件實體語法中，屬性**可以**被包覆在 `""` 裡，但在 JSON 中，它們**必須**包覆在 `""` 當中，所以如果你有解析 JSON 的程式或函式庫，像是 PHP 或 ASP.NET，當它們解析 JSON 時，會預期屬性會被包覆在 `""` 當中。

> 補充：JSON 是物件實體語法的子集合，只要在 JSON 中有效的，在物件實體語法中也是有效的，但不是所有的物件實體語法在 JSON 中都是有效的，這也是大家常搞混的部分。

```JSON
{
  "firstName": "Mary",
  "isAProgrammer": true
}
```
> **注意：JSON 的最後一個屬性不能有 `,`**

### 透過 JavaScript 轉換
JSON 有比較嚴謹的規則，它不是 JavaScript 的一部份，但它可以簡單地讓 JavaScript 解析，JavaScript 有內建的功能可以轉換 JSON，像是：

使用 `JOSN.stringify(Object)` 將物件轉換成 JSON 字串。
```JavaScript
// Object
var objectLiterals = {
  firstName: 'Mary',
  isAProgrammer: true
}

console.log(JSON.stringify(objectLiterals));
```

如果有 JSON 的字串，可以使用 `JSON.parse(String)` 轉換成 JavaSvript 的物件。

```JavaScript
var jsonValue = JSON.parse('{ "firstName": "Mary", "isAProgrammer": true }');

console.log(jsonValue);
```

## 總結
物件和 JSON 是不同的東西，JavaScript 有讓你轉換兩者的功能，JSON 對於使用 `""` 比較嚴格，所以在使用 JSON 時要注意在屬性加上 `""`，否則會出現錯誤。