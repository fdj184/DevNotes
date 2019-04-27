# C# Basics for Beginners

![C# Basics](img/20190427_224541.png)

Course Link: <https://codewithmosh.teachable.com/p/csharp-basics-for-beginners>

---

## C# vs .NET

|      |      C#      |               .NET                |
|:----:|:------------:|:---------------------------------:|
| 定義 | 一種程式語言 | 一種軟體框架，並不限於用 C# 撰寫  |
| 組成 |              | 包含 CLR 和函式庫(Class Library) |

## CLR

- 透過編譯器(compiler) 把 C# 轉成中介語言(IL)，再透過 JIT 編譯器轉成原生機器語言
- 提供記憶體管理、例外處理、垃圾回收機制(GC)、執行緒管理等服務

  ![CLR](img/20190427_230508.png)

可參考[保哥文章](https://blog.miniasp.com/post/2015/07/28/Clarify-the-versions-between-CLR-NET-CSharp-Visual-Studio-and-ASPNET)

## Architecture of .NET Applications

|                       |                        描述                        |                  概念                   |
|:---------------------:|:--------------------------------------------------:|:---------------------------------------:|
|      類別(Class)      | 每個 Class 都有自己的屬性(Attribute)和方法(Method) |    ![Class](img/20190427_232513.png)    |
|  命名空間(Namespace)  |       相似的 Class 會放在同一個 Namespace 中       |  ![Namespace](img/20190427_232918.png)  |
|    組件(Assembly)     |     相似的 Namespace 會放在同一個 Assembly 中      |  ![Assembly](img/20190427_233207.png)   |
| 應用程式(Application) |               由很多的 Assembly 組成               | ![Application](img/20190427_233938.png) |

## Visual Studio 基本介紹 (以 Console 為例)

### 方案總管

![20190428_002347](img/20190428_002347.png)

- 一個方案(Solution)可以有很多個專案(Project)
- 專案中的「Properties\AssemblyInfo.cs」用來描述建立出來的 Assembly 有什麼資訊，例如版本號
- 專案中的「參考」是系統預設你可能會用到的 Assembly，實際上不一定會全用到
- 專案中的「App.config」是這個應用程式的設定檔，例如 DB 連線字串

### 編輯區

![20190428_003407](img/20190428_003407.png)

- 「紅色區塊」用來引用其它 Namespace 的程式
- 「Main」區塊是程式進入點
- 「黃色區塊」放函式回傳值，這裡的 void 代表不回傳值
- 「綠色區塊」放函式傳入值
