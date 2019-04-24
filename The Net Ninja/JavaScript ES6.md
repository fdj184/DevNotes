# JavaScript ES6 Tutorials

Course Link: <https://www.youtube.com/playlist?list=PL4cUxeGkcC9gKfw25slm4CUDUcM_sXdml>

---

## What is ES6

- 全名是 ECMAScript 6，是 JavaScript 的第六版標準
- 基於 ES5 版後，新增了一些功能

## Constants

- 一種新的宣告方式
- 宣告時必須給予值，值未必是常數，也可以是陣列、dom 元素 .. 等等
- 在同一個 { ... } 中不能重新宣告、不能重新指派值
- 在 { ... } 外宣告則視為全域變數

example 1:

``` javascript
const pi = 10;
pi = 3.14; // error
```

example 2:

``` javascript
const pi = 10;
function calcArea(r) {
    // it's available to redeclare "pi" here.
    const pi = 3.14;
    console.log(r * r * pi);
}

// "pi" is still equal to 10 outside the { ... }
console.log(pi);
calcArea(10);

// output:
// 10
// 314
```

## The Let Keyword

- 一種新的宣告方式
- 在同一個 { ... } 中不能重新宣告，但可以指派新的值
- 在 { ... } 外宣告則視為全域變數
- let 和 var 的差別

  |                    |           var           |         let          |
  |:------------------:|:-----------------------:|:--------------------:|
  | 有效範圍(全域變數) |          全部           |    宣告之後的行數    |
  | 有效範圍(區域變數) | 最靠近的 function block |   最靠近的 { ... }   |
  |   for 迴圈的宣告   | 只能存取最後一個迴圈值  | 每次迴圈值都會被儲存 |

example 1:

``` javascript
console.log("globalVar: " + globalVar); // undefined
console.log("globalLet: " + globalLet); // error

var globalVar = "globalVar";
let globalLet = "globalLet";
```

example 2:

``` javascript
function scopeTest() {
    if (0 === 0) {
        var localVar = "localVar";
        let localLet = "localLet";
    }

    console.log(localVar);
    console.log(localLet); // error
}
scopeTest();
```

example 3:

``` html
<ul>
    <li>apple</li>
    <li>banana</li>
    <li>cherry</li>
</ul>
```

``` javascript
// 此寫法中，無論點擊 apple 或是 banana，都是寫 3
function loopVar() {
    const fruits = document.getElementsByTagName("li");
    for (var forVar = 0; forVar < 3; forVar++) {
        fruits[forVar].onclick = function() {
            console.log("forVar:" + forVar);
        };
    }
    console.log(forVar);
}
loopVar();

// 此寫法中，點擊 apple 會寫 1，點擊 banana 會寫 2
function loopLet() {
    const fruits = document.getElementsByTagName("li");
    for (let forLet = 0; forLet < 3; forLet++) {
        fruits[forLet].onclick = function() {
            console.log("forLet:" + forLet);
        };
    }
    // "forLet" 的有效範圍只在最近的 { ... }，在此例也就是 for 迴圈中
    console.log(forLet); // error
}
loopLet();
```

