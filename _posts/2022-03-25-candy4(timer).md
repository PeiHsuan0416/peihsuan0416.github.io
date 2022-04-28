---
title: 新手小白打怪物【JavaScript】
tags: 文章發布,JavaScript,糖果練習題
description: 上課練習~好好打怪吃糖果
date: 2022/03/22
---


>前情提要：此為初入新手村的超級菜鳥，分享課後練習:candy:糖果題，僅供參考~ 敬請指教修正  
:bow::bow::bow:


## :candy:時間秒數轉換

- 程式語言：JavaScript
- 題目：完成函數的內容，把傳進去的秒數變成平常人類看的懂的時間格式
```javascript
console.log(humanReadableTimer(0))      // 印出 00:00:00
console.log(humanReadableTimer(59))     // 印出 00:00:59
console.log(humanReadableTimer(60))     // 印出 00:01:00
console.log(humanReadableTimer(90))     // 印出 00:01:30
console.log(humanReadableTimer(3599))   // 印出 00:59:59
console.log(humanReadableTimer(3600))   // 印出 01:00:00
console.log(humanReadableTimer(45296))  // 印出 12:34:56
console.log(humanReadableTimer(86399))  // 印出 23:59:59
console.log(humanReadableTimer(86400))  // 印出 24:00:00
console.log(humanReadableTimer(359999)) // 印出 99:59:59
```
解題的方式有很多~當初看到這題的時候，只想到國小的運算方式，但要怎麼將它寫成程式呢?
那我們就先來看看如果是國小時候的我會怎麼運算吧~

我們都知道一分鐘是60秒，一小時是60分鐘，也就等於一小時會有3600秒。如果今天是90秒，我們可以用90-60=30，表示有一個60秒以及30秒；或是用90/60=1...30，表示有一個1分鐘和剩餘的30秒。簡單的秒數會用減法來計算，那超多秒數呢?例如3487秒會要減幾次60秒?(<font size="1px" color="#aaa">感覺都要眼花了</font>)

透過除法的方式可以得到==整數==和==餘數==，因此這次利用Math.floor()的方式來進行這次的解題

## 甚麼是`Math.floor()`

參考MDN(Mozilla Developer Network)解釋：<font color="#f00">函式匯回傳小於等於所給數字的最大整數</font>

???看完這句有沒有跟我一開始看到的一樣，明明大都是中文字，怎麼放在一起就都看不懂了~小於又等於的，又最大，是不是感覺非常矛盾:cold_sweat:別怕，來看看範例說明：
```javascript
console.log(Math.floor(5.95));
// expected output: 5

console.log(Math.floor(5.05));
// expected output: 5

console.log(Math.floor(5));
// expected output: 5

console.log(Math.floor(-5.05));
// expected output: -6
```

也就是說，當代入的值是正數的時候，無論他後方的小數點有多少，他都會完全忽視，只取==小於等於==代入值的正整數。也就是我們常說的無條件捨去~
但如果代入的值是負數(值)的時候，他就會取比代入值小的負整數裡==最大的==負整數。以範例來看，當代入值為-5.05的時候，小於-5的負整數有很多(-6,-7,-8....)其中最大的是-6。也就應證了他所解釋的小於、最大、(負)整數。

### :point_right:`math.floor(x)語法`
- X 是一種數字型態
- 回傳值：小於等於所給的數字(x)最大的(正負)整數


運用此函式，我們可以寫一個function，讓題目的秒數，代入此function內，得到一個整數的回傳值，也就像是我們所說的**取整數**一樣。且透過題目，我們可以知道本次將此function的命名為==humanReadableTimer==()，小括號內則是我們要代入的秒數，我們可以將參數就直接設定為==seconds==，清楚的讓別人知道，我們要代入的參數是秒數(<font size="1px" color="#aaa">當然如果你想寫成abc，或是取別的名字也是可以的拉~</font>)

我的計算方式是先從小時開始，看看所代入的seconds，除以3600秒之後，得到它的小時數(hr)。  
再將原本代入的seconds減去已知小時(hr)的秒數，所得到到數再除以60，就會得到分鐘數(min)。
最後將原本代入的seconds-已知(hr)的秒數-已知(min)的秒數，即為剩餘的秒數。

