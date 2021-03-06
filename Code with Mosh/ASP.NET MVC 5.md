# The Complete ASP&#46;NET MVC 5 Course

![ASP.NET MVC 5](img/20190517_172500.jpg)

Course Link: <https://codewithmosh.com/p/asp-net-mvc>

---

## Getting Started

### MVC 架構 (MVC Architectural Pattern)

|                |                                                                                               Model                                                                                                |                    View                     |                     Controller                     |                              Router                               |
|:--------------:|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------:|:-------------------------------------------:|:--------------------------------------------------:|:-----------------------------------------------------------------:|
|      職責      |                                                                            ![20190517_173736](img/20190517_173736.png)                                                                             | ![20190517_174013](img/20190517_174013.png) |    ![20190517_174046](img/20190517_174046.png)     |            ![20190517_182341](img/20190517_182341.png)            |
| 以租片網站為例 |                                                                            ![20190517_173910](img/20190517_173910.png)                                                                             |                                             |    ![20190517_174347](img/20190517_174347.png)     |            ![20190517_182644](img/20190517_182644.png)            |
|      說明      | <ul align="left"><li>僅透過類別 (Class) 中的屬性 (Attribute) 或方法 (Method)，來表示應用程式的狀態或規則</li><li>因為不依賴 UI，所以 Model 中的邏輯也可以拿去套用在 desktop app 或 mobile app</li></ul> |                  網頁呈現                   | Controller 會去 Model 取得資料，並放到 View 作呈現 | 造訪任一網頁時，Router 會選擇出正確的 Controller 去作它該作的工作 |

### 建立 MVC 專案

1. 建立新專案時，選擇「ASP&#46;NET Web Application」

    ![20190521_233105](img/20190521_233105.png)

2. 設定專案名稱、存放路徑和 .NET Framework 版本

    ![20190521_233259](img/20190521_233259.png)

3. 選擇 MVC 範本，並將驗證模式改為「個別使用者帳戶 (Individual User Accounts)」

    ![20190521_233504](img/20190521_233504.png)

### 預設專案結構

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

### MVC in Action

以新增隨選電影頁面為例

1. 新增 Model
    1. 在「Models」資料夾新增「Movie.cs」

        ![20190517_230442](img/20190517_230442.png)

    2. 在「Movie.cs」建立 ```Movie``` 類別並添加 ```Id``` 和 ```Name``` 屬性 (Property)

        ![20190517_230315](img/20190517_230315.png)

2. 新增 Controller
    1. 在「Controller」資料夾新增「MoviesController.cs」，此時 Visual Studio 也會幫我們建立對應名稱的「Views\Movies」資料夾

        ![20190517_230620](img/20190517_230620.png)

    2. 在「MoviesController.cs」中將 action 名稱改為 ```Random()```，函式中的電影在現實案例會是從資料庫撈出，這邊為作示範僅寫死一部電影，並將 ```movie``` 物件丟到 View 中回傳

        ![20190517_233941](img/20190517_233941.png)