ref: [What's the difference between using “let” and “var”?](https://stackoverflow.com/questions/762011/whats-the-difference-between-using-let-and-var)

## Default Parameters (參數預設值)

``` javascript
function log(num = 10) {
    console.log(num);
}
log();
log(20);

// output:
// 10
// 20
```

## The Spread Operator

取出陣列中的個別元素

example 1:

``` javascript
const numArr1 = [1, 2, 3];
const numArr2 = [4, 5, 6];
numArr1.push(numArr2)
console.log(numArr1);

// output:
// [1, 2, 3, [4, 5, 6]]

numArr1 = [1, 2, 3];
numArr1.push(...numArr2)
console.log(numArr1);

// output:
// [1, 2, 3, 4, 5, 6]
```

example 2:

``` javascript
const numArr = [1, 2, 3];
function addNum(a, b ,c) {
    console.log(a + b + c);
}
addNum(...numArr);

// output:
// 6
```

## Template Strings

``` javascript
function log(name, age) {
    const str = `hello, my name is ${name} and my age is ${age}.`;
    console.log(str);
}
log("wayne", 20);

// output:
// hello, my name is wayne and my age is 20.
```

## Object Literal Enhancements

ES6 支援較簡潔的物件宣告寫法

``` javascript
// ES5 的物件宣告寫法
var name = "wayne";
var color = "red";
var person = {
    name: name,
    color: color,
    sayHi: function() {
        return this.name + " says Hi.";
    }
}
console.log(person.color);
console.log(person.sayHi());

// ES6 的物件宣告寫法
let name = "wayne";
let color = "red";
let person = {
    name, color,
    sayHi() {
        return this.name + " says Hi.";
    }
}
console.log(person.color);
console.log(person.sayHi());
```

## New String Methods

|            Methods             |          Description           |
|:------------------------------:|:------------------------------:|
|        repeat(_times_)         |            重複字串            |
| startWith(_value_, _position_) | 檢查字串(在特定位置的)起始文字 |
|  endWith(_value_, _position_)  | 檢查字串(在特定位置的)後推文字 |
|       includes(_value_)        |     檢查字串中是否包含文字     |

``` javascript
const str = "good ";
console.log(str.repeat(5));

// output:
// good good good good good
```

``` javascript
const str = "goodbye guys";
console.log(str.startWith("good"));
console.log(str.startWith("bye", 4));
console.log(str.includes("guy"));

// output:
// true
// true
// true
```

## Arrow Functions (箭頭函式)

宣告函式：

``` javascript
// ES5 寫法
var greeting = function(name) {
    console.log("Hi" + name);
};

// ES6 箭頭函式寫法
var greeting = (name) => {
    console.log("Hi" + name);
};

// 如果函式只有一個參數，連小括號也可以省略
var greeting = name => {
    console.log("Hi" + name);
};
```

宣告物件中的方法：

``` javascript
// ES5 寫法
var person = {
    name: "Wayne",
    jump: function(x) {
        var _this = this;
        window.setInterval(function() {
            if (x > 0) {
                // 這裡若用 this 會指向 function()，所以取不到 name
                // console.log(this.name + " jumped");
                console.log(_this.name + " jumped");
                x--;
            }
        }, 1000);
    }
};
person.jump(5);

// ES6 寫法
const person = {
    name: "Wayne",
    // ES6 較簡潔的物件宣告方式
    jump(x) {
        // 改採箭頭函式
        window.setInterval(() => {
            if (x > 0) {
                // 若用箭頭函式，this 會指向物件本身(即person)
                console.log(this.name + " jumped");
                x--;
            }
        }, 1000);
    }
};
person.jump(5);
```

## Sets

- 新的資料型態
- 不會有重複的值，重複的會自動被忽略

|      Methods      |   Description    |
|:-----------------:|:----------------:|
|       size        |  回傳 set 長度   |
|  add(_element_)   |     添加成員     |
| delete(_element_) |     刪除成員     |
|      clear()      |  |刪除所有成員   |
|  has(_element_)   | 檢查是否含有成員 |

``` javascript
const fruits = new Set();
fruits.add("apple").add("banana").add("apple");
console.log(fruits);

// output:
// {"apple", "banana"}
```

## Generator

- 在「function」關鍵字後面加上「*」
- 類似 function 但是每次僅執行一個步驟 (yield)

example 1:

``` javascript
function* gen() {
    yield "apple";
    yield "banana";
    return "all done";
}

// 首次呼叫 generator 不會執行
const myGen = gen();
// 每次使用 next()，就會執行下一個 yield 中的內容
console.log(myGen.next());
console.log(myGen.next());
console.log(myGen.next());
console.log(myGen.next());

// output:
// Object {value: "apple", done: false}
// Object {value: "banana", done: false}
// Object {value: "all done", done: true}
// Object {value: undefined, done: true}
```

example 2:

``` javascript
// 傳遞參數至 generator 中
function* gen() {
    const x = yield "apple";
    const y = yield "banana";
    return x + y;
}

const myGen = gen();
// 第一次使用 next() 前，還沒辦法傳 x 進去
console.log(myGen.next());
// x 已經產生，故可以傳遞第一個參數
console.log(myGen.next(10));
// y 已經產生，故可以傳遞第二個參數
console.log(myGen.next(5));

// output:
// Object {value: "apple", done: false}
// Object {value: "banana", done: false}
// Object {value: 15, done: true}
```
