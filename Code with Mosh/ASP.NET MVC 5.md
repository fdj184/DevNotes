# The Complete ASP&#46;NET MVC 5 Course

![ASP.NET MVC 5](img/20190517_172500.jpg)

Course Link: <https://codewithmosh.com/p/asp-net-mvc>

---

## MVC 架構(MVC Architectural Pattern)

|                |                                                                                               Model                                                                                                |                    View                     |                     Controller                     |                              Router                               |
|:--------------:|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------:|:-------------------------------------------:|:--------------------------------------------------:|:-----------------------------------------------------------------:|
|      職責      |                                                                            ![20190517_173736](img/20190517_173736.png)                                                                             | ![20190517_174013](img/20190517_174013.png) |    ![20190517_174046](img/20190517_174046.png)     |            ![20190517_182341](img/20190517_182341.png)            |
| 以租片網站為例 |                                                                            ![20190517_173910](img/20190517_173910.png)                                                                             |                                             |    ![20190517_174347](img/20190517_174347.png)     |            ![20190517_182644](img/20190517_182644.png)            |
|      說明      | <ul align="left"><li>僅透過類別(Class)中的屬性(Attribute)或方法(Method)，來表示應用程式的狀態或規則</li><li>因為不依賴 UI，所以 Model 中的邏輯也可以拿去套用在 desktop app 或 mobile app</li></ul> |                  網頁呈現                   | Controller 會去 Model 取得資料，並放到 View 作呈現 | 造訪任一網頁時，Router 會選擇出正確的 Controller 去作它該作的工作 |

## 預設專案結構

![20190517_185504](img/20190517_185504.png)

- :file_folder: App_Data: 存放 DB 檔案
- :file_folder: App_Start: 應用程式啟動時會執行的檔案
    - BundleConfig.cs: 合併和壓縮 css 或 javascript 成一個 bundle，可以減少 http request 數，也就加快網頁載入速度
    - FilterConfig.cs
    - RouteConfig.cs: 設定 route 規則，以下圖為例

        ![20190517_190220](img/20190517_190220.png)

        當 url 符合「{controller}/{action}/{id}」格式時，route 就會去解析該由哪個 controller 的哪個 action 去作事

        ![20190517_190450](img/20190517_190450.png)

        ![20190517_190737](img/20190517_190737.png)

        如果不符合「{controller}/{action}/{id}」格式或 url 不完整時，就會將無法解析的項目交給預設值

        ![20190517_191051](img/20190517_191051.png)

- :file_folder: Content: 存放 css 檔案、圖片 .. 等等 client side 資源
- :file_folder: Controllers: 存放 controller 檔案
    - HomeController.cs: 首頁的 controller
- :file_folder: fonts: 存放字型檔案，可以考慮放到 Content 目錄底下
- :file_folder: Models: 存放 model 檔案
- :file_folder: Scripts: 存放 javascript 檔案
- :file_folder: Views: 存放 view 檔案，每個 controller 在這裡都會有一個對應名稱的資料夾
    - :file_folder: Home: 對應 HomeController.cs 的 view
    - :file_folder: Shared: 供不同 controller 共同使用的 view
- favicon.ico: 供瀏覽器顯示的網站圖示
- Global.asax: 為各種事件提供 hooks 或生命週期的全域類別
    - Application_Start() 中會在應用程式啟動時執行，例如註冊 route
- packages.config: NuGet 套件管理，與其逐一到外部套件網站下載，我們透過 NuGet 來統一下載、更新和管理套件

    ![20190517_193345](img/20190517_193345.png)

- Web.config: 應用程式的各種設定，為 xml 格式，通常只有兩個區塊會作編輯
    - ```connectionStrings```: DB 連線字串
    - ```appSettings```: 應用程式的各種設定

## MVC in Action

以新增隨選電影頁面為例

1. 新增 Model
    1. 在「Models」資料夾新增「Movie.cs」

        ![20190517_230442](img/20190517_230442.png)

    2. 在「Movie.cs」建立 ```Movie``` 類別並添加 ```Id``` 和 ```Name``` 屬性(Property)

        ![20190517_230315](img/20190517_230315.png)

2. 新增 Controller
    1. 在「Controller」資料夾新增「MoviesController.cs」，此時 Visual Studio 也會幫我們建立對應名稱的「Views\Movies」資料夾

        ![20190517_230620](img/20190517_230620.png)

    2. 在「MoviesController.cs」中將 action 名稱改為 ```Random()```，函式中的電影在現實案例會是從資料庫撈出，這邊為作示範僅寫死一部電影，並將 ```movie``` 物件丟到 View 中回傳

        ![20190517_233941](img/20190517_233941.png)

