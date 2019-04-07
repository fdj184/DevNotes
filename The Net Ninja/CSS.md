# CSS Tutorials For Beginners

Course Link: <https://www.youtube.com/playlist?list=PL4cUxeGkcC9gQeDH6xYhmO-db2mhoTSrT>

---

## Syntax (語法)

|          |                 inline                 |       embedded       |  external style sheet  |
|:--------:|:--------------------------------------:|:--------------------:|:----------------------:|
|   寫法   |             寫在每個元素上             |  集中寫在每頁特定處  |    集中寫在外部檔案    |
|   範例   | \<p **style="color: red;"**>Hello\</p> |                      |                        |
| 使用時機 |        特別針對某個元素時可使用        | 特別針對某頁時可使用 | 多數情況建議使用此寫法 |

## Comment (註解)

`/*` color: red; `*/`

## Targeting (指定目標)

### by tags

``` css
div {
    color: red;
}
```

``` html
<div id="mydiv">Hello World</div>
```

### by IDs

``` css
#mydiv {
    color: red;
}
```

``` html
<div id="mydiv">Hello World</div>
```

### by Classes

``` css
.myclass {
    color: red;
}
```

``` html
<div class="myclass">Hello World</div>
```

## Targeting Multiple Elements (指定多個目標)

``` css
/* 選擇所有 h1 和 h2 元素 */
h1, h2 {
    color: red;
}
```

## Descendant Selectors (後代選擇器)

``` css
/* 選擇 div 後代的所有 p 元素 */
div p {
    color: red;
}
```

``` html
<!-- 「Hello」和「World」都會被影響 -->
<div>
    <span>
        <p>Hello</p>
    </span>
    <p>World</p>
</div>
```

## Child Selectors (子代選擇器)

``` css
/* 選擇 div 直接子代的 p 元素 */
div > p {
    color: red;
}
```

``` html
<!-- 僅「World」會被影響 -->
<div>
    <span>
        <p>Hello</p>
    </span>
    <p>World</p>
</div>
```

## Adjacent Selectors (相鄰兄弟選擇器)

``` css
/* 選擇緊接在 span 之後的 p 元素 */
span + p {
    color: red;
}
```

``` html
<!-- 僅「World」會被影響 -->
<div>
    <span>
        <p>Hello</p>
    </span>
    <p>World</p>
</div>
```

## Attribute Selectors (屬性選擇器)

``` css
/* 選擇有 id 屬性的 div 元素 */
div[id] {
    color: red;
}

/* 選擇 class 屬性值為「myclass」的 div 元素 */
div[class="myclass"] {
    color: red;
}

/* 選擇 title 屬性值含「od」的 a 元素 */
a[title*="od"] {
    color: red;
}

/* 選擇 title 屬性值含完整「good」字串的 a 元素 */
a[title~="good"] {
    color: red;
}

/* 選擇 href 屬性值開頭為「https」的 a 元素 */
a[href^="https"] {
    color: red;
}

/* 選擇 href 屬性值結尾為「pdf」的 a 元素 */
a[href$="pdf"] {
    color: red;
}
```

``` html
<div id="mydiv">
    <div class="myclass">
        <a title="good title">Hello</a>
        <a href="https://www.google.com">Click</a>
        <a href="note.pdf">Click</a>
    </div>
</div>
```

## Pseudo Selectors (虛擬選擇器)

### Behavioral (行為相關)

#### hover

``` css
a:hover {
    color: red;
}
```

#### active

``` css
a:active {
    color: blue;
}
```

#### visited

``` css
a:visited {
    color: green;
}
```

### Structural (結構相關)

#### First & Last Child

※ child 指的是所有子元素

``` css
/* 選擇 div 元素的後代中，第一個子元素 */
div p:first-child {
    color: red;
}

/* 選擇 div 元素的後代中，最後一個子元素 */
div p:last-child {
    color: blue;
}
```

``` html
<div>
    <p>I'll be red</p>
    <p>2nd</p>
    <p>3rd</p>
    <p>I'll be blue</p>
</div>
```

#### First & Last of Type

※ 若後代由多種元素組成，用 child 有可能會指不到目標元素，需改用 type 的方式

``` css
/* 用這個寫法不會生效，因為 div 元素的第一個子元素不是 p 元素
div p:first-child {
    color: red;
}
*/

/* 選擇 div 元素的後代中，第一個 p 元素 */
div p:first-of-type {
    color: red;
}

/* 選擇 div 元素的後代中，最後一個 p 元素 */
div p:last-of-type {
    color: blue;
}
```

``` html
<div>
    <h1>I'm first child now</h1>
    <p>I'll be red</p>
    <p>2nd</p>
    <p>3rd</p>
    <p>I'll be blue</p>
    <h1>I'm last child now</h1>
</div>
```

#### nth Child

※ nth 的起始值為 1

