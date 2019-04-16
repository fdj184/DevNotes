# JavaScript Tutorials For Beginners

Course Link: <https://www.youtube.com/playlist?list=PL4cUxeGkcC9i9Ae2D9Ee1RvylH38dKuET>

---

## Where to put your JS

因為 Html 是由上往下載入，建議把 js 寫在 \<body> 元素的最後面，這樣至少先看得到畫面(Html元素)。

``` html
<head> ... </head>
<body>
    ...
    <script>
        alert("Hello World!");
    </script>
</body>
```

如果 js 內容很多的話，則應改成引用獨立的 .js 檔

``` html
<head> ... </head>
<body>
    ...
    <script src="hello.js"></script>
</body>
```

## Basic Syntax & Rules (基本規則)

- JS 的大小寫有差異
- 敘述要用「;」結尾
- 空格(whitespace)和換行(line break)不影響敘述
- 單行註解用「//」，多行註解用「/\*」和「\*/」
- JS 由上往下執行，所以寫在下面的語法有可能會覆蓋掉上面的

## Variables (變數)

- JS 是弱型別語言，意即宣告變數時，不需宣告其型別
- 變數命名不得以數字開頭

``` javascript
var myVar1 = 10;
var myVar2 = "Hello";
```

## Writing to the document & console

Debug 時可用來檢查變數當下的值

``` javascript
var myVar = 10;
// Writing to the document
document.write(myVar);
// Writing to the console
console.log(myVar);
```

## Variable Types & Methods (變數型別和方法)

### Booleans (布林)

``` javascript
var myVar = true;
console.log(myVar);
console.log(Boolean(5 > 7));

// output:
// true
// false
```

### Numbers (數值)

``` javascript
var a = 2;
var b = 5;
console.log(a + b);
console.log(typeof (a + b));

// output:
// 7
// number
```

### NaN (Not a Number)

``` javascript
// 遇到數學運算子時，js 會把純數字字串當成數值下去計算
var a = "2";
var b = 5;
console.log(a * b);

a = "apple";
b = 5;
console.log(a * b);

// output:
// 10
// NaN
```

### Number Methods (常見的數值方法)

|   Methods    |        Description         |
|:------------:|:--------------------------:|
|  toString()  |         轉換成字串         |
|   isNaN()    | 檢查是否為數值或純數字字串 |
|   Number()   |         轉換成數值         |
|  parseInt()  |       轉換成整數數值       |
| parseFloat() |      轉換成浮點數數值      |

``` javascript
// toString()
var a = 5;
console.log(typeof (a));
console.log(a.toString());
console.log(typeof (a.toString()));

/* output:
number
5
string */
```

``` javascript
// isNaN()
var a = "apple";
console.log(isNaN(a));

// output:
// true
```

``` javascript
// Number()
console.log(Number(true));
console.log(Number("  10"));
console.log(Number("John"));

/* output:
1
10
NaN */


// parseInt()
console.log(parseInt("10.33"));
console.log(parseInt("10 years")); // Only the first number is returned.
console.log(parseInt("years 10"));

/* output:
10
10
NaN */


// parseFloat()
console.log(parseFloat("10.33"));
console.log(parseFloat("10 years")); // Only the first number is returned.
console.log(parseFloat("years 10"));

/* output:
10.33
10
NaN */
```