3. 新增 View
    1. 有了 Controller 也要有對應的 View，在「Views\Movies」資料夾加入檢視(View)
        - [部分檢視(Partial View)](#Partial-Views)指的是比較小單位的 View，可以重複使用、組合而成一個完整的 View，這邊不勾選
        - 版面配置頁(Layout Page)代表可以使用 template page 或 master page 讓檢視有相似的風格樣式，這邊選擇內建的「~/Views/Shared/_Layout.cshtml」

        ![20190517_233306](img/20190517_233306.png)

    2. 建立後的「Random.cshtml」分為兩個部分
        - C# code
            - ViewBag.Title: 供瀏覽器顯示的網站標題
            - Layout: 剛剛設定的版面配置頁
        - html

        ![20190517_234141](img/20190517_234141.png)

    3. 欲取得隨選電影的名稱，如果直接在 html 區塊寫 ```@Model``` 會是 dynamic 型別，無法在編譯階段知道 Model 中有什麼類別成員，但是透過在檔案最上方使用 ```@model``` 關鍵字，就可以在編譯階段使用 ```Movie``` 類別中的類別成員

        ![20190517_234831](img/20190517_234831.png)

4. Ctrl+F5(開啟，但不進行偵錯)檢查執行結果

    ![20190517_235917](img/20190517_235917.png)

## 加入主題(Adding a Theme)

ASP<span>.</span>NET MVC 使用 [Bootstrap](https://getbootstrap.com/) 作為前端框架，我們可以到 [Bootswatch](https://bootswatch.com/3/) 尋找免費的 Bootstrap 主題回來替換，以「Lumen」這個主題為例

1. 下載 css，並重新命名為「bootstrap-lumen.css」
2. 加入至專案的「Content」資料夾下

    ![20190518_001158](img/20190518_001158.png)

3. 開啟「App_Start\BundleConfig.cs」，將原本載入 ```bootstrap.css``` 的地方改為 ```bootstrap-lumen.css```

    ![20190518_001901](img/20190518_001901.png)

4. Ctrl+Shift+B(建置方案)後檢查執行結果

## 動作結果(Action Results)

- ```ActionResult``` 類別用來表示每個 action 的結果
- 在下圖 Controller 中，我們可以看到 ```Random()``` 這個 action 應該要回傳 ```ActionResult``` 類別，但是實際上是回傳 ```ViewResult``` 類別，因為後者其實是前者的衍生類別(Derived Class)

    ![20190518_162750](img/20190518_162750.png)

- ```ActionResult``` 類別有以下衍生類別，這些都可以當作 action 回傳的類別，有些還能透過 helper method 簡化

    | 較常用 |         類別          |   Helper Method    |                  功能                  |
    |:------:|:---------------------:|:------------------:|:--------------------------------------:|
    |   V    |      ViewResult       |       View()       |             return a view              |
    |        |   PartialViewResult   |   PartialView()    |         return a partial view          |
    |        |     ContentResult     |     Content()      |          return a simple text          |
    |   V    |    RedirectResult     |     Redirect()     |           redirect to a url            |
    |        | RedirectToRouteResult | RedirectToAction() |      redirect to an action method      |
    |        |      JsonResult       |       Json()       |          return a JSON object          |
    |        |      FileResult       |       File()       |             return a file              |
    |   V    |  HttpNotFoundResult   |   HttpNotFound()   | return "Not Found" or "404 Error" page |
    |        |      EmptyResult      |                    |     nothing to return, like "void"     |

- 以下述程式碼為例

    <table>
    <tr align="center">
    <th>程式碼</th>
    <th>結果</th>
    </tr>
    <tr>
    <td>

    ``` csharp
    public class MoviesController : Controller
    {
        // GET: Movies/Random
        public ActionResult Random()
        {
            // ContentResult
            return Content("Hello World!");
        }
    }
    ```

    </td>
    <td>

    ![20190518_173345](img/20190518_173345.png)

    </td>
    </tr>
    <tr>
    <td>

    ``` csharp
    public class MoviesController : Controller
    {
        // GET: Movies/Random
        public ActionResult Random()
        {
            // HttpNotFoundResult
            return HttpNotFound();
        }
    }
    ```

    </td>
    <td>

    ![20190518_173222](img/20190518_173222.png)

    </td>
    </tr>
    <tr>
    <td>

    ``` csharp
    public class MoviesController : Controller
    {
        // GET: Movies/Random
        public ActionResult Random()
        {
            // RedirectToRouteResult
            return RedirectToAction(
                "Index", "Home", new { page = 1, sortBy = "name" }
            );
        }
    }
    ```

    </td>
    <td>

    ![20190518_173457](img/20190518_173457.png)

    </td>
    </table>

## 動作參數(Action Parameters)

- 參數綁定(Parameter Binding)的過程

    ![20190518_173945](img/20190518_173945.png)

- 參數可以有以下形式

    ![20190518_174254](img/20190518_174254.png)

- 以下述程式碼為例
    1. 在「MoviesController.cs」新增一個 ```Edit()``` action，並傳入參數 ```id```

        ![20190518_175435](img/20190518_175435.png)

    2. 建置後，可以透過「Movies/Edit/1」或「Movies/Edit?id=1」看到我們有正確取得參數

        ![20190518_175802](img/20190518_175802.png)

        ![20190518_175747](img/20190518_175747.png)

    3. 但是如果把參數名稱改為 ```movieId```

        ![20190518_180119](img/20190518_180119.png)

    4. 建置後，可以看到「Movies/Edit?movieId=1」可以運作，但「Movies/Edit/1」是出錯的

        ![20190518_180315](img/20190518_180315.png)

        ![20190518_180452](img/20190518_180452.png)

    5. 因為在「App_Start\RouteConfig.cs」中，預設 route 的預設參數寫作「id」

        ![20190518_180759](img/20190518_180759.png)

    6. 欲修正此錯誤有兩種方式
        1. 新增自定義的 route，會在下一個章節介紹
        2. 若參數為選填，可以將 value type 的參數改為 [Nullable](C%23%20Advanced.md#Nullable-Types)，並自訂預設值

            ※ ```string``` 為 reference type，原本就允許空值，所以不必特別處理

            ![20190518_182446](img/20190518_182446.png)

            ![20190518_182617](img/20190518_182617.png)

## Convention Routing

- 在「App_Start\RouteConfig.cs」中設定
- route 宣告順序會影響優先權，所以越細的 route 要寫在越前面
- ```MapRoute()``` 有好幾種多載，最常用的下列前三個
    1. name: 必須是唯一值(unique)
    2. url: url 格式，符合此格式就會使用這個 route，若有參數須用大括號包覆
    3. defaults (objects): 預設使用的 controller, action, default value of parameters
    4. constraints (objects): 可透過正規表示式(regular expression)設定 url 參數的條件約束
- 以下述程式碼為例
    1. 欲新增「依發行月份取得電影清單」之 action
    2. 先到「RouteConfig.cs」加入自定義 route，在 url 格式加上 ```{year}``` 和 ```{month}``` 參數

        ※ 要記得寫在 default route 的前面，才有優先權

        ![20190519_031747](img/20190519_031747.png)

    3. 回到「MoviesController.cs」，新增 ```ByReleaseDate``` action

        ![20190519_032337](img/20190519_032337.png)

    4. Ctrl+Shift+B 建置後，瀏覽「*專案 url*/movies/release/」會找不到頁面，原因是這個 url 格式對應不到任何的 route

        ![20190519_034629](img/20190519_034629.png)

    5. 如果在 url 加上參數，就可以順利瀏覽

        ![20190519_034828](img/20190519_034828.png)

    6. 如果要更進一步限制年份為4碼數字、月份為2碼數字，可以回到「RouteConfig.cs」增加 url 參數的條件約束

        ![20190519_035708](img/20190519_035708.png)

    7. 建置後，剛剛的「*專案 url*/movies/release/2019/5」就找不到頁面了，原因同步驟 4

        ![20190519_040100](img/20190519_040100.png)

    8. 改用「*專案 url*/movies/release/2019/**05**」才能順利瀏覽

        ![20190519_040225](img/20190519_040225.png)

## Attribute Routing

- Convention Routing 有以下缺點
    1. 隨著自定義的 route 越來越多，「RouteConfig.cs」會越來越大
    2. 必須來回在 controller 和「RouteConfig.cs」間來回修改
    3. 承上點，若在任一邊修改 action name，另一邊須記得修改，否則無法順利導向
- 若專案為 ASP&#46;NET MVC 5 之後的版本，建議使用 Attribute Routing，可以更簡潔的作到自定義 route
- 以下述程式碼為例
    1. 原本的「RouteConfig.cs」如下

        ![20190519_031747](img/20190519_031747.png)

    2. 刪掉原本的自定義 route，並使用 ```MapMvcAttributeRoutes()```

        ![20190519_103935](img/20190519_103935.png)

    3. 回到 controller，在 action method 前面加上屬性

        ※ 透過「{*參數*:regex(*expression*)}」可以為參數加上正規表示式的條件約束，其它條件約束方式可參考 [Routing Constraints](https://docs.microsoft.com/zh-tw/aspnet/web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2#route-constraints)

        ![20190519_105132](img/20190519_105132.png)

## Passing Data to Views

從 Controller 傳遞資料到 View 有以下方式

|          |                 ActionResult                 |                                                                                                                                   ViewData                                                                                                                                   |                                                                   ViewBag                                                                   |
|:--------:|:--------------------------------------------:|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------:|:-------------------------------------------------------------------------------------------------------------------------------------------:|
|   說明   |       透過 ```ActionResult``` 回傳物件       |                                                                                                          用 dictionary 型別的 ```ViewData``` 其 key/value 存放物件                                                                                                           |                                               用 dynamic 型別的 ```ViewBag``` 其屬性存放物件                                                |
|   範例   | 參考 [ActionResult](#動作結果Action-Results) |                                                                             Controller<br>![20190520_193005](img/20190520_193005.png)<br><br>View<br>![20190520_193310](img/20190520_193310.png)                                                                             |            Controller<br>![20190520_193614](img/20190520_193614.png)<br><br>View<br>![20190520_193746](img/20190520_193746.png)             |
|   缺點   |                                              | <ul align="left"><li>修改 key 名稱的話，在 Controller 和 View 都得修改</li><li>View 的程式碼因必須轉型而顯得冗長</li><li>ViewData 的 dictionary value 為 object 型別，轉型會帶來[裝箱(Boxing)和拆箱(Unboxing)](C%23%20Intermediate.md#Boxing-and-Unboxing)的副作用</li></ul> | <ul align="left"><li>修改屬性名稱的話，在 Controller 和 View 都得修改</li><li>ViewBag 為 dynamic 型別，沒辦法在編譯階段就發現 bug</li></ul> |
| 建議使用 |              :heavy_check_mark:              |                                                                                                                                                                                                                                                                              |                                                                                                                                             |

## View Models

- 通常一個 View 只能指定一種 Model 類別，如果一個 View 中需要同時用到很多種 Model，可以改成建立 ViewModel，並將欲使用的 Model 們設為屬性，在 View 中則改為指定這個自定義的 ViewModel
- 以下述程式碼為例
    1. 目前程式碼如下
        |            |                             目前程式碼                             |
        |:----------:|:------------------------------------------------------------------:|
        |   Model    |      Movie.cs<br>![20190517_230315](img/20190517_230315.png)       |
        |    View    |    Movie.cshtml<br>![20190517_234831](img/20190517_234831.png)     |
        | Controller | MoviesController.cs<br>![20190520_224716](img/20190520_224716.png) |
    2. 欲在 ```Random()``` action 中同時使用 ```Movie``` 和 ```List<Customer>``` 兩個類別
    3. 先新增 ```Customer``` 類別在 Model 中

        ![20190520_225145](img/20190520_225145.png)

        ![20190520_225228](img/20190520_225228.png)

    4. 接著在專案目錄中新增「ViewModels」資料夾，並新增「RandomMovieViewModel.cs」

        ![20190520_225557](img/20190520_225557.png)

        ![20190520_225735](img/20190520_225735.png)

    5. 回到 Controller 實作 ```RandomMovieViewModel``` 並放到 action result 中回傳

        ![20190520_230414](img/20190520_230414.png)

    6. 在 View 中，原本是指定 Model，現在要改成指定 ViewModel

        ![20190520_230756](img/20190520_230756.png)

## Razor Syntax

- 在「.cshtml」檔案中，可以混用 html 和 C# code(要使用 ```@``` 符號開頭)
- 以下述程式碼為例
    - foreach loop 和 if-else statement
        |                    View                     |                    結果                     |
        |:-------------------------------------------:|:-------------------------------------------:|
        | ![20190520_232839](img/20190520_232839.png) | ![20190520_233017](img/20190520_233017.png) |
    - dynamic attributes

        ![20190520_233812](img/20190520_233812.png)

    - 註解使用「```@*``` *comment* ```*@```」區塊

        ![20190520_234135](img/20190520_234135.png)

## Partial Views

- 想像成比較小單位的 View，可以重複使用在不同 View 中，也可以組合多個 Partial View 成一個完整的 View
- 命名規則為底線開頭，例如「_NavBar.cshtml」
- 以下述程式碼為例
    1. 欲將「Views\Shared\_Layout.cshtml」的導覽列拆成獨立的檔案
    2. 在「Views\Shared」資料夾新增部分檢視(partial view)，命名須為底線開頭

        ![20190521_032410](img/20190521_032410.png)

        ![20190521_032635](img/20190521_032635.png)

    3. 將「Views\Shared\_Layout.cshtml」的導覽列程式碼剪下並貼到「_NavBar.cshtml」

        ![20190521_033047](img/20190521_033047.png)

    4. 接著在「_Layout.cshtml」原處，透過 ```@Html.Partial()``` 渲染 partial view

        ![20190521_033534](img/20190521_033534.png)

    5. ```@Html.Partial()``` 有另一個多載是傳入 partial view 和 model，不過這邊沒使用到