``` css
/* 選擇第三個 li 元素 */
li:nth-child(3) {
    color: red;
}

/* 選擇所有奇數的 li 元素 */
li:nth-child(odd) {
    color: blue;
}

/* 選擇所有奇數的 li 元素 */
li:nth-child(2n+1) {
    color: blue;
}

/* 選擇所有偶數的 li 元素 */
li:nth-child(even) {
    color: green;
}

/* 選擇所有偶數的 li 元素 */
li:nth-child(2n) {
    color: green;
}
```

``` html
<ul>
    <li>item 1</li>
    <li>item 2</li>
    <li>item 3</li>
    <li>item 4</li>
</ul>
```

#### nth of Type

※ nth 的起始值為 1

## Combining Selectors (選擇器合併使用)

``` css
div.myclass {
    color: red;
}
```

``` html
<!-- 僅「Hello」會被影響 -->
<div>
    <div class="myclass">Hello</div>
    <div>World</div>
</div>
```

## The Universal Selector (通用選擇器)

※ 瀏覽器預設樣式也會被覆蓋

``` css
/* 超連結文字在瀏覽器預設樣式為深藍色，但通用選擇器會覆蓋，所以顏色也會改成紅色 */
* {
    color: red;
}
```

## Conflicts & the Cascade (衝突)

一般情況下，CSS有衝突時

- 寫在後面 > 寫在前面
- 寫在內層(元素上) > 寫在外層

但是也要考量[分數系統](#Selector-Specificity-(選擇器分數系統))

## Inheritance (繼承關係)

外層的樣式會繼承到內層

## Selector Specificity (選擇器分數系統)

| Selectors | Points |
|:---------:|:------:|
| Elements  |   1    |
|  Classes  |   10   |
|    IDs    |  100   |

``` css
/* 原本 color 應該會被後寫的覆蓋，但因為這個選擇器有 101 分，所以會獲勝 */
#mydiv p {
    color: red;
}

/* 這個選擇器僅有 10 分 */
.myclass {
    color: blue;
}
```

``` html
<div id="mydiv">
    <p class="myclass">Hello World</p>
</div>
```

## The Important Declaration

「!important」修飾詞會是最強的，但是濫用會造成未來難以 debug

``` css
/* 這個選擇器有 101 分 */
#mydiv p {
    color: red;
}

/* 這個選擇器僅有 10 分，但有「!important」修飾的樣式最強，所以會獲勝 */
.myclass {
    color: blue !important;
}
```

``` html
<div id="mydiv">
    <p class="myclass">Hello World</p>
</div>
```

## Font Size (字體大小)

### Absolute

#### Pixels (px)

``` css
div {
    font-size: 48px;
}
```

### Relative

#### Em's (em) & Percentage (%)

em 和 % 都代表 inherit 後的字體大小乘以 N 倍

``` css
div {
    font-size: 16px;
}

div p {
    font-size: 3em;
}

div span {
    font-size: 50%;
}
```

``` html
<!-- Hello 的字體大小會是 16px -->
<!-- World 的字體大小會是 48px -->
<!-- Today 的字體大小會是 8px -->
<div>
    Hello
    <p>World</p>
    <span>Today</span>
</div>
```

## Font Family (字體)

依宣告順序使用字體，如果 client 端都沒有這些字體，就會使用瀏覽器預設樣式

``` css
p {
    font-family: 'arial', 'calibri';
}
```

## Text Decoration (文字修飾樣式)

底線、刪除線 .. 等等用法參考 [w3schools](https://www.w3schools.com/cssref/pr_text_text-decoration.asp)

``` css
p {
    text-decoration: underline;
}
```

## Font Weight (文字粗體)

粗體用法參考 [w3schools](https://www.w3schools.com/cssref/pr_font_weight.asp)

``` css
p {
    font-weight: bold;
}
```

## Text Transform (文字轉換大小寫)

首字大寫、全大寫、全小寫 .. 等等用法參考 [w3schools](https://www.w3schools.com/cssref/pr_text_text-transform.asp)

``` css
p {
    text-transform: capitalize;
}
```

## Text Color (文字顏色)

分為文字顏色和底色，值可以寫顏色名稱或色碼

``` css
p {
    color: red;
    background-color: black;
}

span {
    color: #112233;
    background-color: #332211;
}
```

## Letter Spacing & Line Height (字母間距和文字行高)

``` css
/* 字母間距 */
p {
    letter-spacing: 3px;
}

/* 單字間距 */
p {
    word-spacing: 6px;
}

/* 文字行高會和文字大小有關係，以下樣式的文字行高會是 24px */
p {
    font-size: 12px;
    line-height: 2em;
}
```

## The Box Model

![The Box Model](img/20190407_175022.png)

``` css
div {
    margin: 10px;
    padding: 5px;
    border: 1px solid #000000;
    width: 100px;
    height: 50px;
}
```

Result:

![Box Model Sample](img/20190407_175449.png)
