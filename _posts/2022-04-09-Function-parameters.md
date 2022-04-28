---
title: 【JavaScript】新手小白聊Function參數
tags: 文章發布,JavaScript
description: 淺談Function是甚麼
date: 2022/04/09
---

>前情提要：此為初入新手村的超級菜鳥，淺談JavaScript裡的【Function】，僅供參考~ 敬請指教修正:bow::bow::bow:


## Function參數
函式結構：主要分為三個區域  
- 函式的名稱，如範例*hello*為function的命名  
- ==包圍在小括號()裡面的為function的參數==  
- 包圍在大括號或稱花括號{}內的，為此function的作用區 

```javascript
function hello() { //此hello為方程式的命名
  console.log(123);
  console.log(456);
  console.log(789);
}
hello(); //呼叫hello function
```


當我們呼叫一個函式的時候，可以透過「函式名稱」加上「小括號」的方式呼叫。 而小括號內的資料，就是「參數」
```javascript
const plus = function(numA, numB){
    return numA + numB
};
plus(1,2); //印出3
plus(5,8); //印出13
plus(); //印出undefined
```
這裡的`numA, numB`即為參數，`(1,2)`和`(5,8)`分別為引數(也可稱為參數)，引進`(1,2)`和`(5,8)`數字到參數`numA, numB`代入function進行運算並return出值。

---
## 參數的數量
參數的數量可以由==自己來定義==，function的命名很重要，參數的命名也是，方便在代入引數時可以清楚知道代入的內容作用為何，當然你想要取名為`aa,bb,x,y,z`也可以，只是你的隊友大概會想揍你而已 :laughing: 
```javascript
function person(name, num, job) {
  console.log(name);
  console.log(num);
  console.log(job);
}
person(); //都會印出undefined
person("kk") //印出"kk", undefined, undefined
person("kk",2) // 印出"kk", 2, unedfined
person("kk",2,"student") //印出"kk",2,"student"
person("kk",2,"student",8)//印出"kk",2,"student"
person("num","job","name") //印出 "num","job","name"
```

從上述範例來看，即使你沒有給它任何參數(引數)值就加以執行，也不會有錯誤，而是會回傳undefined。而多傳入的參數(引數)，在「大部分」的情況下是沒有意義的。 <Font color="#aaa">(大部分，表示還是可以透過其他方式拿到他)</Font>

:mag_right:為甚麼會回傳undefined?

在執行這個function的時候，它會先為我們的設定的參數(name, num, job)建立好記憶體位置，並且賦予它的值是undefined，
且**參數在給值的過程是由左至右的**。且函式參數的名稱是在定義函式就已經確定了，並「**不會受到引數名稱的影響**」。

---
## arguments
當函式被呼叫的時候，會產生一個arguments物件。而這個arguments物件的內容，其實就是我們呼叫函式所代入的「參數」(引數)。arguments會包含所有你放入function中的參數值。
如果當我們代入的參數(argument)多餘function定義的參數數量時，可以透過arguments這個物件取得參數。

```javascript
const plus = function (numA, numB) {
    console.log( arguments.length ); //5
    console.log(arguments);
  //印出Arguments(5) [1, 2, 3, 4, 5]
    return numA + numB;
  };
  
  // 因為有5個參數，會先印出5，然後回傳 1+2 的結果
  plus(1, 2, 3, 4, 5);
  console.log(plus(1, 2, 3, 4, 5)); //3
```
arguments回傳的值是斜體的[ ] ，看起來很像陣列(array-like)，arguments儲存了所有傳遞參數的值，而且還可以用`lenght`屬性，但它並不是真的陣列！所以arguments回傳的值並不具備所有陣列所具有的特徵。但可以透過arguments取得參數值。
***任何的傳統函式都有 arguments 參數（注意：箭頭函式沒有此參數）***
如果我們修改了 arguments 的值，原本的參數也會被修改：
```javascript
function change(a,b,c){
    console.log(a,b,c);//1 2 3
    arguments[0] *= 20;
    console.log(a,b,c);//20 2 3
}
change(1,2,3);
```

