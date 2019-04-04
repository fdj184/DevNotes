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

- by tags

    ``` css
    div {
        color: red;
    }
    ```

    ``` html
    <div id="mydiv">Hello World</div>
    ```

- by IDs

    ``` css
    #mydiv {
        color: red;
    }
    ```

    ``` html
    <div id="mydiv">Hello World</div>
    ```

- by Classes

    ``` css
    .myclass {
        color: red;
    }
    ```

    ``` html
    <div class="myclass">Hello World</div>
    ```

## Targeting Multiple Elements (指定多個目標)

以下例子，選擇所有 h1 和 h2 元素

``` css
h1, h2 {
    color: red;
}
```

## Descendant Selectors (後代選擇器)

以下例子，選擇 div 後代的所有 p 元素

``` css
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

以下例子，選擇 div 直接子代的 p 元素

``` css
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

以下例子，選擇緊接在 span 之後的 p 元素

``` css
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

/* 選擇 title 屬性值含「title2」字串的 a 元素 */
a[title~="title2"] {
    color: red;
}

/* 選擇 href 屬性值開頭為「https」字串的 a 元素 */
a[href^="https"] {
    color: red;
}

/* 選擇 href 屬性值結尾為「pdf」字串的 a 元素 */
a[href$="pdf"] {
    color: red;
}
```

``` html
<div id="mydiv">
    <div class="myclass">
        <a title="title1 title2">Hello</a>
        <a href="https://www.google.com">Click</a>
        <a href="note.pdf">Click</a>
    </div>
</div>
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

以下例子，前者有 101 分，後者僅有 10 分，所以前者獲勝

``` html
<div id="mydiv">
    <p class="myclass">Hello World</p>
</div>
```

``` css
#mydiv p {
    color: red;
}

.myclass {
    color: blue;
}
```

## The Important Declaration

「!important」修飾詞會是最強的，但是若濫用會造成未來難以 debug

以下例子，有「!important」修飾的樣式獲勝

``` html
<div id="mydiv">
    <p class="myclass">Hello World</p>
</div>
```

``` css
#mydiv p {
    color: red;
}

.myclass {
    color: blue !important;
}
```
