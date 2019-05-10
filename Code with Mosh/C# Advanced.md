# C# Advanced: Get Ready for Job Interviews

![C# Advanced](img/20190510_183200.jpg)

Course Link: <https://codewithmosh.com/p/csharp-advanced>

---

## 泛型(Generics)

- 在 C# 1.0 時代，如果需要可變長度陣列，有兩種作法
    1. 建立各種型態的陣列類別：因為每個類別都需要 Add()、Remove() .. 等等方法，造成大量的重複程式碼
    2. 建立 Object 型態的陣列類別：雖然解決了前者的問題，但在執行過程產生[裝箱(Boxing)、拆箱(Unboxing)](C%23%20Intermediate.md#Boxing-and-Unboxing)，造成效能低落
- 在 C# 2.0 之後，我們可以使用泛型，也就是在定義類別或方法時，可以使用一個或多個不指定型態的變數，等到建立實體時，在明定型態為何

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