:bulb: arguments 物件的屬性：`callee`與`caller` 
- `callee`，指的是目前執行的函式。
- `caller`，目前已棄用，可用來取得call該function的來源物件。
```javascript
function method(a, b, c) {
     console.log(arguments.callee); //印出整個function內容
     console.log(arguments.caller); //印出undefined
     console.log(arguments.callee.caller); //印出function(a){method(1,3);}
     console.log('宣告參數長度--'+arguments.callee.length); //印出宣告參數長度--3
     console.log('實際參數長度--'+arguments.length); //印出實際參數長度--2
     console.log('callmethod的參數長度--' + arguments.callee.caller.length) //印出callmethod的參數長度--1
     console.log(a); //印出1
     console.log(b); //印出3
     console.log(c); //印出undfined
}
function callmethod(a)
{
     method(1,3);
}
callmethod();
```

在「嚴格模式」下不允許存取 `arguments.caller` 和 `arguments.callee` 這兩個屬性。


---
## 參數的預設值
如果已經定義參數，卻沒有傳入時，會出現undefined容易導致後續造成語法錯誤，使函式無法如預期執行的狀況。
ES6新增了參數預設值，可以預先定義參數預設值，避免undefined所造成的錯誤外，也增加函式運用的彈性。

```javascript
function person(name, num, job = "待業中"){
    console.log(name);
    console.log(num);
    console.log(job);
}
person("kk",1) //分別印出"kk",01,"待業中"
```


---
## 物件作為參數
當一個function有很多個參數的時候，可以將多個參數用一個「物件」包裝起來。
***舉例:要製作一個通訊錄***
```javascript
function addPerson(name, phone, email, gender, birth, address){
  //略  
};
addPerson("Sharon", "0912345678", "aaa@aa.com", 'female', "12/34", 'Taipei City');
```
因為參數(引數)在給值的過程中是由左至右依序代入，因此在輸入的時候順序不能錯、參數不能漏、只要中間少了一個參數(引數)，整個欄位就會say掰掰囉 :wave::wave:
這個時候就可以改用物件的方式取代這一堆參數(引數)
```javascript
let people = {
    name: "Sharon", //記得前面是冒號:結尾要逗號區隔,
    phone: "0912345678",
    email: "aaa@aa.com",
    gender: "female",
    birth: "12/34",
    address: "Taipei City",
}
addPerson(people) //物件people作為參數(引數)傳入addPerson
console.log(people);
//印出{name: 'Sharon', phone: '0912345678', email: 'aaa@aa.com', gender: 'female', birth: '12/34', …}
```
物件作為參數(引數)不僅呼叫函式變得更加簡便，而且由於物件的屬性不要求「順序」，所以就算中間忽略掉幾個屬性也沒問題，使得程式碼更容易閱讀，也易於維護。 往後就算要新增參數也不用擔心影響到過去的程式。

</br>
:bulb:延伸小教室
**javaScript的資料型態**
| 原始型別(Primitive type)  | 物件型別Object   |
| ------------------------- |:---------------- |
| 數字【number】            | 陣列【array】    |
| 文字(字串)【string】      | 函數【function】 |
| 真假值(布林值)【boolean】 | 物件【object】   |
| 未定義【undefined】       |                  |
| 符號【Symbol】       |                  |
| 空值                      |                  |

</br>

**為甚麼要不同的型別?** 
- 告訴電腦這是要幹嘛
- 功能不一樣

</br>




---
<spoiler> **參考資料**
- [重新認識 JavaScript: Day 17 函式裡的「參數」](https://ithelp.ithome.com.tw/articles/10192368)
- [呼叫函式時，到底有多少個參數 / 變數可供使用？](https://www.casper.tw/development/2020/09/22/js-function/)
- [重新認識 JavaScript: Day 10 函式 Functions 的基本概念](https://ithelp.ithome.com.tw/articles/10191549)
- [【筆記】談談JavaScript中函式的參數(parameter),arguments和展開運算子(spread)](https://pjchender.blogspot.com/2016/04/javascriptparameterargumentsspread.html)
- [JavaScript 的資料型別與資料結構](https://developer.mozilla.org/zh-TW/docs/Web/JavaScript/Data_structures#%E5%8E%9F%E5%A7%8B%E5%80%BC)
- [JavaScript系列文章-『Arguments、callee、caller的運用』](https://dotblogs.com.tw/h091237557/2014/08/27/146385)
</spolier>