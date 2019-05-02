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
