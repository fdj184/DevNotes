# C# Intermediate: Classes, Interfaces and Object-oriented Programming

![C# Intermediate](img/20190501_155800.jpg)

Course Link: <https://codewithmosh.teachable.com/p/object-oriented-programming-in-csharp>

---

## Classes

### 類別(Classes)

- 應用程式(Application)由眾多的類別(Class)組成，每個類別有自己的工作，例如在一個部落格(blog)中，會有
  
    1. 表現層(Presentation Layer)：有負責瀏覽文章的 Class
    2. 商業邏輯層(Business Logic Layer)/領域層(Domain Layer)：有負責張貼文章的 Class
    3. 資料存取層(Data Access Layer)/持久層(Persistence Layer)：有儲存文章的 Class

    ![20190501_160954](img/20190501_160954.png)

- Class 擁有

    1. 資料(Data)：透過欄位(field)來呈現
    2. 行為(Behavior)：透過方法(method)/函式(function)來呈現

- 在 UML 中，一個 Class 由上至下，分別為 Class Name、Fields、Methods

    ![20190501_162929](img/20190501_162929.png)

- 宣告時，Class Name 須使用 Pascal Case，即所有單字之首字母大寫
- 在介紹到 OOP 的[封裝(Encapsulation)](#封裝Encapsulation)概念以前，本筆記所有範例中的欄位(Field)都會暫時用 public 來展示

    ``` csharp
    // 在官方文件的 coding style 中，建議類別的欄位宣告和方法宣告中間要有一行空白
    public class Person {
        public string Name;

        public void SayHi()
        {
            Console.Write("Hello World");
        }
    }
    ```

### 物件(Objects)

- 從某個類別建立出來的實體(Instance)

    ![20190501_163343](img/20190501_163343.png)

- 宣告時，Object Name 須使用 Camel Case，即首單字之首字母小寫，其餘單字之首字母大寫

    ``` csharp
    Person person = new Person();

    // 在官方文件的 coding style 中，建議能判斷等號右邊類別的宣告，等號左邊改使用隱含宣告(var)
    var person = new Person();
    ```

- 使用 Object

    ``` csharp
    var person = new Person();
    person.Name = "John";
    person.SayHi();
    ```

### 類別成員(Class Members)

Fields 和 Methods 都屬於類別成員，類別成員又可分成

1. Instance Members
    - 從 object 存取
    - 在每個實體都可能有不同定義的類別成員
    - 在每個實體中各自佔有記憶體空間
2. Static Members
    - 從 class 存取
    - 不需要在每個實體都定義的類別成員
    - 不論有多少實體，都只會有一個靜態成員複本
    - 在型別前面加上 static 關鍵字即成為 static member

#### Code Samples for Class Members

1. 建立一個 Person 類別
2. 承上，給予 Name 欄位和 SayHi() 方法，因為這兩者在不同實體中可能有不同的定義，所以應設為 instance member

    ![20190501_185106](img/20190501_185106.png)

3. 承上，為 Person 類別添加一個 Parse() 方法，因為無論何時使用 Parse() 都是為了作同一件事(建立實體)，所以應設為 static member

    ![20190501_190622](img/20190501_190622.png)

### 建構式(Constructor)

- 類別建立實體建立時所呼叫的方法
- 目的是讓物件擁有初始狀態
- 建構式名稱須和類別名稱相同，且沒有回傳值(return)
- 若 class 沒定義建構式，編譯器會自動產生一個沒傳入值且內容為空的建構式，且該建構式不做任何事

    ``` csharp
    public class Person
    {
        // 空的建構式
        public Person()
        {

        }
    }
    ```

- 建構式可以多載(Overloading)，即針對不同傳入值有不同的行為

    ``` csharp
    public class Person
    {
        // 以下都是建構式
        public Person() { ... }
        public Person(string name) { ... }
        public Person(int id, string name) { ... }
    }
    ```

#### Code Samples for Constructors

1. 建立 Customer 類別並將其獨立成一支 Customer.cs

    ![20190502_163142](img/20190502_163142.png)

2. 在 Main() 建立 Customer 的實體，並查看其欄位，會是沒有初始值的狀態

   ![20190502_163757](img/20190502_163757.png)

3. 回到 Customer.cs，建立三個建構式

    ※ 小技巧：在類別中輸入「ctor」即可產生建構式程式碼片段(code snippet)

    ![20190502_165808](img/20190502_165808.png)

4. 回到 Main()，即可透過建構式在建立 Customer 實體時給予初始值

    ![20190502_170509](img/20190502_170509.png)

#### Best Practices for Constructors

- 在現實世界中，類別不可能為每個欄位都建立建構式，我們只針對有必要給予初始狀態的欄位給予建構式
- 以下述程式碼為例

    1. 在 Customer 類別中加入一個 Order 欄位，其型態為 List\<string>

        ![20190502_183531](img/20190502_183531.png)

    2. 回到 Main() 欲使用該欄位會出現 NullReferenceException，因為 ```customer.Order``` 現在還是 null，沒辦法執行 ```Add()``` 方法，換句話說，List<> 型態需要初始化過才能使用

        ![20190502_183715](img/20190502_183715.png)

    3. 承上，初始化的動作不應該是在 Main() 裡面進行，而是應該在類別就有初始化過，這就是建構式派上用場的時候

        ![20190502_184014](img/20190502_184014.png)

    4. 此時建構式還不算完整，因為僅有傳入空值的建構式會初始化 Order 欄位，若是呼叫其它多載的建構式來建立物件，一樣會有步驟2的問題，所以我們可以透過 ```: this()``` 的寫法來複製建構式

        ![20190502_224318](img/20190502_224318.png)

    5. 在上圖中，呼叫 B 之前就會一併呼叫 A，呼叫 C 之前也會呼叫 B，如此一來不管使用哪個建構式來建立物件，都會對 Order 欄位作初始化
    6. 此範例僅是為了 demo ```: this()``` 的用法，實際上 Id 和 Name 兩個欄位是沒必要初始化的， Customer.cs 簡化成下圖即可

        ![20190502_224932](img/20190502_224932.png)

### Object Initializers

- 快速建立物件並不呼叫其類別之任一建構式的寫法
- 此寫法可以避免定義大量的建構式，導致日後難以維護的情況

#### Code Samples for Object Initializers

假設 Person 類別有 4 個欄位，與其在類別中定義 4 個或更多建構式，我們可以在建立物件時使用 Object Initializer 的寫法直接對我們需要的欄位作初始化

``` csharp
// 類別
public class Person
{
    public int Id;
    public string FirstName;
    public string LastName;
    public DateTime Birthday;
}

// 主程式
partial class Program
{
    static void Main(string[] args)
    {
        // 僅針對 FirstName 和 LastName 兩個欄位作初始化
        var person = new Person
        {
            FirstName = "Bruce",
            LastName = "Wayne"
        };
    }
}
```

### 方法(Methods)

#### 方法簽章(Signature of Methods)

用以區別不同方法的特徵，有以下兩點

1. 方法名稱(Name)
2. 傳入參數的型態(Type)和數量

#### 多載(Overloading)

Having a method with same name but different signatures.

``` csharp
public class Person
{
    public void Walk(int distance) { ... }
    public void Walk(string location) { ... }
    public void Walk(string location, int speed) { ... }
}
```

#### Param 修飾詞

- 知道傳入參數都是同樣型態，但不知道個數時可使用
- 參數型態必須是一維陣列
- 呼叫時，可以用陣列或逗號分隔的方式傳入值

``` csharp
public class Calculator
{
    // Add() 可以傳入任何數量的 integer
    public int Add(param int[] numbers) { ... }
}

partial class Program
{
    static void Main(string[] args)
    {
        var calculator = new Calculator();
        var result1 = calculator.Add(new int[] { 1, 2, 3, 4 });
        var result2 = calculator.Add(5, 6, 7);
    }
}
```

#### Ref 修飾詞

- It's a code smell in Mosh's opinion. **不建議使用**，知道有這個東西就好
- 將 value type 當成 reference type 來使用 (兩者差異可參考 [C# Basics.md](C%23%20Basics.md#Value-Types-and-Reference-Types))
- 以下述程式碼為例

    |        |                                                         沒有使用 ref 修飾詞                                                         |                                                           使用 ref 修飾詞                                                           |
    |:------:|:-----------------------------------------------------------------------------------------------------------------------------------:|:-----------------------------------------------------------------------------------------------------------------------------------:|
    | 程式碼 | MyClass.cs<br>![20190503_163555](img/20190503_163555.png)<br>Program.cs<br>![20190503_163620](img/20190503_163620.png)<br>output: 1 | MyClass.cs<br>![20190503_164514](img/20190503_164514.png)<br>Program.cs<br>![20190503_164737](img/20190503_164737.png)<br>output: 2 |
    |  差異  |                                             因為 ```int``` 是 value type，所以 x 還是 1                                             |                                          因為 ```int``` 被當成 reference type，所以 x 是 2                                          |

#### Out 修飾詞

- It's a code smell in Mosh's opinion. **不建議使用**，知道有這個東西就好
- 可以使方法回傳 1 個以上的值，不過較好的作法應該是把欲回傳的多個結果作成另一個 class 的欄位，並回傳該 class 即可
- 以 C# 內建的 TryParse() 為例

    ``` csharp
    int number;
    var result = Int32.TryParse("abc", out number);

    if (result)
        Console.WriteLine(number);
    else
        Console.WriteLine("Conversion failed.");
    ```

### 欄位(Fields)

在類別中宣告的變數，用來存放類別的資料

#### 欄位初始化(Initialization)

- 可以寫在建構式中
- 也可以在宣告時就定義，其好處是無論有多少個建構式，都能保證該欄位會被初始化

    ``` csharp
    // 在建構式中作欄位初始化
    public class Person
    {
        public List<string> Orders;

        public Person()
        {
            Orders = new List<string>();
        }
    }
    ```

    ``` csharp
    // 在宣告時作欄位初始化
    public class Person
    {
        public List<string> Orders = new List<string>();
    }
    ```

#### 唯讀欄位(Read-Only Fields)

- 用來避免欄位被重複初始化(會導致第一次初始化後存放的資料被清空)
- 以下述程式碼來說明

    1. 建立一個 Customer 類別，並給予 Order 欄位，且預先作好初始化，然後在 Main() 使用

        ``` csharp
        public class Customer
        {
            public List<string> Orders = new List<string>();
        }

        partial class Program
        {
            static void Main(string[] args)
            {
                var customer = new Customer();
                customer.Order.Add("apple");
                customer.Order.Add("banana");

                Console.WriteLine(customer.Order.Count);
            }
        }

        // output:
        // 2
        ```

    2. 如果 Customer 類別裡面有個方法，不小心又初始化了一次 Order 欄位，會發生原本存放的資料被清空

        ``` csharp
        public class Customer
        {
            public List<string> Orders = new List<string>();

            public void Cashing()
            {
                // 不小心又初始化了一次 Order 欄位
                Order = new List<string>();
                ...
            }
        }

        partial class Program
        {
            static void Main(string[] args)
            {
                var customer = new Customer();
                customer.Order.Add("apple");
                customer.Order.Add("banana");

                // 在這個呼叫後，原本 Order 的資料會被清空
                customer.Cashing();

                Console.WriteLine(customer.Order.Count);
            }
        }

        // output:
        // 0
        ```

    3. 如果 Order 欄位是使用唯讀欄位，在 compile 階段就會報錯，禁止在一個以上的地方為該欄位作初始化

        ``` csharp
        public class Customer
        {
            // 使用 readonly 關鍵字
            public readonly List<string> Orders = new List<string>();

            public void Cashing()
            {
                // 這行會編譯失敗
                Order = new List<string>();
                ...
            }
        }
        ```

### 存取修飾詞(Access Modifiers)

- 用來控制類別和其成員的存取範圍
- 為了達到 OOP 概念中的封裝(Encapsulation)，將類別中的資訊隱藏
- 共有 5 種存取層級

    <table style="text-align: center;">
        <tr>
            <th></th>
            <th>public</th>
            <th>private</th>
            <th>protected</th>
            <th>internal</th>
            <th>protected internal</th>
        </tr>
        <tr>
            <td>可存取範圍</td>
            <td>到處皆可</td>
            <td>只有在 Class 中才可存取</td>
            <td colspan="3">在<a href="#%E7%B9%BC%E6%89%BFinheritance">繼承(Inheritance)</a>章節再作介紹</td>
        </tr>
    </table>

### 封裝(Encapsulation)

- 想像餐廳是一個應用程式(Application)，裡面的員工們就是類別(Class)，服務生不應插手、也不需要知道廚師的工作內容，也就是說，類別之間不需要知道彼此的細節，只要能互相溝通即可
- 實際作法是將類別中的欄位(Field)設為 private 層級，並提供 public 層級的 setter/getter 方法，讓其它類別可以透過這兩種方法來設定/取得欄位資訊
- 類別中的欄位名稱，應使用 Camel Case 並加上底線前綴，例如 ```private int _myNumber;```
- 以下述程式碼為例

    ``` csharp
    public class Person
    {
        // 欄位使用 private 層級，並使用 Camel Case 加上底線前綴作命名
        private string _name;

        // setter 使用 public 層級，供其它類別叫用
        public void SetName(string name)
        {
            if (!String.IsNullOrEmpty(name))
                this._name = name;
        }

        // getter 使用 public 層級，供其它類別叫用
        public string GetName()
        {
            return this._name;
        }
    }
    ```

### 屬性(Properties)

- 簡化封裝概念中，欄位(Field)的 setter/getter
- 屬性使用 public 層級，供其它類別叫用
- 屬性名稱應使用 Pascal Case
- 以下述程式碼為例

    ``` csharp
    public class Person
    {
        private string _name;

        // 屬性使用 public 層級
        public string Name
        {
            get { return _name; }
            set { Name = _name; }
        }
    }
    ```

#### 自動實作屬性(Auto-Implement Properties)

- 若屬性(Property)無其它邏輯，只是給值/取值，可以更進一步簡化屬性的寫法
- 在編譯階段，編譯器會自動為我們建立 private 層級的欄位(Field)
- 承上，透過 ildasm 工具去反組譯 compile 後的 exe 檔，可以看到中介語言(IL)真的會將這種寫法轉成 private 欄位和對應的 setter/getter
- 以下述程式碼為例

    ※ 小技巧：在類別中輸入「prop」即可產生自動實作屬性的程式碼片段

    ``` csharp
    public class Person
    {
        public string Name { get; set; }
    }
    ```

- 若屬性有其它邏輯，就不應該使用，例如下例的 ```Age```

    ``` csharp
    // 類別
    public class Person
    {
        public DateTime Birthday { get; set; }

        // 年齡應該是根據生日自動算出來的，有其邏輯，所以不使用自動實作，
        // 且年齡不應允許設定其值，所以不給 setter
        public int Age
        {
            get
            {
                var timeSpan = DateTime.Now - this.Birthday;
                var years = timeSpan.Days / 365;

                return years;
            }
        }
    }

    // 主程式
    partial class Program
    {
        static void Main(string[] args)
        {
            var person = new Person();
            person.Birthday = new DateTime(1990, 1, 1);
            Console.WriteLine(person.Age);
        }
    }

    // output:
    // 29
    ```

- 若自動實作的屬性，只允許 set 一次值，則將 ```set``` 加上 private 層級，並改在建構式(Constructor)設定屬性值，如此一來在建立物件時，便強迫需要設定該屬性值，且不得再作更改

    ``` csharp
    // 類別
    public class Person
    {
        // 在建構式設定 Birthday 屬性值
        public Person(DateTime birthday)
        {
            this.Birthday = birthday;
        }

        // set 前面加上 private
        public DateTime Birthday { get; private set; }
        public int Age
        {
            get
            {
                var timeSpan = DateTime.Now - this.Birthday;
                var years = timeSpan.Days / 365;

                return years;
            }
        }
    }

    // 主程式
    partial class Program
    {
        static void Main(string[] args)
        {
            // 建立物件時便必須設定 Birthday 值
            var person = new Person(new DateTime(1990, 1, 1));
            Console.WriteLine(person.Age);
        }
    }

    // output:
    // 29
    ```

### 索引子(Indexers)

- 類似屬性(Property)，當某個欄位(Field)是集合型態(Array, List or Dictionary .. etc.)，可以透過 key/value(一個 key 對應一個 value)的方式來存取該集合中的元素
- 索引子用 ```this``` 關鍵字作為名稱
- 以下述程式碼為例

    ``` csharp
    // 類別
    public class SuperHero
    {
        private Dictionary<string, string> _dictionary;

        public SuperHero()
        {
            this._dictionary = new Dictionary<string, string>();
        }

        // 用 "this" 作為名稱
        // "value" 關鍵字代表欲設定的值
        public string this[string key]
        {
            get { return _dictionary[key]; }
            set { _dictionary[key] = value; }
        }
    }

    // 主程式
    partial class Program
    {
        static void Main(string[] args)
        {
            var superHero = new SuperHero();
            superHero["Batman"] = "Bruce Wayne";
            Console.WriteLine(superHero["Batman"]);
        }
    }

    // output:
    // Bruce Wayne
    ```

## Association Between Classes

### 耦合性(Class Coupling)

- 一種衡量類別(Class)和子系統相依程度的方式

    |            |                                               高耦合<br>(Tightly Coupled)                                               |                              低耦合<br>(Loosely Coupled)                               |
    |:----------:|:-----------------------------------------------------------------------------------------------------------------------:|:--------------------------------------------------------------------------------------:|
    | 較佳的設計 |                                                                                                                         |                                           V                                            |
    |    描述    |                                 類別之間高度相依，修改一個地方就到處都要修改或重新編譯                                  |                   類別之間相依性低，任何類別都較小程度的影響其它類別                   |
    |    範例    | ![20190505_145206](img/20190505_145206.png)<br>修改 Class A 會影響到 Class B 和 Class F，也會影響到其它相依的子類別 | ![20190505_145719](img/20190505_145719.png)<br>無論修改哪個 Class 都僅會影響到本身 |

- 可以透過以下方式達成低耦合設計
    - [封裝(Encapsulation)](#封裝encapsulation)
    - 類別之間的關係，例如[繼承(Inheritance)](#繼承inheritance)和[組合(Composition)](#組合composition)
    - [介面(Interfaces)](#介面interfaces)

### 繼承(Inheritance)

- 兩個類別(Class)間的一種關係，讓其中一個類別的程式可以繼承自另一個類別
- 用英文來說是「Is-A」關係，例如「A Car **is a** Vehicle.」，「Car」就是繼承自「Vehicle」
- 繼承的優點
    - 程式可重複利用(Code re-use)
    - 多型(Polymorphic)特性
- 在 C# 中，類別只能繼承自一個類別(不能繼承多個類別)
- 在 C# 中，所有類別(即使未特別指定)都會繼承自 object 類別，所以都會有 ```Equals()```、```GetType()```、```ToString()``` .. 等方法
- 宣告時，在類別名稱後加上「:」即可作繼承
- 以下述情境為例
    1. 假設今天要設計一個簡報程式，我們會有文字、圖片、表格 .. 等等元件，但其實每個類別都會有複製、貼上、移至最上層 .. 等等共同功能，所以我們會建立一個母類別，並把這些共同功能都放在該類別中，再讓其它元件來繼承它
    2. 我們設計的 UML 如下圖
        ![20190505_160548](img/20190505_160548.png)
    3. 程式碼如下

        ``` csharp
        // Parent class
        public class PresentationObject
        {
            public int Width { get; set; }
            public int Height { get; set; }

            public void Copy() { ... }
        }

        // Child class
        public class Text : PresentationObject
        {
            public int FontSize { get; set; }
            public string FontFamily { get; set; }

            public void AddText(string str) { ... }
        }

        // Main
        partial class Program
        {
            static void Main(string[] args)
            {
                var text = new Text();
                // 可以使用母類別的類別成員
                text.Width = 10;
                text.Copy();
            }
        }
        ```

### 組合(Composition)

- 兩個類別(Class)間的一種關係，讓其中一個類別擁有另一個類別
- 用英文來說是「Has-A」關係，例如「A Car **has an** Engine.」
- 繼承的優點
    - 程式可重複利用(Code re-use)
    - 彈性
    - 具低耦合特性
- 以下述情境為例
    1. 假設今天要設計 DbMigrator 和 Installer 兩個類別，且兩個類別都需要寫 log 的功能
    2. 我們設計的 UML 如下圖
        ![20190505_161809](img/20190505_161809.png)
    3. 程式碼如下

        ``` csharp
        // 共有的功能
        public class Logger
        {
            public void Log(string message)
            {
                Console.WriteLine(message);
            }
        }

        // 類別一
        public class DbMigrator
        {
            private Logger _logger;

            public DbMigrator(Logger logger)
            {
                this._logger = logger;
            }

            public void Migrate()
            {
                _logger.Log("We are migrating.");
            }
        }

        // 類別二
        public class Installer
        {
            private Logger _logger;

            public Installer(Logger logger)
            {
                this._logger = logger;
            }

            public void Install()
            {
                _logger.Log("We are installing.");
            }
        }

        // Main
        partial class Program
        {
            static void Main(string[] args)
            {
                var dbMigrator = new DbMigrator(new Logger());
                dbMigrator.Migrate();

                var logger = new Logger();
                var installer = new Installer(logger);
                installer.Install();
            }
        }
        ```

### Favor Composition over Inheritance

- 繼承(Inheritance)的缺點
    - 容易被濫用
    - 繼承關係越長，越難維護
    - 造成高耦合
- 任何繼承關係都能轉換成組合關係
- 以下述情境為例

    |              |                       繼承(Inheritance)                       |                                      組合(Composition)                                      |
    |:------------:|:-------------------------------------------------------------:|:-------------------------------------------------------------------------------------------:|
    |    人和狗    | 人和狗都是動物<br>![20190505_165100](img/20190505_165100.png) |             人和狗都擁有動物特性<br>![20190505_165719](img/20190505_165719.png)             |
    | 加入走路功能 |  動物都會走路<br>![20190505_165251](img/20190505_165251.png)  |                 增加走路特性<br>![20190505_165814](img/20190505_165814.png)                 |
    |   加入金魚   |  金魚也是動物<br>![20190505_165417](img/20190505_165417.png)  | 金魚擁有動物特性、游泳特性，但不擁有走路特性<br>![20190505_165851](img/20190505_165851.png) |
    |     差異     | 出現錯誤，因為金魚不會走路，必須修改動物、人、狗 .. 等等類別  |             因為低耦合，所以較不易牽一髮而動全身，特性還可以作成介面(Interface)             |