其它 Number 方法，自行參考 [w3schools](https://www.w3schools.com/js/js_number_methods.asp)

### Math Methods

|   Methods    |   Description    |
|:------------:|:----------------:|
| Math.round() |  四捨五入成整數  |
| Math.ceil()  | 無條件進位成整數 |
| Math.floor() | 無條件捨去成整數 |
|  Math.max()  |  取集合中最大值  |
|  Math.min()  |  取集合中最小值  |

``` javascript
console.log(Math.round(10.53));
console.log(Math.ceil(10.53));
console.log(Math.floor(10.53));
console.log(Math.max(1, 5, 7));
console.log(Math.min(1, 5, 7));

/* output:
11
11
10
7
1 */
```

其它 Math 方法，自行參考 [w3schools](https://www.w3schools.com/js/js_math.asp)

### Strings (字串)

- 可以用單引號(')或雙引號(")包起字串
- 跳脫字元為「\」

``` javascript
var a = "Hel\"lo";
var b = "World";
console.log(a + b);
console.log(typeof (a + b));

// output:
// Hel"loWorld
// String
```

### String Methods (常見的字串方法)

| Methods      | Description      |
| :---: | :---: |
| length      | 字串長度      |
| toUpperCase()      | 轉大寫字母      |
| toLowerCase()      | 轉小寫字母      |
| trim()      | 移除字串前後的空格      |
| replace(xxx, ooo)      | 將字串中，第一個 xxx 取代為 ooo      |
| replace(/xxx/g, ooo)      | 將字串中，所有 xxx 取代為 ooo      |
| indexOf()      | 第一個出現在字串中的目標之位置索引      |
| slice(start, end)      | 擷取部分字串      |
| split()      | 將字串轉換為陣列      |

``` javascript
var a = " Hello world1 & world2 ";
console.log(a.length);
console.log(a.toUpperCase());
console.log(a.toLowerCase());
console.log(a.trim());
console.log(a.replace("world", "friend"));
console.log(a.replace(/world/g, "friend"));

/* output:
23
" HELLO WORLD1 & WORLD2 "
" hello world1 & world2 "
"Hello world1 & world2"
" Hello friend1 & world2 "
" Hello friend1 & friend2 " */
```

``` javascript
var a = "Hello beautiful world";
console.log(a.indexOf("beautiful"));
console.log(a.indexOf("good"));
console.log(a.slice(2, 8));
console.log(a.slice(2));
console.log(a.slice(-5, -1));
var b = "apple, banana, cherry";
console.log(b.split(","));

/* output:
6
-1
llo be
llo beautiful world
worl
["apple", " banana", " cherry"] */
```

其它 String 方法，自行參考 [w3schools](https://www.w3schools.com/js/js_string_methods.asp)

## Basic Mathematical Operators (基本數學運算子)

| Operators | Description |
|:---------:|:-----------:|
|     =     |    指派     |
|     +     |     加      |
|     -     |     減      |
|     *     |     乘      |
|     /     |     除      |

``` javascript
var myVar = 10;
myVar = myVar + 5;
myVar = myVar - 3;
myVar = myVar * 2;
myVar = myVar / 3;
```

## Comparison Operators (比較運算子)

| Operators |        Description        |
|:---------:|:-------------------------:|
|     >     |           大於            |
|    >=     |         大於等於          |
|     <     |           小於            |
|    <=     |         小於等於          |
|    ==     |      等於 (只檢查值)      |
|    ===    |  等於 (同時檢查值和型別)  |
|    !=     |     不等於 (只檢查值)     |
|    !==    | 不等於 (同時檢查值和型別) |

``` javascript
var myVar = 5;
console.log(myVar > 7); // false
console.log(myVar == 5); // true
console.log(myVar == "5"); // true
console.log(myVar === 5); // true
console.log(myVar === "5"); // false
```

## Logical Operators (邏輯運算子)

| Operators | Description |
|:---------:|:-----------:|
|    &&     |     AND     |
|    \|\|   |      OR     |
|     !     |     NOT     |

``` javascript
var myAge = 25;
if (myAge >= 18 && myAge <= 30) {
    console.log("you can come to the party.");
}
```

## Control Flow & Logic (流程控制)

### If-else Statements

``` javascript
var youLikeMeat = true;
if (youLikeMeat) {
    console.log("you like meat.");
}
else {
    console.log("you don't like meat.");
}

// output:
// you like meat.
```

※ 判斷式如果有「等於」的概念，因為「=」代表指派，所以應該用「==」或是「===」

``` javascript
if (5 == 7) {
    ...
}
```

### If-else if-else Statements

``` javascript
var myAge = 29;
if (myAge > 30) {
    console.log("you are over 30.");
}
else if (myAge > 20) {
    console.log("you are over 20.");
}
else if (myAge > 10) {
    console.log("you are over 10.");
}
else {
    console.log("you are not over 10.");
}

// output:
// you are over 20.
```

### While Loops

``` javascript
var myAge = 5;
while (myAge < 10) {
    console.log(myAge);
    myAge++;
}
console.log("you are over 10.");

/* output:
5
6
7
8
9
you are over 10.*/
```

### For Loops

※ 如果不在 for 迴圈用 var 或 let 開頭來宣告變數，則該變數會自動被宣告成全域變數，導致該變數生命週期過長

``` javascript
for (var myAge = 5; myAge < 10; myAge++) {
    console.log(myAge);
}
console.log("you are over 10.");

/* output:
5
6
7
8
9
you are over 10.*/
```

### Break & Continue

| Keyword  |          Description           |
|:--------:|:------------------------------:|
|  Break   |          中斷整個迴圈          |
| Continue | 略過本次迴圈，直接進入下次迴圈 |

``` javascript
for (var myAge = 5; myAge < 10; myAge++) {
    if (myAge === 7) {
        continue;
    }
    else if (myAge === 9) {
        break;
    }
    console.log(myAge);
}
console.log("you are over 10.");

/* output:
5
6
8
you are over 10.*/
```

## Functions (函式)

``` javascript
// Declare a function
function getAverage(a, b) {
    var avg = (a + b) / 2;
    return avg;
}

// Call by the function name
var myResult = getAverage(5, 15);
console.log(myResult);

// output:
// 10
```

## Variable Scope (變數有效範圍)

|                  | Description |     Scope      |
|:----------------:|:-----------:|:--------------:|
| global variables |  全域變數   |    整個網頁    |
| local variables  |  區域變數   | 區塊之間 {...} |

``` javascript
// global variable
var myVar1 = 1;

function myFunc() {
    // local variable
    var myVar2 = 2;
    console.log(myVar1);
    console.log(myVar2);
}

myFunc();
console.log(myVar1);
console.log(myVar2);

/* output:
1
2
1
Error: myVar2 is not defined */
```