3. 新增 View
    1. 有了 Controller 也要有對應的 View，在「Views\Movies」資料夾加入檢視 (View)
        - [部分檢視 (Partial View)](#partial-views) 指的是比較小單位的 View，可以重複使用、組合而成一個完整的 View，這邊不勾選
        - 版面配置頁 (Layout Page) 代表可以使用 template page 或 master page 讓檢視有相似的風格樣式，這邊選擇內建的「~/Views/Shared/_Layout.cshtml」

        ![20190517_233306](img/20190517_233306.png)

    2. 建立後的「Random.cshtml」分為兩個部分
        - C# code
            - ViewBag.Title: 供瀏覽器顯示的網站標題
            - Layout: 剛剛設定的版面配置頁
        - html

        ![20190517_234141](img/20190517_234141.png)

    3. 欲取得隨選電影的名稱，如果直接在 html 區塊寫 ```@Model``` 會是 dynamic 型別，無法在編譯階段知道 Model 中有什麼類別成員，但是透過在檔案最上方使用 ```@model``` 關鍵字，就可以在編譯階段使用 ```Movie``` 類別中的類別成員

        ![20190517_234831](img/20190517_234831.png)

4. Ctrl+F5 (開啟，但不進行偵錯) 檢查執行結果

    ![20190517_235917](img/20190517_235917.png)

### 加入主題 (Adding a Theme)

ASP<span>.</span>NET MVC 使用 [Bootstrap](https://getbootstrap.com/) 作為前端框架，我們可以到 [Bootswatch](https://bootswatch.com/3/) 尋找免費的 Bootstrap 主題回來替換，以「Lumen」這個主題為例

1. 下載 css，並重新命名為「bootstrap-lumen.css」
2. 加入至專案的「Content」資料夾下

    ![20190518_001158](img/20190518_001158.png)

3. 開啟「App_Start\BundleConfig.cs」，將原本載入 ```bootstrap.css``` 的地方改為 ```bootstrap-lumen.css```

    ![20190518_001901](img/20190518_001901.png)

4. Ctrl+Shift+B (建置方案) 後檢查執行結果

### 動作結果 (Action Results)

- ```ActionResult``` 類別用來表示每個 action 的結果
- 在下圖 Controller 中，我們可以看到 ```Random()``` 這個 action 應該要回傳 ```ActionResult``` 類別，但是實際上是回傳 ```ViewResult``` 類別，因為後者其實是前者的衍生類別 (Derived Class)

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

### 動作參數 (Action Parameters)

- 參數綁定 (Parameter Binding) 的過程

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
        2. 若參數為選填，可以將 value type 的參數改為 [Nullable](C%23%20Advanced.md#nullable-types)，並自訂預設值

            > ※ ```string``` 為 reference type，原本就允許空值，所以不必特別處理

            ![20190518_182446](img/20190518_182446.png)

            ![20190518_182617](img/20190518_182617.png)

### Convention Routing

- 在「App_Start\RouteConfig.cs」中設定
- route 宣告順序會影響優先權，所以越細的 route 要寫在越前面
- ```MapRoute()``` 有好幾種多載，最常用的下列前三個
    1. name: 必須是唯一值 (unique)
    2. url: url 格式，符合此格式就會使用這個 route，若有參數須用大括號包覆
    3. defaults (objects): 預設使用的 controller, action, default value of parameters
    4. constraints (objects): 可透過正規表示式 (regular expression) 設定 url 參數的條件約束
- 以下述程式碼為例
    1. 欲新增「依發行月份取得電影清單」之 action
    2. 先到「RouteConfig.cs」加入自定義 route，在 url 格式加上 ```{year}``` 和 ```{month}``` 參數

        > ※ 要記得寫在 default route 的前面，才有優先權

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

### Attribute Routing

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

        > ※ 透過「{*參數*:regex(*expression*)}」可以為參數加上正規表示式的條件約束，其它條件約束方式可參考 [Routing Constraints](https://docs.microsoft.com/zh-tw/aspnet/web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2#route-constraints)

        ![20190519_105132](img/20190519_105132.png)

### Passing Data to Views

從 Controller 傳遞資料到 View 有以下方式

|          |                 ActionResult                 |                                                                                                                                   ViewData                                                                                                                                   |                                                                   ViewBag                                                                   |
|:--------:|:--------------------------------------------:|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------:|:-------------------------------------------------------------------------------------------------------------------------------------------:|
|   說明   |       透過 ```ActionResult``` 回傳物件       |                                                                                                          用 dictionary 型別的 ```ViewData``` 其 key/value 存放物件                                                                                                           |                                               用 dynamic 型別的 ```ViewBag``` 其屬性存放物件                                                |
|   範例   | 參考 [ActionResult](#動作結果-action-results) |                                                                             Controller<br>![20190520_193005](img/20190520_193005.png)<br><br>View<br>![20190520_193310](img/20190520_193310.png)                                                                             |            Controller<br>![20190520_193614](img/20190520_193614.png)<br><br>View<br>![20190520_193746](img/20190520_193746.png)             |
|   缺點   |                                              | <ul align="left"><li>修改 key 名稱的話，在 Controller 和 View 都得修改</li><li>View 的程式碼因必須轉型而顯得冗長</li><li>ViewData 的 dictionary value 為 object 型別，轉型會帶來[裝箱 (Boxing) 和拆箱 (Unboxing)](C%23%20Intermediate.md#boxing-and-unboxing) 的副作用</li></ul> | <ul align="left"><li>修改屬性名稱的話，在 Controller 和 View 都得修改</li><li>ViewBag 為 dynamic 型別，沒辦法在編譯階段就發現 bug</li></ul> |
| 建議使用 |              :heavy_check_mark:              |                                                                                                                                                                                                                                                                              |                                                                                                                                             |

### View Models

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

### Razor Syntax

- 在「.cshtml」檔案中，可以混用 html 和 C# code (要使用 ```@``` 符號開頭)
- 以下述程式碼為例
    - foreach loop 和 if-else statement
        |                    View                     |                    結果                     |
        |:-------------------------------------------:|:-------------------------------------------:|
        | ![20190520_232839](img/20190520_232839.png) | ![20190520_233017](img/20190520_233017.png) |
    - dynamic attributes

        ![20190520_233812](img/20190520_233812.png)

    - 註解使用「```@*``` *comment* ```*@```」區塊

        ![20190520_234135](img/20190520_234135.png)

### Partial Views

- 想像成比較小單位的 View，可以重複使用在不同 View 中，也可以組合多個 Partial View 成一個完整的 View
- 命名規則為底線開頭，例如「_NavBar.cshtml」
- 以下述程式碼為例
    1. 欲將「Views\Shared\_Layout.cshtml」的導覽列拆成獨立的檔案
    2. 在「Views\Shared」資料夾新增部分檢視 (partial view)，命名須為底線開頭

        ![20190521_032410](img/20190521_032410.png)

        ![20190521_032635](img/20190521_032635.png)

    3. 將「Views\Shared\_Layout.cshtml」的導覽列程式碼剪下並貼到「_NavBar.cshtml」

        ![20190521_033047](img/20190521_033047.png)

    4. 接著在「_Layout.cshtml」原處，透過 ```@Html.Partial()``` 渲染 partial view

        ![20190521_033534](img/20190521_033534.png)

    5. ```@Html.Partial()``` 有另一個多載是傳入 partial view 和 model，不過這邊沒使用到

### Exercise in section 2

1. 在「/Movies」頁面用 hard code 方式，在表格中顯示兩部電影
2. 建立 ```Customer``` 類別
3. 在「/Customers」頁面用 hard code 方式，在表格中顯示兩個客戶姓名 (```Customer.Name```)
4. 承上，若目前無客戶，需顯示「Sorry, we don't have any customer yet.」
5. 承上，客戶姓名可超連結至「/Customers/Details/*{id}*」
6. 承上，該頁僅需顯示該客戶姓名

解答可參考我的 [repo](https://github.com/fdj184/Vidly/commits/develop)

## Working with Data

### Entity Framework

- 一種物件關聯式對應程式 (O/RM)，透過資料抽象化將將每個資料庫物件都轉換成應用程式物件 (entity)，而資料欄位都轉換為屬性 (property)，關聯則轉換為結合屬性 (association)

    ![20190521_184623](img/20190521_184623.png)

- Entity Framework 提供
    1. ```DbContext``` 類別：代表 DB 或是多張 table、view 的集合
    2. ```DbSet``` 類別：代表單張 table、view

    ![20190521_200742](img/20190521_200742.png)

- 實際使用時，我們會透過 LINQ 操作 ```DbSet``` 物件
    - 若 LINQ 為查詢指令，會在 runtime 階段轉為 SQL 向 DB 要資料，DB 再將撈到的資料回傳至 ```DbSet```

        ![20190521_201302](img/20190521_201302.png)

    - 若 LINQ 為新增、修改或刪除指令，```DbSet``` 會記錄過程的變化，並在我們要求異動時，一樣轉為 SQL 要求 DB 執行

        ![20190521_201709](img/20190521_201709.png)

#### Database First vs Code First

|        |               Database First                |                 Code First                  |
|:------:|:-------------------------------------------:|:-------------------------------------------:|
|  描述  |     傳統策略，先有 DB tables，才建立 EF     |     先寫程式，再透過 DF 建立 DB tables      |
| 示意圖 | ![20190521_223011](img/20190521_223011.png) | ![20190521_223219](img/20190521_223219.png) |

> ※ 兩者詳細差異，可參考 Mosh 的另一堂課 [Entity Framework 6 in Depth](https://codewithmosh.com/p/entity-framework)

### Code First Migrations

- 若使用 Code First 策略，在每次建立或修改類別 (Class) 時，我們就需要建立移轉 (Migration) 並寫至 DB
- 以下述操作為例
    1. 開啟套件管理器主控台 (Package Manager Console)

        ![20190521_225253](img/20190521_225253.png)

    2. 首次使用 Migration 時要先作啟用，在 console 輸入 ```enable-migrations```

        ![20190521_225704](img/20190521_225704.png)

    3. 啟用成功後會出現「Migrations\Configuration.cs」

        ![20190522_001538](img/20190522_001538.png)

    4. 輸入「add-migration *{移轉名稱}*」以建立移轉，例如 ```add-migration InitialModel```

        ![20190522_002454](img/20190522_002454.png)

    5. 建立後會出現「Migrations\\*{guid}*_*{移轉名稱}*.cs」，例如「Migrations\201905211620599_InitialModel.cs」

        ![20190522_002754](img/20190522_002754.png)

    6. 承上，這個建立移轉的動作，會在 Model 中尋找有使用 ```DbContext``` 的類別並建立快照，以此專案為例，目前快照的來源皆來自「Models\IdentityModels.cs」的 ```ApplicationDbContext``` 類別，為 ASP&#46;NET 內建的驗證身分和授權功能

        > ※ ```ApplicationDbContext``` 為 ```IdentityDbContext``` 的衍生類別，```IdentityDbContext``` 又為 ```DbContext``` 的衍生類別

        ![20190522_004811](img/20190522_004811.png)

    7. 為了讓我們自定義的 ```Movie``` 和 ```Customer``` 類別也建立自己的 table，我們在 ```ApplicationDbContext``` 類別下用增加屬性 (Property) 的方式宣告我們的自定義類別，型別為 ```DbSet<T>```

        ![20190522_010136](img/20190522_010136.png)

    8. 接著再一次建立移轉，可以使用新的移轉名稱，也可以用「add-migration *{移轉名稱}* -force」洗掉原本的移轉內容，這邊以後者為例

        > ※ ```cls``` 指令可以清掉 console 訊息

        ![20190522_011706](img/20190522_011706.png)

    9. 回到「Migrations\201905211620599_InitialModel.cs」可以看到 ```Movie``` 和 ```Customer``` 類別對應的 LINQ 語法已經建立

        ![20190522_011904](img/20190522_011904.png)

    10. 接著在 console 輸入 ```update-database```，便會在「App_Data」資料夾底下建立 DB 檔案 (.mdf)

        ![20190522_012328](img/20190522_012328.png)

    11. 如果在方案總管沒看到，可以點擊「顯示所有檔案」

        ![20190522_012551](img/20190522_012551.png)

    12. 雙擊 mdf 檔，即可在伺服器總管檢視我們的 table 和 column 的確有被建立

        ![20190522_012807](img/20190522_012807.png)

### Changing the Model

為了讓 Model 更接近實務般的複雜，我們欲在 ```Customer``` 類別加入 ```IsSubscribedToNewsletter``` 和 ```MembershipType``` 兩個屬性，其中，```MembershipType``` 為一類別，並擁有 ```SignUpFee```, ```DurationInMonths```, ```DiscountRate``` 三個屬性，以下述操作為例

1. 先在 ```Customer``` 類別加入 ```IsSubscribedToNewsletter``` 屬性

    ![20190522_155739](img/20190522_155739.png)

2. 建立移轉，並更新至 DB 檔案

    > ※ 使用 Code First 策略時，建立移轉時機類似於 Git 的 commit，每作完一個單位的異動就建立一次移轉，而不是作完全部功能才建立一個移轉，這樣比較不會有問題

    ![20190522_144901](img/20190522_144901.png)

3. 建立 ```MembershipType``` 類別，除了先前提到的三個屬性，我們還必須額外建立 ```Id``` 欄位，作為 DB table 的 primary key

    > ※ .NET Framework 和 SQL Server 資料型別對應可參考這篇：[C# Equivalent of SQL Server DataTypes](https://stackoverflow.com/questions/425389/c-sharp-equivalent-of-sql-server-datatypes)

    ![20190522_151813](img/20190522_151813.png)

4. 回到 ```Customer``` 類別
    1. 加入 ```MembershipType``` 屬性，這個屬性讓 EF 在建立移轉時，意識到也要建立 \[MembershipTypes] 這張 table
    2. 加入 ```MembershipTypeId``` 屬性，這個屬性讓 EF 知道要將 \[Customers].\[MembershipTypeId] 這個欄位當作 foreign key 指向 \[MembershipTypes].\[Id]

    ![20190522_160803](img/20190522_160803.png)

5. 建立移轉，並更新至 DB 檔案

    ![20190522_161603](img/20190522_161603.png)

    ![20190522_161846](img/20190522_161846.png)

### Seeding the Database

採用 Code First 策略時，如果欲新增的資料為應用程式的一部份，則應該透過程式碼和移轉來新增資料，而不是經由其它介面來下指令，以下述操作為例

> ※ 在此應用程式中，「客戶類別」為應用程式的一部份，必須事先定義且不太會異動

1. 欲在 \[MembershipTypes] table 新增四筆資料
2. 先建立一個空白的移轉

    ![20190522_163209](img/20190522_163209.png)

3. 承上，在產出的移轉檔案中，在 ```Up()``` 區塊手動加上新增資料的 SQL 語法

    ![20190522_170615](img/20190522_170615.png)

4. 更新至 DB 檔案

    ![20190522_170807](img/20190522_170807.png)

5. 檢查 \[MembershipTypes] table 確實有新增四筆資料

    ![20190522_170908](img/20190522_170908.png)

### Overriding Conventions

欲覆寫 DB column 的預設屬性，例如長度和不允許空值，可以透過 ```System.ComponentModel.DataAnnotations``` 命名空間，以下述操作為例

1. .NET Framework 的 ```string``` 型別在 SQL Server 中，預設會轉成 ```nvarchar(max)``` 而且允許 null
2. 以 \[Customers].\[Name] 為例

    ![20190522_172806](img/20190522_172806.png)

3. 在 ```Customer``` 類別中，在 ```Name``` 前面加上資料註解 (Data Annotation)

    ![20190522_173317](img/20190522_173317.png)

4. 建立移轉，並更新至 DB 檔案

    ![20190522_173638](img/20190522_173638.png)

5. 檢查 \[Customers].\[Name] 有限制長度且不允許 null

    ![20190522_173932](img/20190522_173932.png)

### Querying Objects

透過 EF 來查詢資料，以下述操作為例

1. 欲在「CustomersController.cs」中，將原本 hard code 的客戶改為由 DB 取得
2. 先透過伺服器總管在 \[Customers] 塞入兩筆資料

    > ※ 在此應用程式中，「客戶」並不是應用程式的一部份，無法事先定義，亦會不斷增加，所以不需透過程式碼和移轉的方式來新增資料

    ![20190522_181509](img/20190522_181509.png)

3. 在「CustomersController.cs」中，加入 ```ApplicationDbContext``` 類別的欄位 (Field) 和相關程式碼

    ![20190522_182450](img/20190522_182450.png)

4. 接著，原本 hard code 的地方全改成從 ```ApplicationDbContext``` 來取得資料

    > ※ 要注意的是，EF 並不是在一使用 ```ApplicationDbContext``` 就立刻作查詢，例如下圖的 ```_context.Customers``` 應不會立刻去查詢 \[Customers] 的資料，而是會等到真正要使用資料時，也就是傳到 View 在跑迴圈時才作查詢

    ![20190522_182932](img/20190522_182932.png)

    > ※ 在下圖中，因為 ```_context.Customers``` 緊接著 LINQ，馬上要使用真正的資料，所以在這行就會立刻作查詢

    ![20190522_183119](img/20190522_183119.png)

5. Ctrl+F5 檢查「/Customers」頁面有正常顯示

### Eager Loading

為了讓 EF 能夠載入相關的實體，我們需要 Eager Loading，以下述程式碼為例

1. 在下圖寫法中，EF 只會載入一個實體 (Entity)，也就是 ```Customers```

    ![20190522_182932](img/20190522_182932.png)

2. 但是實際上 ```Customer``` 類別還有另一個相關的實體 ```MembershipType```，就目前的寫法，我們無法取得其中的資料

    ![20190522_160803](img/20190522_160803.png)

3. 透過 Eager Loading，也就是 ```Include()``` 的方式，我們可以告訴 EF 要載入相關的 ```MembershipType``` 實體

    > ※ 使用 ```Include()``` 必須引用 ```System.Data.Entity```

    ![20190522_232224](img/20190522_232224.png)

4. 接著我們在「Views\Customers\Index.cshtml」就可以使用 ```MembershipType``` 中的資料

    |                    View                     |                    結果                     |
    |:-------------------------------------------:|:-------------------------------------------:|
    | ![20190522_232649](img/20190522_232649.png) | ![20190522_233008](img/20190522_233008.png) |

### Exercise in section 3

- 1st exercise
    1. 在「/Customers」頁面，顯示「客戶姓名」和「會員類型」
    2. 承上，會員類型必須是純文字內容

        ![20190523_002110](img/20190523_002110.png)

- 2nd exercise
    1. 在「/Customers/Details/*{id}*」頁面，顯示「會員類型」和「生日」
    2. 承上，「生日」無資料時不顯示

        ![20190523_001837](img/20190523_001837.png)

- 3rd exercise
    1. 在「/Movies」頁面顯示以下資料

        ![20190523_004023](img/20190523_004023.png)

    2. 承上，電影名稱可超連結至「/Movies/Details/*{id}*」，並在該頁顯示以下資料

        ![20190523_004135](img/20190523_004135.png)

## Building Forms

### The Markup

建立一個簡單的表單 (Form) 以新增客戶，以下述操作為例

1. 在 Controller 建立新的 Action

    ![20190524_005642](img/20190524_005642.png)

2. 在 ```return View();``` 按右鍵可以新增 View

    ![20190524_005808](img/20190524_005808.png)

3. 透過 ```@using (Html.BeginForm(actionName, controllerName)) { ... }``` 可以建立 ```<form></form>``` 元素

    > ※ 如果沒有使用 ```@using```，只有 ```@Html.BeginForm(actionName, controllerName)``` 的話，渲染出來會只有開頭的 ```<form>```，沒有結尾的 ```</form>```

    ![20190524_020652](img/20190524_020652.png)

4. 透過 ```@Html.LabelFor()``` 加入 Label、```@Html.TextBoxFor()``` 加入 TextBox

    > ※ ```form-group```, ```form-input``` 是 Bootstrap 的 [css 語法](https://getbootstrap.com/docs/3.4/css/#forms)
    >
    > ※ ```@Html.TextBoxFor()``` 預設會根據 Model 是否有設定長度等限制來設定驗證 (Validation) 屬性

    |        |                                             |
    |:------:|:-------------------------------------------:|
    | source | ![20190524_024602](img/20190524_024602.png) |
    | result | ![20190524_025632](img/20190524_025632.png) |
    |  html  | ![20190524_025601](img/20190524_025601.png) |

5. 加入更多 TextBox 和 CheckBox

    > ※ CheckBox 的結構也是參考 Bootstrap 文件的範例

    |        |                                             |
    |:------:|:-------------------------------------------:|
    | source | ![20190524_030644](img/20190524_030644.png) |
    | result | ![20190524_030437](img/20190524_030437.png) |
    |  html  | ![20190524_030606](img/20190524_030606.png) |

### Form Labels

如果要自訂 Label 文字，有以下兩種方式

|          |                                        使用 ```@Html.LabelFor()```                                        |                             使用純 html 的 ```<label></label>```                              |
|:--------:|:---------------------------------------------------------------------------------------------------------:|:---------------------------------------------------------------------------------------------:|
| 額外修改 | 須在 Model 加上資料註解 ```[Display(Name = "")]```，如下圖<br>![20190524_032103](img/20190524_032103.png) |             須用 ```<label for="">``` 的方式，才能在點擊文字時，欄位也獲得 focus              |
|   缺點   |                                          改動 Model 就要重新編譯                                          | ```for``` 的值是為欄位 id，也就是 Model 中的屬性，若屬性名稱有異動，得記得修改 ```for``` 的值 |

### Drop-down Lists

透過 ```@Html.DropDownListFor()``` 可以加入下拉選單，以下述程式碼為例

1. 欲在「/Customers/New」頁加入會員類型的下拉選單
2. 承上，該頁會同時用到 ```Customer``` 和 ```MembershipType``` 兩個 Model，所以要改成使用 [ViewModel](#view-models)

    ![20190524_155304](img/20190524_155304.png)

    ![20190524_155351](img/20190524_155351.png)

3. 因為在步驟 4 會需要直接透過 ```ApplicationDbContext``` 撈 ```MembershipType``` 的資料，所以先到「IdentityModels.cs」加上相關程式碼

    ![20190524_155825](img/20190524_155825.png)

4. 回到「CustomersController.cs」，在 ```New()``` 中建立 ViewModel 的實體並回傳給 View 使用

    ![20190524_160701](img/20190524_160701.png)

5. 回到 View，原本開頭是 ```@model Vidly.Models.Customer```，要改成使用 ViewModel，Razor 使用到 Model 的地方也都要修改

    ![20190524_164814](img/20190524_164814.png)

6. 加入下拉選單

    > ※ 用 ```SelectList(items, valueField, textField)``` 渲染下拉選單中的項目

    |        |                                             |
    |:------:|:-------------------------------------------:|
    | source | ![20190524_170303](img/20190524_170303.png) |
    | result | ![20190524_170442](img/20190524_170442.png) |
    |  html  | ![20190524_170547](img/20190524_170547.png) |

### Model Binding

按下 submit 按鈕將 View 的資料透過 Http Post 的方式傳回 Controller，以下述操作為例

1. 在 View 作出 submit 按鈕

    > ※ 黃色區塊中，我們當初有指定這個表單 (Form) submit 後要回傳給 ```Customers``` Controller 中的 ```Create``` Action

    ![20190524_172040](img/20190524_172040.png)

2. 在「CustomersController.cs」建立 ```Create``` Action

    > ※ ```[HttpPost]``` 用來限制此 Action 僅能處理來自 Http Post 的要求

    ![20190524_172529](img/20190524_172529.png)

#### 驗證 Http Post

1. 在 ```Create``` Action 下中斷點

    ![20190524_172902](img/20190524_172902.png)

2. F5 開始偵錯，到「Customers/New」頁填完資料後，先按下 F12 到「Network」頁籤

    ![20190524_173549](img/20190524_173549.png)

3. 按下表單的 Save 按鈕，執行到中斷點後，回來看瀏覽器的「Network」頁籤

    ![20190524_173844](img/20190524_173844.png)

4. 承上，可以看到 Post 方式送出的表單資訊

### Saving New Data

把 Controller 取得的資料存回 DB，以下述程式碼為例

1. 從剛剛 debug 時可以看出，```Create``` Action 接收的 ViewModel 都是屬於 ```Customer``` 類別的資料

    ![20190524_174636](img/20190524_174636.png)

2. 所以接收的參數類別可以從 ```NewCustomerViewModel```，改成 ```Customer```

    ![20190524_174949](img/20190524_174949.png)

3. 透過 ```ApplicationDbContext``` 新增客戶，並在完成後導至「/Customers」頁

    > ※ 一定要記得下 ```SaveChanges()``` 才會啟動交易

    ![20190524_175206](img/20190524_175206.png)

### Edit Form

在這章節要加上修改表單的功能，而且要和新增表單共用頁面，以下述操作為例

1. 原本在「/Customers」頁，點擊客戶姓名會導至「/Customers/Details/*{id}*」頁，我們改成導至「/Customers/Edit/*{id}*」頁

    > ※ 要記得這個 ActionLink 是有傳一個 ```Id``` 參數的

    ![20190524_180723](img/20190524_180723.png)

2. 建立 ```Edit``` Action，傳入 ```Id``` 參數

    > ※ 如果最後沒指定 View 名稱的話，MVC 會去找我們故意沒建的「Edit.cshtml」

    ![20190524_181724](img/20190524_181724.png)

3. 因為新增和修改表單都是使用同一個 View，所以我們修改所有的「New.cshtml」成「CustomerForm.cshtml」，相關程式碼也要修改

    ![20190524_182653](img/20190524_182653.png)

4. 同上，新增和修改表單都是使用同一個 ViewModel，所以修改所有的 ```NewCustomerViewModel``` 成 ```CustomerFormViewModel```

    > ※ 可以點擊任何一個 ```NewCustomerViewModel```，按下 F2 作全域修改

5. 最後，我們可以在 ```TextBoxFor()``` 使用指定日期格式的多載，讓日期格式符合我們的要求

    ![20190524_184257](img/20190524_184257.png)

    ![20190524_184435](img/20190524_184435.png)

### Updating Data

在新增表單 submit 後，我們使用 ```Create``` Action 去作新增資料到 DB 的動作。在修改表單 submit 後，也需要類似的動作儲存資料回 DB，以下述操作為例

1. 較簡潔的設計是讓新增和修改表單在 submit 後都使用同一個 Action，所以我們先調整 Form 要 post 的 Action 名稱

    > ※ 一樣可以用 F2 作全域修改

    ![20190524_190628](img/20190524_190628.png)

    ![20190524_190651](img/20190524_190651.png)

2. 在 Controller 會透過是否有 ```Customer.Id``` 來判斷這個 ```Save``` Action 是要新增還是修改，所以在 View 也要實作這個資訊

    > ※ ```Id``` 不需要讓使用者知道，所以用隱藏欄位 (Hidden Field)

    ![20190524_191043](img/20190524_191043.png)

3. 接著到 Controller，根據是否有 ```Customer.Id``` 來判斷要作新增或修改

    > ※ 另一種修改方式，在撈出 DB 中的狀態後，用 [```TryUpdateModel(model)```](https://docs.microsoft.com/zh-tw/dotnet/api/system.web.mvc.controller.tryupdatemodel?view=aspnet-mvc-5.2) 一次修改所有欄位，但是有以下問題須解決
    >   1. 安全性問題，來源的 Form data 有可能被竄改而異動到不該動的欄位
    >   2. 承上，需要額外下參數定義那些欄位可動、哪些不可動

    ![20190524_191556](img/20190524_191556.png)

### Exercise in section 4

1. 在「/Movies」頁面加入「New Movie」按鈕
2. 承上，點擊後進入以下表單

    ![20190527_114136](img/20190527_114136.png)

3. 在「/Movies」頁面點擊電影名稱，則會進入以下表單

    ![20190527_114254](img/20190527_114254.png)

4. 實作新增/修改電影的邏輯

## Implementing Validation

### Adding Validation

表單驗證 (Form Validation) 有三個簡單的步驟

1. 在 Model 使用資料註解 (Data Annotation)
2. 在 Controller 使用 ```ModelState.IsValid``` 來判斷是否符合上一步驟的資料註解
3. 在 View 透過 ```@Html.ValidationMessageFor()``` 顯示特定欄位不符合驗證時的警語

以下述操作為例

1. 在 ```Customer``` Model 已經有資料註解，限制此欄位必填且長度須小於255字元

    ![20190527_172716](img/20190527_172716.png)

2. 在 Controller 增加判斷式，如果沒通過驗證，就導回原本的表單頁面並帶入原本敲入的資訊

    ![20190527_173158](img/20190527_173158.png)

3. 在 View 加上警語

    ![20190527_173529](img/20190527_173529.png)

4. 在「/Customers/New」頁面故意不填 Name 欄位就送出表單，會被導回「/Customers/New」頁面並顯示警語

    ![20190527_173806](img/20190527_173806.png)

### Styling Validation Form

驗證失敗時，欄位的 css class 會被加上 ```input-validation-error```，警語則會加上 ```field-validation-error```

![20190527_174634](img/20190527_174634.png)

所以可以在 css 檔案中自定義這兩個 class 的語法

![20190527_175149](img/20190527_175149.png)

結果就如下圖

![20190527_175223](img/20190527_175223.png)

### Data Annotations for Validation

以下為常見的資料註解 (Data Annotation)

|                屬性                |                     描述                     |
|:----------------------------------:|:--------------------------------------------:|
|            \[Required]             |                 是否符合必填                 |
|     \[StringLength(*length*)]      |               是否符合長度限制               |
|       \[Range(*min*, *max*)]       |                 是否符合範圍                 |
|    \[Compare(*otherProperty*)]     | 跟另一個欄位作比較，例如用於二次確認密碼欄位 |
|              \[Phone]              |             是否符合電話號碼格式             |
|          \[EmailAddress]           |             是否符合 Email 格式              |
|               \[Url]               |              是否符合 Url 格式               |
| \[RegularExpression(*expression*)] |              是否符合正則表達式              |

也可以用來自定義驗證失敗的警語文字

![20190527_180731](img/20190527_180731.png)

### Custom Validation

透過繼承 ```ValidationAttribute``` 類別，並覆寫 (Override) ```IsValid()``` 方法，我們可以添加新的資料註解且用於驗證自定義的邏輯，以下述操作為例

1. 假設「Pay as You Go」以外的會員資格皆須年滿18歲才能申請
2. 先在 Model 建出 ```ValidationAttribute``` 的衍生類別，並覆寫 ```IsValid()``` 方法

    ![20190528_125849](img/20190528_125849.png)

    ![20190528_130448](img/20190528_130448.png)

3. 自定義驗證邏輯

    > ※ 使用 ```validationContext.ObjectInstance``` 取得欲驗證的物件 (須轉型)
    >
    > ※ 驗證成功回傳 ```return ValidationResult.Success;```
    >
    > ※ 驗證失敗回傳 ```return new ValidationResult(errorMessage);```

    ![20190528_160235](img/20190528_160235.png)

4. 在 Model 加上剛剛建立的資料註解屬性

    ![20190528_160656](img/20190528_160656.png)

5. 在 View 也要加上警語的 placeholder

    ![20190528_160925](img/20190528_160925.png)

#### Refactoring Magic Numbers

這裡的 magic number 指的是在下圖邏輯中的 0 和 1

![20190528_161727](img/20190528_161727.png)

因為 0 和 1 代表的意義必須翻資料庫才能知道，其它工程師也不見得知道要查哪一張 table，較好的作法是將 0 和 1 兩個數字改寫成有意義的程式碼，可以透過以下兩種方式達成

1. 建立 ```MembershipType.Id``` 屬性對應的[列舉 (enum)](C%23%20Basics.md#enum) 類別
2. 在 ```MembershipType``` 類別建立 ```public static readonly``` 的欄位 (Field)

    |              MembershipType.cs              |           Min18YearsIfAMember.cs            |
    |:-------------------------------------------:|:-------------------------------------------:|
    | ![20190528_162710](img/20190528_162710.png) | ![20190528_163109](img/20190528_163109.png) |

### Validation Summary

在 View 透過 ```@Html.ValidationSummary()``` 可以顯示不符合驗證時的警語結論，以下述操作為例

1. 在 View 的 Form 元素一開始就加入 ```@Html.ValidationSummary()```，便可以看到警語結論

    ![20190528_164504](img/20190528_164504.png)

    ![20190528_164709](img/20190528_164709.png)

2. 承上，會看到 hidden field 的 ```Id``` 欄位也驗證失敗是因為我們的 ```New``` Action 並沒有給 ```Customer.Id``` 值

    ![20190528_164954](img/20190528_164954.png)

3. 所以我們可以在 ```New``` Action 時，就先對 ```Customer``` 初始化，此時 ```Customer.Id``` 也會被給予預設值，就不會有後續的驗證失敗問題

    ![20190528_165205](img/20190528_165205.png)

4. 另外，也可以透過 ```@Html.ValidationSummary(true, message)``` 的多載自定義警語結論文字

    ![20190528_165709](img/20190528_165709.png)

### Client-side Validation

截至目前為止都是使用 Server-side 的驗證，通常安全性驗證和自定義驗證 (例如18歲才能辦會員) 會交給 Server-side，而基本格式的驗證則交給 Client-side

<table>
    <tr>
        <th colspan="2">Client-side Validation</th>
    </tr>
    <tr>
        <th>優點</th>
        <th>缺點</th>
    </tr>
    <tr>
        <td>
            <ul>
                <li>立即回饋，不必等到 Server-side 處理完才給回饋</li>
                <li>不需浪費 Server-side 的資源</li>
            </ul>
        </td>
        <td align="center">無法驗證自定義的資料註解 (Data Annotation) 屬性</td>
    </tr>
</table>

#### 啟用 Client-side Validation

1. 在「App_Start\BundleConfig.cs」可以看到 MVC 專案預設會將「jquery.validate*」等 script 打包成一支「jqueryval.js」

    ![20190528_171418](img/20190528_171418.png)

2. 但是在主版頁面「Views\Shared\_Layout.cshtml」的 scripts section 是不啟用的

    ![20190528_171829](img/20190528_171829.png)

3. 所以我們可以在欲使用 Client-side Validation 的 View 中自行啟用

    > ※ ```@section scripts``` 用來表示其中的內容要在主版頁面的 scripts section 呈現
    >
    > ※ ```@Scripts.Render(path)``` 則是用來宣告欲使用的 script 檔案

    ![20190528_172331](img/20190528_172331.png)

4. 檢查 Client-side Validation 是否確實啟用

    1. 在「/Customers/New」頁面，按下 F12 打開「Network」頁籤
    2. 按下 submit 按鈕，如果「Network」頁籤沒有出現任何 request 但表單已經出現驗證警語，代表是使用 Client-side Validation 驗證的

5. 承上，驗證原理是透過欄位的 ```data-val-*``` 屬性，此屬性是 MVC Framework 根據 Model 的資料註解加上的

    ![20190528_173417](img/20190528_173417.png)

### CSRF (Cross-site Request Forgery) 攻擊

CSRF 攻擊的原理如下

1. 使用者在 A 網站 (例如網路銀行) 登入後，未登出即瀏覽其它網站
2. 駭客在 B 網站的圖片或連結中埋 script，其內容會對 A 網站送出 post request
3. 使用者只要一瀏覽 B 網站就會中標，駭客可以任意竄改甚至刪除使用者在 A 網站上的資料 (例如把錢都轉走)
4. 承上，對 A 網站來說，這是使用者發出的 request，但卻**不是自願發出**的

> ※  [CSRF 攻擊](https://zh.wikipedia.org/wiki/%E8%B7%A8%E7%AB%99%E8%AF%B7%E6%B1%82%E4%BC%AA%E9%80%A0)和 [XSS 攻擊](https://zh.wikipedia.org/wiki/%E8%B7%A8%E7%B6%B2%E7%AB%99%E6%8C%87%E4%BB%A4%E7%A2%BC)不同

#### Anti-forgery Tokens

MVC Framework 透過兩個步驟來確保 request 是發自本身的網站表單

1. 在有表單的頁面加上 ```@Html.AntiForgeryToken()```

    ![20190528_181100](img/20190528_181100.png)

2. 在 Controller 的 post action 加上 ```[ValidateAntiForgeryToken]``` 屬性

    ![20190528_181311](img/20190528_181311.png)

背後邏輯如下

1. 在我們瀏覽此頁面時，MVC Framework 會藏一個 token 在 hidden field 中

    ![20190528_181557](img/20190528_181557.png)

2. 並且在 client side 也存一個加密過的 cookie

    ![20190528_181952](img/20190528_181952.png)

3. 因為駭客頂多可以偷到瀏覽器的 cookie，但是沒辦法知道原網頁中的 token，所以當我們送出 request 時，MVC Framework 就會去檢查兩個值是否一致，若一致就代表是在這個網站送出的 request，否則就有可能是 CSRF 攻擊

### Exercise in section 5

1. 在「/Movies/New」頁面加入驗證功能

    ![20190528_183040](img/20190528_183040.png)

2. 承上，其中「Number in Stock」欄位有其範圍限制

    ![20190528_183118](img/20190528_183118.png)

3. 「Release Date」和「Number in Stock」欄位必須有初始值
4. 啟用 Client-side Validation
5. 實作 Anti-forgery Token

#### 處理表單欄位初始值

為了在新增表單時能通過驗證，我們在 ```New``` Action 中初始化了一個 ```Movie``` 實體，使 ```Id``` 能夠擁有初始值

![20190529_121858](img/20190529_121858.png)

但是這會造成其它 Value Type 的欄位也跑出初始值，如下圖

![20190529_121636](img/20190529_121636.png)

為了解決這個問題，我們可以遵循以下原則

1. 將**所有實際儲存於 DB** 的欄位及特性寫在 Model，例如 ```[Required]``` (for not null column), ```[StringLength]``` (for column length) .. 等等
2. 將**欲呈現在 View** 的欄位及特性寫在 ViewModel，例如 ```[Required]``` (for form validation), ```[StringLength]``` (for form validation), ```[Range]```, ```[Display]``` .. 等等

以剛剛電影的新增表單為例

1. 將 ViewModel 中原本的 ```Movie``` 屬性，替換成所有會在 View 中使用的 ```Movie``` 類別中的屬性

    ![20190529_124647](img/20190529_124647.png)

2. 在 ViewModel，就可以把 ```ReleaseDate``` 和 ```NumberInStock``` 兩個欄位改為 Nullable
3. 在 ViewModel 加上建構式，讓日後的 ```New``` 和 ```Edit``` Action 使用

    ![20190529_125034](img/20190529_125034.png)

4. 回去調整 Controller，有使用到 ViewModel 的地方改用相對應的建構式
5. 回去調整 View，有使用到 ViewModel 的地方都要改

## Building RESTful Services with ASP&#46;NET Web API

### What is a Web API

先前的 MVC 策略都是在 Server-side 作好 Html 再回傳給 Client-side

![20190529_164713](img/20190529_164713.png)

有另一種替代方案是 Server-side 僅回傳 Data，Client-side 再根據 Data 渲染 Html

![20190529_164810](img/20190529_164810.png)

用第二種方案的優點如下

- 使用較少的 Server-side 資源 (增加可擴充性)
- 減少資料傳輸量 (增加效能)
- 能夠支援 Web Application 以外的 Client-side

    ![20190529_165432](img/20190529_165432.png)

這種策略就是透過「ASP&#46;NET Web API」framework 在 Server-side 提供 Web API

![20190529_165606](img/20190529_165606.png)

很多知名網站，例如 Facebook, YouTube, Twitter .. 等等，也會提供公開的 Web API 供他人使用，且 Web API 不僅是可以用來查詢，也可以用來修改、刪除資料

### RESTful Convention

RESTful API 即遵守 [REST (Representational State Transfer)] 風格的 Web API

<table>
    <tr>
        <th>HTTP Verbs</th>
        <th>描述</th>
        <th colspan="2">範例</th>
    </tr>
    <tr align="center">
        <td rowspan="2">GET</td>
        <td rowspan="2">取得資料</td>
        <td><b>GET</b> /api/customers</td>
        <td>取得所有客戶資料</td>
    </tr>
    <tr align="center">
        <td><b>GET</b> /api/customers/1</td>
        <td>取得 id = 1 的客戶資料</td>
    </tr>
    <tr align="center">
        <td>POST</td>
        <td>新增資料</td>
        <td><b>POST</b> /api/customers</td>
        <td>新增所有客戶資料</td>
    </tr>
    <tr align="center">
        <td>PUT</td>
        <td>修改資料</td>
        <td><b>PUT</b> /api/customers/1</td>
        <td>修改 id = 1 的客戶資料</td>
    </tr>
    <tr align="center">
        <td>DELETE</td>
        <td>刪除資料</td>
        <td><b>DELETE</b> /api/customers/1</td>
        <td>刪除 id = 1 的客戶資料</td>
    </tr>
</table>

### Building an API

建立操作 Customers 資料的 API，以下述操作為例

1. 建立「Controllers\Api」資料夾
2. 承上，在該資料夾中加入「CustomersController.cs」，這次我們選擇「Web API 2 控制器」

    ![20190529_175200](img/20190529_175200.png)

3. 首次使用 Web API 會開啟「readme.txt」，依照說明在「Global.asax」添加程式碼

    ![20190529_175617](img/20190529_175617.png)

4. 回到「Controllers\Api\CustomersController.cs」，加入 ```ApplicationDbContext``` 類別的欄位 (Field) 和相關程式碼

    ![20190529_180537](img/20190529_180537.png)

5. 加入供 GET request 使用的方法

    > ※ RESTful API 規定 exception 要用 ```throw new HttpResponseException(HttpStatusCode.{element});``` 的方式來回傳，在這裡的錯誤是找不到資料，所以用 ```HttpStatusCode.NotFound```

    ![20190529_190836](img/20190529_190836.png)

6. 加入供 POST request 使用的方法

    > ※ 要記得添加給 API Controller 用的 ```[System.Web.Http.HttpPost]``` 屬性，順帶一提， ```[System.Web.MVC.HttpPost]``` 是給 MVC Controller 用的，小心搞混
    >
    > ※ 這裡的錯誤會是無效的 ```Id```，所以用 ```HttpStatusCode.BadRequest```

    ![20190529_190910](img/20190529_190910.png)

7. 加入供 PUT request 使用的方法

    > ※ 要記得添加給 API Controller 用的 ```[System.Web.Http.HttpPut]``` 屬性

    ![20190529_191713](img/20190529_191713.png)

8. 加入供 DELETE request 使用的方法

    ![20190529_191847](img/20190529_191847.png)

### Testing an API

如果直接用瀏覽器開啟我們的 Web API，預設會是 xml 格式的 data

![20190529_223519](img/20190529_223519.png)

較好的作法是透過 [Postman](https://chrome.google.com/webstore/detail/postman/fhbjgbiflinjbdggehcddcbncdddomop) 套件，直接對 API 發出 request，以下述操作為例

1. 選擇 Http Verbs，輸入 URL 後，按下「Send」，即可取得 json 格式的 data

    ![20190529_224201](img/20190529_224201.png)

2. 如果想測 POST request，可以在「Body」頁籤輸入資料，然後按下「Send」

    ![20190529_224647](img/20190529_224647.png)

3. 承上，如果收到 media type 的錯誤，可以去「Headers」頁籤添加「Content-Type」的值為「application/json」

    ![20190529_224824](img/20190529_224824.png)

    ![20190529_225104](img/20190529_225104.png)

### 資料傳輸物件 (Data Transfer Objects)

讓 API 直接回傳 domain model 物件有以下風險

1. 修改 domain model 時會造成依賴於現行 API 的程式崩潰，例如重新命名 ```Customers``` 的屬性名稱
2. 將 domain model 的邏輯整個公開，恐遭有心人士惡意攻擊，例如修改不應異動的欄位

我們可以透過 DTO (Data Transfer Objects) 僅揭露 domain model 中的必要資訊給 client，以下述操作為例

1. 建立「Dtos」資料夾和「Dtos\CustomerDto.cs」檔案

    ![20190529_230940](img/20190529_230940.png)

2. 將「Models\Customer.cs」的屬性全部複製過來，然後刪掉所有 client 端不需要知道的屬性和資訊

    ![20190529_233432](img/20190529_233432.png)

3. ```CustomerDto``` 最後只保留 client 需要知道的內容

    ![20190529_233556](img/20190529_233556.png)

4. 接著回到 API，將所有使用到 Model，也就是用到 ```Customer``` 類別的地方，都改成使用 ```CustomerDto``` 類別，下個章節會介紹用 Auto Mapper 重新作欄位對應

### Auto Mapper

Auto Mapper 能自動在兩個對應的類別間作轉換，例如 Model <-> ViewModel 或是 Model <-> DTO，可省去一一條列 ```Class1.Attribute = Class2.Attribute``` 的動作，以下述操作為例

1. 開啟套件管理器主控台 (Package Manager Console)，並輸入 ```Install-Package AutoMapper```

    ![20190530_000746](img/20190530_000746.png)

2. 建立「App_Start\MappingProfile.cs」，為類別對應的設定檔

    ![20190530_001234](img/20190530_001234.png)

3. 承上，先繼承 ```Mapper``` 類別，然後在建構式中設定類別對應

    > ※ 不同版本會有不同寫法，此處為 8.1.0 版，語法為 ```CreateMap<TSource, TDestination>();```

    ![20190530_001742](img/20190530_001742.png)

4. 接著，修改「Global.asax」，讓應用程式一啟動就會載入這個設定檔

    > ※ 初始化並載入設定檔 ```Mapper.Initialize(c => c.AddProfile<profileName>());```

    ![20190530_002423](img/20190530_002423.png)

5. 再來就可以回到 API，將用到 ```Customer``` 類別的地方，都改成使用 ```CustomerDto``` 類別，並搭配 AutoMapper 作欄位對應
6. 調整 GET request

    > ※ 如果是物件集合，可以透過 [LINQ](C%23%20Advanced.md#linq) 用 ```.Select(Mapper.Map<TSource, TDestination>)``` 產生目標類別的集合
    >
    > ※ 如果是單一物件，可以透過 ```Mapper.Map<TSource, TDestination>(sourceClass)``` 產生目標類別

    ![20190530_003406](img/20190530_003406.png)

7. 調整 POST request

    > ※ ```Id``` 欄位會在新增進 DB 後有值，要更新回 ```customerDto``` 再回傳

    ![20190530_004416](img/20190530_004416.png)

8. 調整 PUT request

    > ※ 透過 ```Mapper.Map<TSource, TDestination>(sourceClass, destinationClass)```，或是簡化成 ```Mapper.Map(sourceClass, destinationClass)``` 可以將來源類別更新至目標類別，而且不必條列欄位

    ![20190530_005003](img/20190530_005003.png)

### Enable Camel Case

目前 API 產生的 json，其屬性名稱是 Pascal Case

![20190530_150234](img/20190530_150234.png)

但為了後續讓 javascript 便於使用 data，我們應該改成 Camel Case，以下述操作為例

1. 開啟「App_Start\WebApiConfig.cs」
2. 加上以下程式碼

    ![20190530_150951](img/20190530_150951.png)

### IHttpActionResult

目前我們的 GET 和 POST request 都是回傳自定義的類別，在[官方文件](https://docs.microsoft.com/zh-tw/aspnet/web-api/overview/getting-started-with-aspnet-web-api/action-results)寫到，如果回傳的類別不為 ```void```、```HttpResponseMessage``` 或 ```IHttpActionResult``` 其中一種，就會是回傳 [HTTP 狀態碼](https://zh.wikipedia.org/wiki/HTTP%E7%8A%B6%E6%80%81%E7%A0%81) 200

![20190530_153900](img/20190530_153900.png)

![20190530_160848](img/20190530_160848.png)

但根據 RESTful API 的規則，我們在 POST request 建立資料後，應該要回傳 HTTP 狀態碼 201 且提供該筆新資料的 Uri，在 ASP<span>.</span>NET Web API 中建議我們使用 ```IHttpActionResult``` 達成，以下述操作為例

1. 調整 POST request

    > ※ 透過 ```Created()``` helper method 可以回傳 201 狀態碼
    >
    > ※ 丟例外 ```HttpResponseException``` 也有更簡潔的寫法

    ![20190530_161330](img/20190530_161330.png)

    實際呼叫可以看到

    1. HTTP 狀態碼已經變成 201
    2. 回傳結果的 Header 多了 location 屬性並指向該筆新資料的 Uri

    ![20190530_161509](img/20190530_161509.png)

2. 調整 GET request

    > ※ 透過 ```Ok()``` helper method 可以回傳 200 狀態碼

    ![20190530_162300](img/20190530_162300.png)

### Exercise in section 6

加入對 ```Movie``` 操作 CRUD 的 API

## Client-side Development

### Calling an API Using jQuery

> ※ 沒學過 [jQuery](https://api.jquery.com/) 的話可能要先去稍微了解一下

在「Customers/Index」頁面建立「刪除」按鈕，並透過 jQuery 語法，用 Ajax 跟 API 發出 request，以下述操作為例

1. 加入按鈕和 script section

    > ※ ```btn-link``` 是 Bootstrap 的語法

    ![20190531_171849](img/20190531_171849.png)

2. 用 jQuery 監聽所有刪除按鈕的 click event

    > ※ ```table``` 元素加上 id、按鈕加上 class，都是為了方便寫 jQuery 的選擇器 (Selector)
    >
    > ※ 寫 js 時，可以透過 ```console.log()``` 來 debug，在 Chrome 按 F12，「Console」頁籤可以看到輸出結果

    ![20190531_172939](img/20190531_172939.png)

3. click event 觸發時，跳出 confirm 視窗，確認後用 Ajax 發出 DELETE request，接著刪除該行 tr 元素

    > ※ ```const``` 是 javascript ES 6 的語法
    >
    > ※ ```.data()```, ```.parents()```, ```.remove()``` 都是 jQuery 的語法

    ![20190531_191738](img/20190531_191738.png)

### Bootbox Plug-in

Bootstrap 風格的彈跳視窗套件，欲替換原生的 confirm 視窗，以下述操作為例

1. 用 NuGet 安裝 [Bootbox](http://bootboxjs.com/)
2. 在「App_Start\BundleConfig.cs」加入「~/Scripts/bootbox.js」為 bundle 的一部份

    ![20190531_194314](img/20190531_194314.png)

3. 改寫 jQuery 中原本使用 ```confirm()``` 的地方成 ```bootbox.confirm()```

    ![20190531_194923](img/20190531_194923.png)

4. 測試結果

    ![20190531_194745](img/20190531_194745.png)

### Optimizing jQuery Code

目前的 code 是將 event handler 綁在所有按鈕上

``` javascript
$("#customers .js-delete").on("click", function () {
    ...
});
```

Mosh 說，如果你有 100 個按鈕，上述寫法就代表你要在記憶體中存 100 個 event handler，較好的寫法是將 event handler 寫在按鈕的共同 parent 上，並用 ```.on()``` 去過濾出符合條件的選擇器，如下述程式

``` javascript
$("#customers").on("click", ".js-delete", function () {
    ...
});
```

參考文章：

- [Should all jquery events be bound to $(document)?](https://stackoverflow.com/questions/12824549/should-all-jquery-events-be-bound-to-document)
- [Why not take Javascript event delegation to the extreme?](https://stackoverflow.com/questions/9711118/why-not-take-javascript-event-delegation-to-the-extreme)

### DataTables Plug-in

表格套件，內建許多功能，例如排序、篩選、分頁 .. 等等，欲替換「/Customers/Index」頁面的表格，以下述操作為例

1. 用 NuGet 安裝 [DataTables](https://datatables.net/)
2. 在「App_Start\BundleConfig.cs」加入
    - 「~/Scripts/DataTables/jquery.dataTables.js」
    - 「~/Scripts/DataTables/dataTables.bootstrap.js」
    - 「~/Content/DataTables/css/dataTables.bootstrap.css」
3. 在 ```$(document).ready()``` 後套用 DataTables

    > ※ 如果沒作用，可以 F12 看「Console」頁籤是否有 error

    ``` javascript
    $(document).ready(function () {
        $("#customers").DataTable();

        $("#customers").on("click", ".js-delete", function () {
            ...
        });
    });
    ```

4. 檢視結果

    ![20190601_005802](img/20190601_005802.png)

### DataTables with Ajax Source

在剛剛的例子中，其實是很浪費 Server-side 資源的

1. 我們先在 Server-side 作好 Html (包含表格)
2. 傳到 Client-side
3. DataTables 元件讀取表格的 Html，再套用 DataTables

較好的作法是

1. 我們先在 Server-side 作好表格以外的 Html
2. 傳到 Client-side
3. DataTables 元件用 Ajax 去要表格的 data (json 格式)，用 data 作出 DataTables 的表格

以下述操作為例

1. 在套用 DataTables 時，加上初始化屬性

    ``` javascript
    $(document).ready(function () {
        $("#customers").DataTable({
            ajax: {
                url: "/api/customers",
                dataSrc: ""
            },
            columns: [
                {
                    data: "name",
                    render: function (data, type, row) {
                        return "<a href='/Customers/Edit/" + row.id + "'>" + data + "</a>";
                    }
                },
                {
                    // this data is temporary
                    data: "name"
                },
                {
                    data: "id",
                    render: function (data) {
                        return "<button class='btn-link js-delete' data-customer-id='" + data + "'>Delete</button>";
                    }
                }
            ]
        });
        ...
    });
    ```

    > ※ ```dataSrc``` 會依資料來源的格式給予不同的值
    >
    >> <table>
    >> <tr align="center">
    >> <th>資料來源</th>
    >> <th>DataTables 初始化</th>
    >> </tr>
    >> <tr>
    >> <td>
    >>
    >> ``` json
    >> {
    >>     count: 10,
    >>     customers: [
    >>         { ... },
    >>         { ... },
    >>         { ... }
    >>     ]
    >> }
    >> ```
    >>
    >> </td>
    >> <td>
    >>
    >> ``` javascript
    >> $("#customers").DataTable({
    >>     ajax: {
    >>         url: "/api/customers",
    >>         dataSrc: "customers"
    >>     },
    >>     ...
    >> });
    >> ```
    >>
    >> </td>
    >> </tr>
    >> <tr>
    >> <td>
    >>
    >> ``` json
    >> [
    >>     { ... },
    >>     { ... },
    >>     { ... }
    >> ]
    >> ```
    >>
    >> </td>
    >> <td>
    >>
    >> ``` javascript
    >> $("#customers").DataTable({
    >>     ajax: {
    >>         url: "/api/customers",
    >>         dataSrc: ""
    >>     },
    >>     ...
    >> });
    >> ```
    >>
    >> </td>
    >> </tr>
    >> </table>
    >
    > ※ ```columns``` 用來定義表格欄位的值來自哪個屬性

2. 原本的 Html 只需要保留 table 的基礎架構，資料就交給 DataTables 來渲染

    ``` html
    <h2>Customers</h2>
    <table id="customers" class="table table-bordered table-hover">
        <thead>
            <tr>
                <th>Customer</th>
                <th>Membership Type</th>
                <th>Delete</th>
            </tr>
        </thead>
        <tbody>
            <!-- everything here is deleted -->
        </tbody>
    </table>
    ```

3. Controller 的 ```Index()``` Action 也不再需要回傳 Model 物件

    ``` csharp
    // GET: Customers
    public ActionResult Index()
    {
        // everything here is deleted
        return View();
    }
    ```

4. 檢視結果

### Returning Hierarchical Data

在剛剛的例子中，因為 ```CustomerDto``` 類別中沒有 ```MembershipType``` 的屬性，所以在表格中我們無法用現有的 API 取得 ```MembershipType.Name```，為了解決這個問題，以下述操作為例

1. 建立 ```MembershipTypeDto``` 類別，目的只是為了讓 API 能夠揭露 ```MembershipType``` 的必要屬性

    ``` csharp
    public class MembershipTypeDto
    {
        public byte Id { get; set; }
        public string Name { get; set; }
    }
    ```

2. 在 ```CustomerDto``` 加入 ```MembershipTypeDto``` 屬性

    ![20190601_022901](img/20190601_022901.png)

3. 在 API 的 GET request method 加上 ```.Include()```

    ![20190601_023403](img/20190601_023403.png)

4. 也要在 「App_Start\MappingProfile.cs」，加上 ```MembershipType``` -> ```MembershipTypeDto``` 的對應

    ![20190601_023851](img/20190601_023851.png)

5. 用 Postman 檢查 API 是否有正確回傳 ```MembershipTypeDto```

    ![20190601_024023](img/20190601_024023.png)

6. 修改 script，定義成正確的屬性

    ![20190601_024147](img/20190601_024147.png)

7. 檢視結果

### DataTables: Removing Records

修正「刪除表格列」的寫法，以下述程式碼為例

> ※ ```draw()``` 是為了在刪除資料後，重新渲染表格

![20190601_152902](img/20190601_152902.png)

### Single Page Applications (SPAs)

user 點擊任何連結或發出 request 時

- Server-side 僅提供各種 API 來取得或修改資料
- 網站只有一個 View，而且全交由 Client-side 渲染 (不再需要 Razor view)

    ![20190601_154000](img/20190601_154000.gif)

- 對使用者體驗會更快更流暢

### Exercise in section 7

在「/Movies/Index」頁面實作 DataTables 和刪除的 Bootbox 功能

## Authentication and Authorization

目前的應用程式並沒有作權限管理，也就是應該要有以下限制

1. 有登入帳號的使用者才能租借電影
2. 有管理員帳號的使用者才能作新增、修改、刪除

### Authentication Options

在 Visual Studio 建立 MVC 專案時，有一個驗證的選項

![20190521_233504](img/20190521_233504.png)

其中包括

- 無驗證：不作權限控管
- 個別使用者帳戶：帳號儲存在獨立的 DB 或透過社群帳號登入 (eg:Facebook, Google, Twitter .. etc.)
- 工作或學校帳戶：透過將帳戶儲存在 Active Directory (AD) 中，達到 Single Sign-on (SSO) 的功能
- Windows 驗證：登入 Windows OS 時就完成授權，適用於組織內部網路

![20190601_175355](img/20190601_175355.png)

### ASP&#46;NET Identity

內建的 Identity Framework 由三個部分構成，如下圖所示 (灰字為部分類別)，用來管理基本的帳號註冊、驗證、登入

![20190601_182318](img/20190601_182318.png)

第一次使用 EF Migration 時，會跑出很多「dbo.AspNetUser*」開頭的 table，就是來自「Models\IdentityModels.cs」的預設類別，兩者皆為 ASP&#46;NET Identity 的一部份

![20190601_182949](img/20190601_182949.png)

打開「Controllers\AccountController.cs」，就可以看到一些註冊和登入用的 Action，例如 ```Register()``` 就可以調整註冊後是否要發 email、註冊後是否自動登入 .. 等等邏輯

![20190601_183455](img/20190601_183455.png)

### Restricting Access

- ```[Authorize]``` 屬性用來限制 Action 或整個 Controller 需要登入才能使用
- ```[AllowAnonymous]``` 屬性用來允許匿名使用 Action 或整個 Controller
- 以下述程式碼為例，就是整個 ```CustomersController``` 中的 Action 都要登入才能使用，但是 ```Index()``` Action 允許匿名使用

    ``` csharp
    [Authorize]
    public class CustomersController : Controller
    {
        ...

        // GET: Customers
        [AllowAnonymous]
        public ActionResult Index()
        {
            return View();
        }

        ...
    ```

- 如果要全域使用，需要在「App_Start\FilterConfig.cs」加上程式碼

    ![20190602_153221](img/20190602_153221.png)

### Seeding Users and Roles

欲產生 admin 和 guest 帳號，以下述操作為例

1. 先透過 Web 建立「guest@vidly.com」帳號
2. 註冊完會自動登入，先作登出
3. 在 ```Register()``` Action 暫時加入程式碼，讓下一個註冊者自動擁有 "CanManageMovie" 角色 (角色名稱最好為實際功能)

    ![20190602_162158](img/20190602_162158.png)

4. 透過 Web 建立「guest@vidly.com」帳號
5. 刪除在步驟3建立的程式碼
6. 建立空的 Migration，把「AspNetRoles」、「AspNetUsers」和「AspNetUserRoles」的 insert script 複製到 Migration 中

    ![20190602_163831](img/20190602_163831.png)

7. 刪除步驟6那三張 table 的資料，跑 ```Update-Database``` 指令
8. 承上，這樣的好處是，可以把相關 SQL script 帶到其它環境建置

### Working with Roles

欲讓 admin 和 guest 看到不同功能的「/Movies/Index」頁面，以下述操作為例

1. 將「View\Movies\Index.cshtml」重新命名為「List.cshtml」，並複製檔案命名為「ListReadOnly.cshtml」
2. 移除「ListReadOnly.cshtml」的新增、修改、刪除功能
3. 到「Controllers\MoviesController.cs」修改 ```Index()``` Action，透過 ```User.IsInRole()``` 來判斷要將使用者導向「List.cshtml」或是「ListReadOnly.cshtml」

    ![20190602_171658](img/20190602_171658.png)

4. 承上，為了將字串 "CanManageMovies" 統一管理，我們可以在 Model 新增 ```RoleName``` 類別

    ![20190602_172004](img/20190602_172004.png)

5. 將步驟3的字串替換成步驟4建立的類別屬性

    ![20190602_172118](img/20190602_172118.png)

6. 截至目前，guest 還是可以透過 URL 直接進入新增、修改表單頁，所以我們透過 ```[Authorize(Role = {nameOfRole})]``` 屬性來限制相關的 Action

    ``` csharp
    [Authorize(Roles = RoleName.CanManageMovies)]
    public ActionResult New()
    {
        ...
    }

    [Authorize(Roles = RoleName.CanManageMovies)]
    public ActionResult Edit(int Id)
    {
        ...
    }

    [HttpPost]
    [ValidateAntiForgeryToken]
    [Authorize(Roles = RoleName.CanManageMovies)]
    public ActionResult Save(Movie movie)
    {
        ...
    }
    ```

### Adding Profile Data

欲在註冊頁加上「駕照號碼」欄位，以下述操作為例

1. 從 Model 開始改，在「Models\IdentityModels.cs」的 ```ApplicationUser``` 添加屬性

    ![20190602_200603](img/20190602_200603.png)

2. 承上，調整 Model 就要建立新的 Migration，然後更新至 DB
3. 接著欲修改 View，可以看到「Views\Account\Register.cshtml」在一開始宣告 model type 是來自 ```RegisterViewModel```

    ``` csharp
    @model Vidly.Models.RegisterViewModel
    ...
    ```

4. 所以也要在 ```RegisterViewModel``` 添加屬性 (這裡的修改是為了在 View 中顯示欄位，而步驟 1 的修改是為了在 EF 中新增欄位)

    ![20190602_202024](img/20190602_202024.png)

5. 在「Register.cshtml」添加相關 razor 語法

    ``` csharp
    ...
    <div class="form-group">
        @Html.LabelFor(m => m.DrivingLicense, new { @class = "col-md-2 control-label" })
        <div class="col-md-10">
            @Html.TextBoxFor(m => m.DrivingLicense, new { @class = "form-control" })
        </div>
    </div>
    ...
    ```

### OAuth

大部分的社群登入會透過 Open Authorization (OAuth) 的機制來取得驗證，以透過 Facebook 帳號登入 Vidly 為例，其原理如下

![20190603_025400](img/20190603_025400.gif)

| 順序 |                       使用者端                       |                                                   背景邏輯                                                   |
|:----:|:----------------------------------------------------:|:------------------------------------------------------------------------------------------------------------:|
|  1   |     使用者在 Vidly 點擊「用 Facebook 帳號登入」      |                                                                                                              |
|  2   |                                                      |                                Vidly 拿著 key 和 secret 向 Facebook 發出要求                                 |
|  3   | Facebook 要求使用者登入，並告知 Vidly 欲存取哪些權限 |                                                                                                              |
|  4   |                    使用者同意存取                    |                                                                                                              |
|  5   |                                                      |                   Facebook 確認有此使用者且其允許存取後，回傳 authorization token 給 Vidly                   |
|  6   |                                                      | Vidly 再次拿著 authorization token、key 和 secret 向 Facebook 確認這個 authorization key 的確是來自 Facebook |
|  7   |                                                      |                       Facebook 確認 authorization token 來自本身後，回傳 access token                        |
|  8   |                                                      |                        Vidly 用 access token 跟 Facebook 取得該使用者的 email 等資料                         |

### Social Logins

在 ASP&#46;NET MVC 中要使用社群登入要先作兩件事

#### 開啟 SSL

以確保 OAtuh 的過程是安全加密的，以下述操作為例

1. 在「方案總管」選擇專案檔，接著點擊「屬性」視窗

    ![20190603_033102](img/20190603_033102.png)

2. 將 SSL 設定為啟用，啟用後會一併出現 SSL URL，複製該 URL

    ![20190603_033148](img/20190603_033148.png)

3. 在「方案總管」選擇專案檔，右鍵選擇「屬性」，在「Web」頁籤的專案 URL 貼上剛剛的 SSL URL 後儲存

    ![20190603_033424](img/20190603_033424.png)

4. Ctrl + F5 啟動專案，確認可以用 SSL URL 開啟網站

    ![20190603_033700](img/20190603_033700.png)

5. 回到專案，開啟「App_Start\FilterConfig.cs」，加入 ```RequireHttpsAttribute()``` filter

    > ※ 實際上用原本的非 SSL 加密的 URL 也還是可以開啟網站，所以要作此步驟

    ![20190603_034134](img/20190603_034134.png)

#### 向社群平台 (eg: Facebook) 註冊

註冊後，透過 key 和 secret 就能讓它認得我們，以下述操作為例

1. 註冊/登入 [Facebook for Developers](https://developers.facebook.com/)
2. 建立 app 後，可以得到 key 和 secret

    ![20190603_035031](img/20190603_035031.png)

3. 回到專案，開啟「App_Start\Startup.Auth.cs」，這裡已經內建了許多 OAuth 的程式
4. 承上，找到 ```app.UseFacebookAuthentication``` 將步驟 2 的 key 和 secret 填上
5. 建置後瀏覽登入頁，便會看到 Facebook 登入的選項

    ![20190603_040002](img/20190603_040002.png)

#### 社群登入並添加額外欄位資訊

以下述操作為例

1. 目前的「Facebook 登入」按鈕在取得授權後，僅會要求你提供 email

    ![20190603_040349](img/20190603_040349.png)

    在送出 email 後，其實會發生錯誤

    ![20190603_040436](img/20190603_040436.png)

    因為我們先前在 ```IdentityModels``` 中有額外定義要提供駕照號碼，而目前經由社群登入的確認表單並沒有這個欄位

    ![20190602_200603](img/20190602_200603.png)

2. 開啟「Views\Account\ExternalLoginConfirmation.cshtml」，加上「駕照號碼」欄位的程式碼

    ![20190603_041439](img/20190603_041439.png)

3. 承上，從檔案開頭可以看出 ViewModel 來源，在 ```ExternalLoginConfirmationViewModel``` 中，添加「駕照號碼」屬性

    ![20190603_041602](img/20190603_041602.png)

4. 在步驟 2 中，也能看出表單會 POST 到 ```AccountController``` 的 ```ExternalLoginConfirmation()``` Action，在這邊也要修改程式碼，讓表單 POST 後會將「駕照號碼」存至 DB

    ![20190603_042120](img/20190603_042120.png)

5. 再次用「Facebook 登入」驗證

### Exercise in section 8

1. 在登入頁添加「電話」欄位，設為必填且最大 50 字元
2. 將 Movie 在 CUD 相關的 Action 和 API 都套用 admin 才能操作的權限
3. 將 ```ExternalLoginConfirmationViewModel``` 類別獨立成一支檔案

## Performance Optimization

效能調校可以從三個層級下手，越下層的效能調校會越有感，效益也越大

- Data Tier: DB 或 I/O 端，例如 schema, query .. 等等
- Application Tier: Server-side 程式，例如 output cache, data cache, release builds, disabling session .. 等等
- Client Tier: Client-side 程式

    <table>
    <tr>
    <td>

    ![20190603_160358](img/20190603_160358.png)

    </td>
    <td>

    ![20190603_160618](img/20190603_160618.png)

    </td>
    </tr>
    </table>

作效能調校時應遵守幾點原則

- 避免過度調校，而犧牲了程式的可維護性
- 可以得到顯著的效益才付出相對應的心力和時間作調校

### Data Tier

- Schema
    - Primary Key: table 應該要有 PK
    - Relationship
    - Indexes: 過濾用的欄位應該加上索引
- Queries
    - EF 有時候會建出太複雜的查詢，此時可以改成自己刻一個 stored procedure 來跑
    - 透過 SQL Server 的「執行計畫 (Execution Plan)」功能來檢查查詢慢在哪
    - 透過 cache 暫存查詢結果

### Glimpse

監控所有 request 的套件，安裝與啟用方式，以下述操作為例

1. 透過 NuGet 安裝「Glimpse.Mvc5」和「Glimpse.EF6」
2. 瀏覽「https://localhost:{portNumber}/glimpse.axd」，將 Glimpse 開啟，Glimpse 便會開始監控

    ![20190603_164211](img/20190603_164211.png)

3. 試著瀏覽「https://localhost:{portNumber}/customers」，便會在網頁下方看到 Glimpse 面板，可以用來觀察連線時間、Action 來源、Ajax 呼叫過程、SQL 連線過程 .. 等等資訊

    ![20190603_171800](img/20190603_171800.gif)

### Output Cache

如果資料的異動頻率不高，可以透過 cache 的方式，其原理如下

1. 第一個使用者發出 request
2. application 向 DB 要資料
3. application 將 View 回傳給 client，並儲存 View 在 cache 中
4. 後續使用者發出 request
5. application 檢查 cache 若還沒過期，直接回傳 View 給 client

    ![20190603_173300](img/20190603_173300.gif)

#### 開啟 Output Cache

在 Controller 或 Action 上方加入 ```[OutputCache]``` 即可開啟 cache 功能

> ※ 千萬不要濫用 Output Cache，無法取得即時的正確資料恐會導致很多問題

``` csharp
// Duration 設定 cache 壽命，單位為秒
// Location 設定 cache 儲存於 Server-side 或 Client-side
// VaryByParam 設定要依哪個參數判斷是否使用 cache，可使用萬用字元 (*)
[OutputCache(Duration = 60, Location = System.Web.UI.OutputCacheLocation.Server, VaryByParam = "*")]
public ActionResult Index()
{
    // 可以透過回傳 DateTime.Now 來確認頁面是否有 cache 功能
    return View();
}
```

#### 強制關閉 Output Cache

``` csharp
// Duration 設為 0 秒
// VaryByParam 設定要依哪個參數判斷是否使用 cache，可使用萬用字元 (*)
// NoStore 告知瀏覽器不要從 cache 讀取資料
[OutputCache(Duration = 0, VaryByParam = "*", NoStore = true)]
public ActionResult Index()
{
    // 可以透過回傳 DateTime.Now 來確認頁面是否有 cache 功能
    return View();
}
```

### Data Caching

透過 ```MemoryCache.Default[key] = value``` 即可用 key/value 的方式儲存 Data Caching

> ※ 同樣地，不要濫用 Data Cache，無法取得即時的正確資料恐會導致很多問題

``` csharp
using System.Runtime.Caching;

public ActionResult Index()
{
    if (MemoryCache.Default["membershipTypes"] == null)
        MemoryCache.Default["membershipTypes"] = _context.MembershipTypes.ToList();

    var membershipTypes = (IEnumerable)MemoryCache.Default["membershipTypes"];

    return View(membershipTypes);
}
```

### Async

[非同步處理 (Async)](C%23%20Advanced.md#非同步處理-asyncawait) 簡單的概念如下

1. 未使用非同步處理時，使用者發出 request 後，由 thread 去 I/O 端取資料一定都會有延遲，thread 會一直等到 I/O 端回傳資料後，才繼續執行後續動作

    ![20190603_213408](img/20190603_213408.png)

2. 承上，想像如果同時有多個使用者都發出一樣的 request，每個人都會占用一條 thread，而且每個人都在等 thread 回應，當 thread pool 中的 thread 用盡時，後續的使用者就必須等待更久的時間

    ![20190603_213610](img/20190603_213610.png)

3. 使用非同步處理時，每條 thread 發現在 I/O 端取資料需要等待，就會改成先服務其它 request，等其它 request 處理完且原本 I/O 端也回傳資料了，再回頭處理第一個 request 的後續動作

    ![20190603_214019](img/20190603_214019.png)

#### Async 不會增進效能

想像下圖為水管，request 為由上往下流的水，Async 的確能同時處理更多 request，但在 I/O 端仍然需要等待一樣長的時間

![20190603_214500](img/20190603_214500.gif)

Async 增進的是可擴充性，如果在 I/O 端也一同透過 SQL Cluster, NoSQL, Azure 等方式作擴展，這樣才是徹底的增進效能

![20190603_214957](img/20190603_214957.png)

### Release Builds

我們預設是用 Debug 模式建置專案，檔案會產生許多 assembly 是 debug 才會用到的，如果是正式環境的建置，可以改採用 Release 模式建置，產出的檔案會較為輕量

![20190603_215512](img/20190603_215512.png)

### Disabling Session

Session 為 web server 分配給每個使用者的記憶體，用來暫存資料，而現今的 app 講求「**無狀態 (Stateless)**」，也就是每個 request 都是獨立且唯一的，處理完就結束，不會去記住他這次處理了什麼，以下述操作為例

1. 開啟「Web.config」，在 ```system.web``` 區塊加入以下程式

    ``` xml
    <system.web>
        <sessionState mode="Off"></sessionState>
        ...
    </system.web>
    ```

### Client Tier

在 Client Tier 調校效能有兩個原則

- 減少 request 的數量
- 減少 Server-side 回傳資料的 size

舉幾個例子來說

- DTO: 透過 DTO 僅回傳必要的屬性，也就減少回傳資料的 size
- images: 作適度的壓縮
- JS/CSS
    - 透過 bundle 可以減少 request 數量
    - 透過 compress 則可以減少 JS/CSS 的檔案大小
    - 將 js 放在 ```<body>``` tag 結尾前才載入，讓使用者可以先看到 Html 元素，避免 js 載入過久而導致畫面凍結

        ``` html
        <body>
            ...
            ...
            ...

            // Loading JS files here.
        </body>
        ```

#### 開啟 JS/CSS bundle

在「方案總管」可以看到「Web.config」底下還有兩個檔案，分別對應在 Debug 或 Release 模式下要如何改寫「Web.config」

![20190603_222454](img/20190603_222454.png)

在「Web.Release.config」可以看到，Release 模式時，下述程式碼的 ```debug``` 屬性會被移除

``` xml
<system.web>
    <compilation debug="true" targetFramework="4.7.2" />
</system.web>
```

以相同頁面載入 JS 為例

|                  無 bundle                  |                  有 bundle                  |
|:-------------------------------------------:|:-------------------------------------------:|
| ![20190603_223345](img/20190603_223345.png) | ![20190603_223628](img/20190603_223628.png) |

> ※ bundle 檔案後面那段亂碼是根據 bundle 的內容作的 hash，一旦檔案內容有異動 hash 就會跟著變動，目的是避免檔案修改後因瀏覽器 cache 住而未重新載入

## Building a Feature End-to-End Systematically

此章節仿造設計一個新功能時，應如何依序下手

### 設計新功能 - 選擇 Controller 形式

以設計「租借電影」的表單為例，可以先思考其使用情境、input、output、Controller 形式

- 使用情境：租借電影
- input: Customer and Movies
- output: none
- Controller 形式：因為會用到 JS 來修飾頁面，採 API Controller (僅回傳 data) 比 MVC Controller (回傳 html markup) 更合適

### 設計新功能 - 建立 Action

一開始先建立空的 Action 即可，傳入的物件應作成 DTO

``` csharp
// Path: Dtos\RentalDto.cs

public class RentalDto
{
    public int CustomerId { get; set; }
    public IEnumerable<int> MovieIds { get; set; }
}
```

``` csharp
// Path: Controllers\Api\RentalController.cs

public class RentalController : ApiController
{
    // POST /api/rental
    [HttpPost]
    public IHttpActionResult New(RentalDto rentalDto)
    {
        throw new NotImplementedException();
    }
}
```

### 設計新功能 - 建立 Model

根據下圖 UML 建立相關 Model 和 Migration 並更新至 DB

![20190604_175911](img/20190604_175911.png)

``` csharp
// Path: Models\Rentals.cs

public class Rental
{
    public int Id { get; set; }

    [Required]
    public Customer Customer { get; set; }

    [Required]
    public Movie Movie { get; set; }

    public DateTime DateRented { get; set; }

    public DateTime? DateReturned { get; set; }
}
```

``` csharp
// Path: Models\IdentityModels.cs

public class ApplicationDbContext : IdentityDbContext<ApplicationUser>
{
    ...

    public DbSet<Rental> Rentals { get; set; }

    ...
}
```

### 設計新功能 - 實作簡易 Action

先不管資料驗證，透過 Postman 打 API 看 Action 是否能正確儲存資料

``` csharp
// Path: Controllers\Api\RentalController.cs

// POST /api/rental
[HttpPost]
public IHttpActionResult AddRental(RentalDto rentalDto)
{
    var customer = _context.Customers.SingleOrDefault(c => c.Id == rentalDto.CustomerId);

    foreach (var movieId in rentalDto.MovieIds)
    {
        var movie = _context.Movies.SingleOrDefault(m => m.Id == movieId);

        var rental = new Rental
        {
            Customer = customer,
            Movie = movie,
            DateRented = DateTime.Now
        };

        _context.Rentals.Add(rental);
    }

    _context.SaveChanges();

    return Ok();
}
```

### 設計新功能 - 實作 Action 細節

例如去 \[Movies] table 檢查電影庫存量足夠才算成功租借

### 設計新功能 - Edge Cases

考量 Action 中有哪些地方可能會有例外 (Exception) 發生，加以防範並揭露錯誤資訊

### 設計新功能 - 前端畫面

建立前端用的 MVC Controller 和 View

<table>
<tr>
<th></th>
<th>傳統 Html Form</th>
<th>Ajax Form</th>
</tr>
<tr>
<td align="center">Razor 語法</td>
<td>

``` csharp
@using (Html.BeginForm("action", "controller"))
{
    ...
}
```

</td>
<td>

``` csharp
@using (Ajax.BeginForm("action", "controller",
    new AjaxOptions { HttpMethod = "POST" }))
{

}
```

</td>
</tr>
<tr>
<td align="center">Html 原始碼</td>
<td>

``` html
<form action="/controller/action" method="post">
    ...
</form>
```

</td>
<td>

``` html
<form action="/controller/action" data-ajax="true"
data-ajax-method="POST" id="form0" method="post">
    ...
</form>
```

</td>
</tr>
<tr>
<td align="center">差異</td>
<td>

submit 後到 Server-side 回傳的這段期間，網頁會變空白且無法操作

![20190605_005700](img/20190605_005700.gif)

</td>
<td>

submit 後到 Server-side 回傳的這段期間，網頁仍能繼續操作

![20190605_010200](img/20190605_010200.gif)

</td>
</tr>
</table>

如果 Form 是要 POST 到 Web API 的話，也可以考慮透過 Html + JS 就好

``` html
<form>
    ...
    <button type="submit">OK</button>
</form>

<script>
    $("form").submit(function (e) {
        ...
    });
</script>
```

### 設計新功能 - typeahead.js

文字欄位顯示建議的套件，安裝與使用方式，以下述操作為例

1. 透過 NuGet 安裝「Twitter.Typeahead」
2. 建立「Content\typeahead.css」，內容去複製 [example page](http://twitter.github.io/typeahead.js/examples/#remote) 的 css (class name 為 ```tt-*``` 開頭)
3. 在 bundle 加入此套件

    ``` csharp
    // js bundle
    bundles.Add(new ScriptBundle("~/bundles/lib").Include(
        ...,
        "~/Scripts/typeahead.bundle.js"));

    // css bundle
    bundles.Add(new StyleBundle("~/Content/css").Include(
        ...,
        "~/Content/typeahead.css",
        "~/Content/site.css"));
    ```

4. 模仿 [example page](https://twitter.github.io/typeahead.js/examples/#remote)，套用 typeahead.js 到欄位上

    ``` html
    <form>
        ...
        <input id="movie" type="text" class="form-control" />
        ...
    </form>

    @section scripts
    {
        <script>
            $(document).ready(function () {
                // customer field is similar as below code
                ...

                // for movie field
                var movies = new Bloodhound({
                    datumTokenizer: Bloodhound.tokenizers.obj.whitespace('name'),
                    queryTokenizer: Bloodhound.tokenizers.whitespace,
                    remote: {
                        url: '/api/movies?query=%QUERY',
                        wildcard: '%QUERY'
                    }
                });

                $('#movie').typeahead({
                    minLength: 2,
                    highlight: true
                }, {
                    name: 'movies',
                    display: 'name',
                    source: movies
                });
            });
        </script>
    }
    ```

5. 加上 event handler，在選擇文字後儲存在物件中，並顯示在列表

    ``` html
    <form>
        ...
        <input id="movie" type="text" class="form-control" />
        <div class="row">
            <div class="col-md-4">
                <ul id="movieList" class="list-group"></ul>
            </div>
        </div>
        ...
    </form>

    @section scripts
    {
        <script>
            $(document).ready(function () {

                ...

                let vm = {
                    movieIds: []
                };

                $('#movie').typeahead({
                    // initial options
                }).on("typeahead:select", function (e, movie) {
                    vm.movieIds.push(movie.id);
                    $("#movieList").append(`<li class="list-group-item">${movie.name}</li>`)
                    $('#movie').typeahead("val", "");
                });
            });
        </script>
    }
    ```

6. 調整 API，根據 query 給的字串去過濾結果

    ![20190605_182505](img/20190605_182505.png)

### 設計新功能 - 提交表單

用 Ajax 將儲存的物件送到 API

``` html
<form id="addRental">
    ...
    <button type="submit">OK</button>
</form>

<script>

    ...

    $("#AddRental").submit(function (e) {
        // 避免 Html submit 的動作出現
        e.preventDefault();

        $.ajax({
            method: "POST",
            url: "/api/rental",
            data: vm
        })
        .done(function (e) {
            console.log("done");
        })
        .fail(function (e) {
            console.log("fail");
        });
    });
</script>
```

### 設計新功能 - toastr.js

顯示通知視窗的套件，安裝與使用方式，以下述操作為例

1. 透過 NuGet 安裝「toastr」
2. 在 bundle 加入此套件

    ``` csharp
    // js bundle
    bundles.Add(new ScriptBundle("~/bundles/lib").Include(
        ...,
        "~/Scripts/toastr.js"));

    // css bundle
    bundles.Add(new StyleBundle("~/Content/css").Include(
        ...,
        "~/Content/toastr.css",
        "~/Content/site.css"));
    ```

3. 實作

    ``` javascript
    ...

    $("#AddRental").submit(function (e) {
        e.preventDefault();

        $.ajax({
            method: "POST",
            url: "/api/rental",
            data: vm
        })
        .done(function (e) {
            toastr.success("Rentals successfully recorded.");
        })
        .fail(function (e) {
            toastr.error("Unexpected error.");
        });
    });
    ```

### 設計新功能 - Client-side Validation

使用內建的 jQuery Validation 套件作表單驗證

1. 在頁面載入

    ``` csharp
    @section scripts
    {
        @Scripts.Render("~/bundles/jqueryval")

        ...
    }
    ```

2. 欄位加入 ```name``` 和 驗證屬性且 submit 事件改成在 ```validate()``` 中處理

    ``` html
    <form id="AddRental">
        ...
        <!-- 沒加 name 屬性的話，jQuery Validation 有時候不會生效 -->
        <input id="customer" name="customer" type="text" class="form-control" required />
        ...
    </form>

    @section scripts
    {
        <script>
            $(document).ready(function () {
                ...

                $("#AddRental").validate({
                    submitHandler: function (form) {
                        event.preventDefault();

                        $.ajax({
                            method: "POST",
                            url: "/api/rental",
                            data: vm
                        })
                        .done(function (e) {
                            toastr.success("Rentals successfully recorded.");
                        })
                        .fail(function (e) {
                            toastr.error("Unexpected error.");
                        });
                    }
                });
            });
        </script>
    }
    ```

3. 加上自定義規則

    ``` html
    <form id="AddRental">
        ...
        <input id="customer" name="customer" type="text" class="form-control" required data-rule-validCustomer="true" />
        ...
    </form>

    @section scripts
    {
        <script>
            $(document).ready(function () {
                ...

                // add custom rules
                $.validator.addMethod("validCustomer", function (value, element) {
                    return vm.customerId && vm.customerId > 0;
                }, "Select a valid customer.");

                $("#AddRental").validate({
                    submitHandler: function (form) {
                        event.preventDefault();

                        $.ajax({
                            method: "POST",
                            url: "/api/rental",
                            data: vm
                        })
                        .done(function (e) {
                            toastr.success("Rentals successfully recorded.");
                        })
                        .fail(function (e) {
                            toastr.error("Unexpected error.");
                        });
                    }
                });
            });
        </script>
    }
    ```

4. 調整錯誤訊息的 css

### 設計新功能 - 表單初始化

在 Ajax 成功後，要清空欄位、初始化物件、重置 Validation 狀態

``` javascript
$.ajax({
    method: "POST",
    url: "/api/rental",
    data: vm
})
.done(function (e) {
    toastr.success("Rentals successfully recorded.");

    $("#customer").typeahead("val", "");
    $("#movie").typeahead("val", "");
    $("#movieList").empty();

    vm = { movieIds: [] };

    validator.resetForm();
})
.fail(function (e) {
    toastr.error("Unexpected error.");
});
```

## Deployment

### Deploying the Application

發行應用程式，以下述操作為例

1. 在「方案總管」對著專案檔右鍵，選擇「發行...」

    ![20190606_153750](img/20190606_153750.png)

2. 挑選發佈目標

    ![20190606_153948](img/20190606_153948.png)

3. 發佈或建立設定檔 (供其他專案發佈使用)
4. Visual Studio 也會建立「Properties\PublishProfiles\\*{設定檔名稱}*.pubxml」，如果將此檔案加到版控，就可以讓其它工程師也使用相同的發佈設定
5. 發佈後可以在目標資料夾看到以下資料夾和檔案，其內容可參考[預設專案結構](#預設專案結構)

    ![20190606_154432](img/20190606_154432.png)

### Deploying the Database

如果是採用 Code First 策略的 EF

1. 在套件管理器主控台 (Package Manager Console) 輸入「Update-Database -Script \[-SourceMigration:*{migrationName}*]」產生 Migration script，例如 ```Update-Database -Script -SourceMigration:InitialModel```
2. 將產生的 script 交給 DBA 或在目標 DB 執行

> ※ 可以在 \[__MigrationHistory] table 看到所有執行過的 Migration 紀錄
>
> ![20190606_155829](img/20190606_155829.png)

### Build Configurations

在不同環境或 Server 中的發佈設定可能有所差異，例如 DB 連線字串不同，以下述操作為例建立不同的「Web.config」

1. 在「方案組態」選擇「組態管理員」

    ![20190606_160626](img/20190606_160626.png)

2. 新增方案組態，例如「SIT」

    ![20190606_160725](img/20190606_160725.png)

    除非是在開發環境，否則設定值建議複製自「Release」設定檔

    ![20190606_160854](img/20190606_160854.png)

3. 接著在「方案總管」對著「Web.config」右鍵，選擇「新增設定轉換」

    ![20190606_161109](img/20190606_161109.png)

    就會長出「Web.SIT.config」

    ![20190606_161212](img/20190606_161212.png)

4. 承上，在發佈專案時，如果選擇這個組態，就會用這個「Web.SIT.config」去覆寫原本的「Web.config」

### Application Settings

在不同環境或 Server 中的應用程式設定可能有所差異，例如 API 金鑰、Mail Server 網址，以下述操作為例建立不同的「Web.config」

1. 在「Web.config」的 ```<appSettings></appSettings>``` 區塊，用 key/value 的方式加入設定值

    ``` xml
    <!-- Web.config -->

    ...
    <appSettings>
        ...
        <add key="FacebookApiId" value="12345" />
        <add key="FacebookApiKey" value="abcde" />
        <add key="MailServer" value="dev-smtp.vidly.com" />
    </appSettings>
    ...
    ```

2. 在程式中叫用的方式為 ```ConfigurationManager.AppSettings[{key}]```，例如

    ``` csharp
    ...
    app.UseFacebookAuthentication(
        appId: ConfigurationManager.AppSettings["FacebookApiId"],
        appSecret: ConfigurationManager.AppSettings["FacebookApiKey"]);
    ...
    ```

3. 也可以在其它組態設定檔，例如「Web.SIT.config」，來作覆寫

    ``` xml
    <!-- Web.SIT.config -->

    ...
    <appSettings>
        ...
        <add key="MailServer" value="sit-smtp.vidly.com"
            xdt:Transform="SetAttributes" xdt:Locator="Match(key)"/>
    </appSettings>
    ...
    ```

4. 在「方案總管」對著「Web.SIT.config」右鍵，選擇「預覽和轉換」就可以預覽覆寫後的「Web.config」

    ![20190606_163712](img/20190606_163712.png)

### Securing Configuration Settings

在現實案例中，如果你的 repo 會發佈到公開的版控 Server (例如 GitHub)，將連線字串跟 API Key 直接寫在 xml 中是很危險的，以下述操作為例示範如何加密設定檔

1. 在專案新增一個組態設定檔，例如「AppSettings.config」

    ![20190606_164915](img/20190606_164915.png)

2. 將原本「Web.config」的 ```<appSettings></appSettings>``` 區塊移到「AppSettings.config」中，並用 ```configSource``` 屬性指定來源

    ``` xml
    <!-- Web.config -->

    ...
    <appSettings configSource="AppSettings.config"></appSettings>
    ...
    ```

    ``` xml
    <!-- AppSettings.config -->

    ...
    <appSettings>
        ...
        <add key="FacebookApiId" value="12345" />
        <add key="FacebookApiKey" value="abcde" />
        <add key="MailServer" value="dev-smtp.vidly.com" />
    </appSettings>
    ...
    ```

3. 在發佈專案後，對「AppSettings.config」作加密再部署檔案至 Server

    > ※ 因為加密方式會依情況不同而異，這邊僅提供概念

### Custom Error Pages

如果採用內建的 error page 會嚇到使用者，而且也揭露過多程式資訊，導致容易被攻擊，常見的錯誤畫面有以下幾種

|          錯誤來源          |                  範例畫面                   |
|:--------------------------:|:-------------------------------------------:|
|   ASP&#46;NET exception    | ![20190606_170954](img/20190606_170954.png) |
| ASP&#46;NET 回傳 404 error | ![20190606_173412](img/20190606_173412.png) |
|     IIS 回傳 404 error     | ![20190606_171729](img/20190606_171729.png) |

#### Custom Error Pages - ASP&#46;NET exception

替換 error page，以下述操作為例

1. 在「Web.config」的 ```<system.web></system.web>``` 區塊，加上 ```<customErrors>``` 標籤

    ``` xml
    <!-- Web.config -->

    ...
    <system.web>
        <!-- mode="On" 為永遠開啟，
             mode="RemoteOnly" 為在非 localhost 環境才開啟 -->
        <customErrors mode="On"></customErrors>
    </system.web>
    ...
    ```

2. ASP&#46;NET MVC 預設的錯誤畫面來自「Views\Shared\Error.cshtml」，其 Controller 為在「App_Start\FilterConfig.cs」的 ```HandleErrorAttribute()```

    ![20190606_172749](img/20190606_172749.png)

#### Custom Error Pages - ASP&#46;NET 回傳 404 error

替換 error page，以下述操作為例

1. 在「Web.config」的 ```<system.web></system.web>``` 區塊，其 ```<customErrors>``` 標籤中，再寫明錯誤代碼和自定義錯誤頁的路徑

    ``` xml
    <!-- Web.config -->

    ...
    <system.web>
        <customErrors mode="On">
            <error statusCode="404" redirect="~/404.html"/>
        </customErrors>
    </system.web>
    ...
    ```

2. 建立「404.html」

    ![20190606_173700](img/20190606_173700.png)

#### Custom Error Pages - IIS 回傳 404 error

替換 error page，以下述操作為例

1. 在「Web.config」的 ```<system.webServer></system.webServer>``` 區塊，加上 ```<httpErrors>``` 標籤，並寫明錯誤代碼和自定義錯誤頁的路徑

    ``` xml
    <!-- Web.config -->

    ...
    <system.webServer>
        <!-- errorMode="Custom" 為永遠開啟，
             errorMode="DetailedLocalOnly" 為在非 localhost 環境才開啟 -->
        <httpErrors errorMode="Custom">
            <remove statusCode="404"/>
            <error statusCode="404" path="404.html" responseMode="File"/>
        </httpErrors>
    </system.webServer>
    ...
    ```

2. 建立「404.html」
3. 在瀏覽器瀏覽靜態檔案，會出現自定義錯誤頁，但是「Network」紀錄仍是 404，這樣才容易 debug

    ![20190606_174549](img/20190606_174549.png)

### Logging Unhandled Exceptions

[ELMAH (Error Logging Modules and Handlers)](https://elmah.github.io/) 是紀錄所有 exception 的套件，安裝與啟用方式，以下述操作為例

1. 透過 NuGet 安裝「Elmah」
2. 瀏覽「https://localhost:{portNumber}/elmah.axd」，即可看到過去的錯誤訊息

    ![20190606_175914](img/20190606_175914.png)

3. Elmah 預設只有 localhost 才能存取，欲釋放權限可以在「Web.config」的 ```<elmah></elmah>``` 區塊調整

    ![20190606_180139](img/20190606_180139.png)

4. 另外，Elmah 預設僅會將錯誤訊息存在記憶體，欲寫至 DB 或寄信等功能須自行參考官方文件

## 主題顏色設計

可以到 <https://color.adobe.com> 設計網頁的配色
