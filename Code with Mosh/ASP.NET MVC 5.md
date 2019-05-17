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
    - RouteConfig.cs: 設定 router 規則，以下圖為例

        ![20190517_190220](img/20190517_190220.png)

        當 url 符合「{controller}/{action}/{id}」格式時，router 就會去解析該由哪個 controller 的哪個 action 去作事

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
    - Application_Start() 中會在應用程式啟動時執行，例如註冊 router
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
        - 部分檢視(Partial View)指的是可以用很多個部分檢視，組合成一個完整的檢視，這邊不勾選
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

4. Ctrl+F5 檢查執行結果

    ![20190517_235917](img/20190517_235917.png)

## 加入主題(Adding a Theme)

ASP<span>.</span>NET MVC 使用 [Bootstrap](https://getbootstrap.com/) 作為前端框架，我們可以到 [Bootswatch](https://bootswatch.com/3/) 尋找免費的 Bootstrap 主題回來替換，以「Lumen」這個主題為例

1. 下載 css，並重新命名為「bootstrap-lumen.css」
2. 加入至專案的「Content」資料夾下

    ![20190518_001158](img/20190518_001158.png)

3. 開啟「App_Start\BundleConfig.cs」，將原本載入 ```bootstrap.css``` 的地方改為 ```bootstrap-lumen.css```

    ![20190518_001901](img/20190518_001901.png)

4. Ctrl+F5 檢查執行結果