而顯示方式要是`00:00:00`如果時分秒不足10的時候，前方要補0上去。本次補0的方式選用.padStart()函式


## 甚麼是`padStart()`

參考MDN(Mozilla Developer Network)解釋：<font color="#f00">會將你給他用於填充的==字串==，插入到目標字串的起頭(左側)，值到目標字串到達指定的長度。</font>

也就是說，你告訴它你最終的字串長度要多少，並且給它你所指定它要補的字串，它將不足的地方用該字串補上。  
:bell:這裡非常重要是要用==字串==，在程式裡，你看到的數字可能不是數字，它是字串(把它想成文字)，簡單舉例：數字123+字串123會得到123123。

### :point_right:String.prototype.padStart()語法
`str.padStart(targetLength [, padString])`

- str 字串
- targetLength 你希望最終字串的長度(如果小於原本字串的長度，它會直接返回原本字串給你(<font size="1px" color="#aaa">你都要它補足不足~卻又給他足夠的，阿內乾丟</font>
- padString 用來填充的字串,如果填充的字串太長，會從左側開始擷取所需

範例：
```javascript
'abc'.padStart(10);         // "       abc"
'abc'.padStart(10, "foo");  // "foofoofabc"
'abc'.padStart(6,"123465"); // "123abc"
```


## 解題：

透過Math.floor(x)取整數的方式計算，再用.toString()的方式轉為字串，最後才可以用.padStart()的方式組裝增加長度。
如果不用.padStart()的方式也可以設假設if，`if (hour < 10) {hour = "0" + hour;}`的方式補足

```javascript
function humanReadableTimer(seconds) {
  let hr = (Math.floor(seconds / 3600)).toString();  
    //這裡計算會無條件捨去餘數，所得到的整數即為小時
  let min = (Math.floor((seconds - (hr * 3600)) / 60)).toString();  
    //將(帶入的秒數-小時的秒數)除60(一分鐘60秒)取整數得到幾分
  let sec = (Math.floor(seconds - (hr * 3600) - (min * 60))).toString(); 
    //利用.toString()將數字轉換成字串
  return hr.padStart(2,0) + ":" + min.padStart(2,0) + ":" + sec.padStart(2,0) 
    //利用.padStart(字串長度,想補的字串)將0補再前面
  }
```

以上~得到本次糖果拉:candy::candy::candy:



---

##### 延伸說明
floor()是 Math的靜態函式, 所以不需實體化Math物件, 只要直接呼叫 Math.floor()就能使用。

---

## :mag_right:延伸比較Math.floor()和Math.trunc()的差別

剛剛看過了Math.floor()的使用方式，那我們現在來看看Math.trunc()的使用方式吧

- **參考MND說明**
    The Math.trunc() function returns the integer part of a number by removing any fractional digits.
    函數通過刪除任何小數位返回數字的整數部分。

這就是我們直覺上的取整數呀!!!:joy::joy::joy:看看它的範例：
```javascript
console.log(Math.trunc(13.37));
// expected output: 13

console.log(Math.trunc(42.84));
// expected output: 42

console.log(Math.trunc(0.123));
// expected output: 0

console.log(Math.trunc(-0.123));
// expected output: -0
```
透過範例得知，無論它是正數還是負數，就直接把小數點通通刪除掉就對了:joy:

:::success
:question: 那為甚麼我們這次不選用它呢?
如果單以這次的時間題來看，時間都是正數，所以在選用的時候會沒有差別，但如果今天遇到負數的計算使用時，它就會有很大的差異。
假設所給的值為(-2.45)再Math.trunc()裡所得到的會是(-2)可是再Math.floor()的結果裡面會得到(-3)這樣就差了-1耶~
所以囉~ 還是要謹慎確認使用的方式唷!!
:::





---
:::spoiler **參考資料**
- [MDN-math.floor()取整數](https://developer.mozilla.org/zh-TW/docs/Web/JavaScript/Reference/Global_Objects/Math/floor)
- [MDN-math.trunc()取整數](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Math/trunc)
- [MDN-str.padStart()延伸字串](https://developer.mozilla.org/zh-TW/docs/Web/JavaScript/Reference/Global_Objects/String/padStart)
:::