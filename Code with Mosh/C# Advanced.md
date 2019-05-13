# C# Advanced: Get Ready for Job Interviews

![C# Advanced](img/20190510_183200.jpg)

Course Link: <https://codewithmosh.com/p/csharp-advanced>

---

## 泛型(Generics)

- 在 C# 1.0 時代，如果需要可變長度陣列，有兩種作法
    1. 建立各種型態的陣列類別：因為每個類別都需要 Add()、Remove() .. 等等方法，造成大量的重複程式碼
    2. 建立 Object 型態的陣列類別：雖然解決了前者的問題，但在執行過程產生[裝箱(Boxing)、拆箱(Unboxing)](C%23%20Intermediate.md#Boxing-and-Unboxing)，造成效能低落
- 在 C# 2.0 之後，我們可以使用泛型，也就是在定義類別或方法時，可以使用一個或多個不指定型態的變數，等到建立實體時，再明定型態為何

    ``` csharp
    // T 代表 Type 或 Template，不指定型態
    public class GenericList<T>
    {
        public void Add(T element)
        {

        }
    }

    partial class Program
    {
        static void Main(string[] args)
        {
            // 建立實體時才指定此陣列為哪種型態
            var genericList = new GenericList<int>();
            genericList.Add(10);
        }
    }
    ```

### 條件約束(Constraints)

- 使用條件約束可以讓 compiler 知道，泛型類別或方法建立實體後的型態會有那些功能
- 語法為在類別或方法之後加上「```where T :``` + 條件約束」
- 條件約束有以下幾種
    1. 介面(Interface)

        ``` csharp
        public class Utilities
        {
            // 數字型態的比大小
            public int IntBigger(int a, int b)
            {
                return a > b ? a : b;
            }

            // 泛型的比大小
            // 因為 a 跟 b 本身是 object 型態，無法單純用數學的方式比大小，
            // 但是可以透過條件約束 IComparable，告訴 compiler 這兩個值在建立實體時能夠比大小
            public T GenericBigger<T>(T a, T b) where T : IComparable
            {
                return a.CompareTo(b) > 0 ? a : b;
            }
        }
        ```

    2. 類別(Class)

        ``` csharp
        public class Product
        {
            public int Id { get; set; }
            public int Price { get; set; }
        }

        // 條件約束使用類別，即可使用該類別的類別成員
        public class DiscountList<T> where T : Product
        {
            public decimal GetDiscount(T product)
            {
                return product.Price * 0.8m;
            }
        }
        ```

    3. Value Types

        ``` csharp
        public class Nullable<T> where T : struct
        {

        }
        ```

    4. 無參數建構式

        ``` csharp
        public class Nullable<T> where T : new()
        {
            object obj = new T();
        }
        ```

    5. 其它用法可參考[微軟文件](https://docs.microsoft.com/zh-tw/dotnet/csharp/programming-guide/generics/constraints-on-type-parameters)

## 委派(Delegates)

- 把一或多個方法(Method)當作參數，傳遞到某一個類別的方法中
- 用於建立彈性且可擴充的應用程式
- 以下述程式碼為例
    1. 假設我們有一個後製照片的流程如下

        ``` csharp
        // PhotoFilters.cs 相片後製細節
        public class PhotoFilters
        {
            public void ApplyBrightness(Photo photo)
            {
                Console.WriteLine("Apply brightness");
            }

            public void ApplyContrast(Photo photo)
            {
                Console.WriteLine("Apply contrast");
            }

            public void Resize(Photo photo)
            {
                Console.WriteLine("Resize the photo");
            }
        }

        // PhotoProcessor.cs 相片後製的標準流程
        public class PhotoProcessor
        {
            public void Process(string path)
            {
                var photo = new Photo(path);

                var filters = new PhotoFilters();
                filters.ApplyBrightness(photo);
                filters.ApplyContrast(photo);
                filters.Resize(photo);

                photo.Save();
            }
        }

        // Photo.cs 相片物件
        public class Photo
        {
            public Photo(string path) { }

            public void Save() { }
        }

        // Program.cs 主程式
        partial class Program
        {
            static void Main(string[] args)
            {
                var processor = new PhotoProcessor();
                processor.Process(@"C:\123.jpg");
            }
        }
        ```

    2. 承上，相片後製的標準流程是寫死的，今天如果有其它工程師要在某張相片添加新的後製功能，例如去除紅眼，就必須要改動 PhotoFilters.cs(增加去除紅眼的細節) 和 PhotoProcessor.cs(流程增加去除紅眼)，改動程式也等於要重新編譯和部署，產生諸多不便
    3. 這個時候就可以把流程作成 delegate，透過增減方法(Method)的方式來決定流程

        ``` csharp
        // PhotoFilters.cs 相片後製細節
        public class PhotoFilters
        {
            public void ApplyBrightness(Photo photo)
            {
                Console.WriteLine("Apply brightness");
            }

            public void ApplyContrast(Photo photo)
            {
                Console.WriteLine("Apply contrast");
            }

            public void Resize(Photo photo)
            {
                Console.WriteLine("Resize the photo");
            }
        }

        // PhotoProcessor.cs 相片後製的標準流程
        public class PhotoProcessor
        {
            // 宣告委派
            public delegate void PhotoFilterHandler(Photo photo);

            public void Process(string path, PhotoFilterHandler filterHandler)
            {
                var photo = new Photo(path);

                // 這裡透過委派，只定義相片要經過後製，但是是那些後製項目則在主程式才決定
                filterHandler(photo);

                photo.Save();
            }
        }

        // Photo.cs 相片物件
        public class Photo
        {
            public Photo(string path) { }

            public void Save() { }
        }

        // CustomPhotoFilters.cs 去除紅眼功能
        public class CustomPhotoFilters
        {
            // 需要和 PhotoFilters.cs 中的方法，使用相同型態和數量的傳入參數
            public void RemoveRedEyes(Photo photo)
            {
                Console.WriteLine("Remove red eyes on the photo");
            }
        }

        // Program.cs 主程式
        partial class Program
        {
            static void Main(string[] args)
            {
                var processor = new PhotoProcessor();

                var filters = new PhotoFilters();
                // 建立委派的實體，並指明要經過哪些後製項目
                PhotoProcessor.PhotoFilterHandler filterHandler = filters.ApplyBrightness;
                filterHandler += filters.ApplyContrast;
                filterHandler += filters.Resize;
                // 後來新增的去除紅眼功能，可以不異動到 PhotoFilters.cs 和 PhotoProcessor.cs 就加到後製流程中
                var customFilters = new CustomPhotoFilters();
                filterHandler += customFilters.RemoveRedEyes;

                processor.Process(@"C:\123.jpg", filterHandler);
            }
        }
        ```

### System.Action and System.Func

<table style="text-align: center;">
    <tr>
        <th></th>
        <th>System.Action&lt;T&gt;</th>
        <th>System.Func&lt;T&gt;</th>
    </tr>
    <tr>
        <td>描述</td>
        <td colspan="2">內建的委派泛型集合</td>
    </tr>
    <tr>
        <td>差異</td>
        <td>一般使用</td>
        <td>要多傳一個 out 參數</td>
    </tr>
</table>

用前面的相片例子來改寫

``` csharp
// PhotoFilters.cs 相片後製細節
public class PhotoFilters
{
    // code 不變
    ...
}

// PhotoProcessor.cs 相片後製的標準流程
public class PhotoProcessor
{
    // 不必再自行宣告委派型態
    // public delegate void PhotoFilterHandler(Photo photo);

    // 傳入的委派型態改用內建的 Action<T>
    public void Process(string path, Action<Photo> filterHandler)
    {
        var photo = new Photo(path);

        filterHandler(photo);

        photo.Save();
    }
}

// Photo.cs 相片物件
public class Photo
{
    // code 不變
    ...
}

// CustomPhotoFilters.cs 去除紅眼功能
public class CustomPhotoFilters
{
    // code 不變
    ...
}

// Program.cs 主程式
partial class Program
{
    static void Main(string[] args)
    {
        var processor = new PhotoProcessor();

        var filters = new PhotoFilters();
        // 建立委派的實體，也要改成使用內建的 Action<T>
        Action<Photo> filterHandler = filters.ApplyBrightness;
        filterHandler += filters.ApplyContrast;
        filterHandler += filters.Resize;
        var customFilters = new CustomPhotoFilters();
        filterHandler += customFilters.RemoveRedEyes;

        processor.Process(@"C:\123.jpg", filterHandler);
    }
}
```

### 委派(Delegate) vs. 介面(Interface)

<table style="text-align: center;">
    <tr>
        <th></th>
        <th>Delegate</th>
        <th>Interface</th>
    </tr>
    <tr>
        <td>用途</td>
        <td colspan="2">建立彈性且可擴充的應用程式</td>
    </tr>
    <tr>
        <td>差異</td>
        <td style="text-align: left;">委派類似於只有一種方法(Method)的介面，<br>建立實體後傳入的方法只會作類似的事，並使用相同的傳入參數</td>
        <td style="text-align: left;">一個介面可以有多個方法(Method)，<br>介面被實作後，會根據定義，實作不一樣的方法<br></td>
    </tr>
    <tr>
        <td>適用情況</td>
        <td style="text-align: left;">
            <ul>
                <li>使用觀察者模式(Observer Pattern)時</li>
                <li>會被多次建立實體時(不同實體會註冊不同的方法)</li>
            </ul>
        </td>
        <td style="text-align: left;">介面實作後的方法僅會建立一次時</td>
    </tr>
</table>

參考文章：

[Delegate vs Interfaces-Any more clarifications available?](https://softwareengineering.stackexchange.com/questions/114138/delegate-vs-interfaces-any-more-clarifications-available)

[When to Use Delegates Instead of Interfaces](https://stackoverflow.com/questions/11204598/when-to-use-delegates-instead-of-interfaces)
