# Clean Coding and Refactoring

![Clean Coding and Refactoring](img/20200617_224229.png)

Course Link: <https://codewithmosh.com/p/clean-code>

---

## 糟糕的命名

### 不知所謂

無法直覺的推斷其用途

``` csharp
SqlDataReader dr1;

int od;

void Button1_Click();

class Page1 { }
```

### 無意義

在一個方法中做了太多事，導致命名有看沒有懂

``` csharp
void BeginCheckFunctionality_StoreClientSideCheckBoxIDArray();
```

### 匈牙利命名法

在 C++ 中流行的命名法，但 C# 使用 Visual Studio 開發，已經不再需要特別在變數名稱強調型別

``` csharp
int iMaxRequest;
```

### 模稜兩可

從字面推斷其用途有多種可能

``` csharp
bool MultiSelect() {}
```

### 多此一舉

多餘的贅字

``` csharp
Customer theCustomer;

List<Customer> listOfApprovedCustomers;
```

### 在 Visual Studio 重新命名

使用 F2，會一併修改所有參考到的變數

## Naming Convention

以下使用 PascalCase

- 類別名稱 (Class Name)
- 屬性 (Property)
- 方法 (Method)

以下使用 camelCase

- 私用欄位 (Private Field)
- 方法參數 (Parameter)
- 區域變數 (Local Variable)

``` csharp
public class Customer
{
    // Private Field 會額外加上底線作前綴
    private int _id;

    public string Name { get; set; }

    public void Charge(int amount)
    {
        var tax = 0;
    }
}
```

## 糟糕的 Method Signatures

方法回傳類別、方法名稱、方法參數，三者應該是要能互相呼應的

``` csharp
// 1. 方法叫 GetCustomer，結果回傳 Orange
// 2. airplane 根本不是一個數字
Orange GetCustomer(int airplane);
```

一般而言，方法參數有 boolean flag 便容易有 code smell，代表裡面會根據此 flag，作不一樣的事情，所以應該拆成兩個方法，讓他們各作各的邏輯，例如下面的 `GetUser()`

``` csharp
public User GetUser(string username, string password, bool login)
{
    if (login)
    {
        // Check if there is a user with the given username and password in db
        // If yes, set the last login date
        // and then return the user.
        var user = _dbContext.Users.SingleOrDefault(u => u.Username == username && u.Password == password);
        if (user != null)
            user.LastLogin = DateTime.Now;
        return user;
    }
    else
    {
        // Check if there is a user with the given username
        // If yes, return it, otherwise return null
        var user = _dbContext.Users.SingleOrDefault(u => u.Username == username);
        return user;
    }
}
```

應該拆成 `Login()` 和 `GetUser()`，並傳入適當的參數即可

``` csharp
public User Login(string username, string password)
{
    // Check if there is a user with the given username and password in db
    // If yes, set the last login date
    // and then return the user.
    var user = _dbContext.Users.SingleOrDefault(u => u.Username == username && u.Password == password);
    if (user != null)
        user.LastLogin = DateTime.Now;
    return user;
}

public User GetUser(string username)
{
    // Check if there is a user with the given username
    // If yes, return it, otherwise return null
    var user = _dbContext.Users.SingleOrDefault(u => u.Username == username);
    return user;
}
```

## 避免使用過長的傳入參數

無法直覺的看出所有傳入參數的意義，最好將傳入參數數量控制在 3 個以下

``` csharp
CheckNotifications(null, 1, true, false, DateTime.Now);
```

### 封裝重複出現的傳入參數

例如下面的 `dateFrom` 和 `dateTo` 出現在多個方法中

``` csharp
public class LongParameterList
{
    public IEnumerable<Reservation> GetReservations(
    DateTime dateFrom, DateTime dateTo,
    User user, int locationId,
    LocationType locationType, int? customerId = null)
    {
        // ... 省略 ...
    }

    public IEnumerable<Reservation> GetUpcomingReservations(
        DateTime dateFrom, DateTime dateTo,
        User user, int locationId,
        LocationType locationType)
    {
        // ... 省略 ...
    }

    public void CreateReservation(DateTime dateFrom, DateTime dateTo, int locationId)
    {
        // ... 省略 ...
    }
}
```

就可以將其封裝成一個獨立類別

``` csharp
public class DateRange
{
    public DateTime DateFrom { get; set; }
    public DateTime DateTo { get; set; }
}

public class LongParameterList
{
    // 改成傳入 DateRange 物件作為參數
    public IEnumerable<Reservation> GetReservations(
    DateRange dateRange,
    User user, int locationId,
    LocationType locationType, int? customerId = null)
    {
        // ... 省略 ...
    }

    public IEnumerable<Reservation> GetUpcomingReservations(
        DateRange dateRange,
        User user, int locationId,
        LocationType locationType)
    {
        // ... 省略 ...
    }

    public void CreateReservation(DateRange dateRange, int locationId)
    {
        // ... 省略 ...
    }
}
```

## 避免使用傳出參數

使用 `out` 傳出參數不夠直覺，且須在外層先宣告變數，造成混亂

``` csharp
int count = 0;
bool more = false;
var customers = GetCustomers(pageIndex, out count, out more);
```

### 利用 Tuple 取代傳出參數

例如下面的 `GetCustomers()`

``` csharp
public class OutputParameters
{
    public void DisplayCustomers()
    {
        int totalCount = 0;
        var customers = GetCustomers(1, out totalCount);

        Console.WriteLine("Total customers: " + totalCount);
        foreach (var c in customers)
            Console.WriteLine(c);
    }

    public IEnumerable<Customer> GetCustomers(int pageIndex, out int totalCount)
    {
        totalCount = 100;
        return new List<Customer>();
    }
}
```

可以改寫成

``` csharp
public class OutputParameters
{
    public void DisplayCustomers()
    {
        // 透過解構 Tuple 取得對應的值
        var tuple = GetCustomers(1);
        var customers = tuple.Item1;
        var totalCount = tuple.Item2;

        Console.WriteLine("Total customers: " + totalCount);
        foreach (var c in customers)
            Console.WriteLine(c);
    }

    // 使用 Tuple 取代原本要傳出的 totalCount
    public Tuple<IEnumerable<Customer>, int> GetCustomers(int pageIndex)
    {
        var totalCount = 100;
        return Tuple.Create((IEnumerable<Customer>)new List<Customer>(), totalCount);
    }
}
```

### 利用封裝取代傳出參數

使用 Tuple 的缺點之一是無法辨識 `Item1` 和 `Item2` 的定義，所以可以將其封裝成一個獨立類別

``` csharp
// 把結果作成一個獨立類別
public class GetCustomersResult
{
    public IEnumerable<Customer> Customers { get; set; }
    public int TotalCount { get; set; }
}

public class OutputParameters
{
    public void DisplayCustomers()
    {
        // 讓屬性名稱比起 Item1 和 Item2 來的更有意義
        var result = GetCustomers(1);
        var customers = result.Customers;
        var totalCount = result.TotalCount;

        Console.WriteLine("Total customers: " + totalCount);
        foreach (var c in customers)
            Console.WriteLine(c);
    }

    // 改成回傳 GetCustomersResult
    public GetCustomersResult GetCustomers(int pageIndex)
    {
        var totalCount = 100;
        return new GetCustomersResult
        {
            Customers = (IEnumerable<Customer>)new List<Customer>(),
            TotalCount = totalCount
        };
    }
}
```

## 避免在最一開始宣告所有變數

在需要使用的時候再宣告，例如以下的 `regularPay`、`overtimePay` 和 `grossPay`

``` csharp
public decimal CalcGross(decimal rate, decimal hours)
{
    decimal overtimeHours = 0;
    decimal regularHours = 0;
    decimal regularPay = 0;
    decimal overtimePay = 0;
    decimal grossPay = 0;

    if (_payFrequency == PayFrequency.Fortnightly)
    {
        if (hours > 80)
        {
            overtimeHours = hours - 80;
            regularHours = 80;
        }
        else
            regularHours = hours;
    }

    else if (_payFrequency == PayFrequency.Weekly)
    {
        if (hours > 40)
        {
            overtimeHours = hours - 40;
            regularHours = 40;
        }
        else
            regularHours = hours;
    }

    if (overtimeHours > 0m)
    {
        overtimePay += (rate * 1.5m) * overtimeHours;
    }

    regularPay = (regularHours * rate);
    grossPay = regularPay + overtimePay;

    return grossPay;
}
```

應該改寫成

``` csharp
public decimal CalcGross(decimal rate, decimal hours)
{
    // overtimeHours 和 regularHours 從頭用到尾，所以保留在最上面
    decimal overtimeHours = 0;
    decimal regularHours = 0;

    if (_payFrequency == PayFrequency.Fortnightly)
    {
        if (hours > 80)
        {
            overtimeHours = hours - 80;
            regularHours = 80;
        }
        else
            regularHours = hours;
    }

    else if (_payFrequency == PayFrequency.Weekly)
    {
        if (hours > 40)
        {
            overtimeHours = hours - 40;
            regularHours = 40;
        }
        else
            regularHours = hours;
    }

    // overtimePay 從這裡才開始使用，所以宣告在這裡
    decimal overtimePay = 0;
    if (overtimeHours > 0m)
    {
        overtimePay += (rate * 1.5m) * overtimeHours;
    }

    // regularPay 和 grossPay 都是在這裡才開始使用，而且可以直接指派初始值
    decimal regularPay = regularHours * rate;
    decimal grossPay = regularPay + overtimePay;
    return grossPay;
}
```

## 避免使用 Magic Number

無法辨識其定義的常數或字串，例如下面的 `status == 1` 和 `status == 2`

``` csharp
public class MagicNumbers
{
    public void ApproveDocument(int status)
    {
        if (status == 1)
        {
            // ...
        }
        else if (status == 2)
        {
            // ...
        }
    }
}
```

### 利用 Enum 取代 Magic Number

``` csharp
public enum DocumentStatus
{
    Draft = 1,
    Lodged
}

public class MagicNumbers
{
    public void ApproveDocument(DocumentStatus status)
    {
        if (status == DocumentStatus.Draft)
        {
            // ...
        }
        else if (status == DocumentStatus.Lodged)
        {
            // ...
        }
    }
}
```

## 簡化巢狀式條件

### 利用條件運算符

例如

``` csharp
if (a)
    b = someValue;
else
    b = anotherValue;
```

可以改寫成

``` csharp
b = (a) ? someValue : anotherValue;
```

但是千萬不要累加使用，例如

``` csharp
a = b ? : c ? : d ? e : f;
```

### 簡化 true/false

例如

``` csharp
if (a)
    b = true;
else
    b = false;
```

可以改寫成

``` csharp
b = (a);
```

### 合併 if

例如

``` csharp
if (a)
{
    if (b)
    {
        statement
    }
}
```

可以改寫成

``` csharp
if (a && b)
{
    statement
}
```

或是採用 Defensive Programming 的策略

``` csharp
if (!a || !b)
    return;

statement
```

### 調整條件的順序

例如

``` csharp
if (a)
{
    if (b)
    {
        isValid = true;
    }
}
if (c)
{
    if (b)
    {
        isValid = true;
    }
}
```

可以改寫成

``` csharp
if (b)
{
    if (a)
    {
        isValid = true;
    }
    if (c)
    {
        isValid = true;
    }
}
```

再更進一步合併成

``` csharp
if (b)
{
    if (a || c)
    {
        isValid = true;
    }
}
```

或是

``` csharp
if (b && (a || c))
{
    isValid = true;
}
```

## 避免使用 Switch Statement

例如下面的 `MonthlyStatement` 類別，有以下風險

- 每次有新的 case，就需要重新建置 `MonthlyStatement`，並影響所有相依的程式
- case 不斷增加，會讓 `MonthlyStatement` 越長越肥，難以維護

``` csharp
public class MonthlyStatement
{
    public float CallCost { get; set; }
    public float SmsCost { get; set; }
    public float TotalCost { get; set; }

    public void Generate(MonthlyUsage usage)
    {
        switch (usage.Customer.Type)
        {
            case CustomerType.PayAsYouGo:
                CallCost = 0.12f * usage.CallMinutes;
                SmsCost = 0.12f * usage.SmsCount;
                TotalCost = CallCost + SmsCost;
                break;

            case CustomerType.Unlimited:
                TotalCost = 54.90f;
                break;

            default:
                throw new NotSupportedException("The current customer type is not supported");
        }
    }
}

public class MonthlyUsage
{
    public Customer Customer { get; set; }
    public int CallMinutes { get; set; }
    public int SmsCount { get; set; }
}

public class Customer
{
    public CustomerType Type { get; set; }
}

public enum CustomerType
{
    PayAsYouGo = 1,
    Unlimited
}
```

### 利用多型 (Polymorphism) 取代 Switch Statement

※ 此動作為影響範圍較大的重構，建議先寫好對應的單元測試才作此重構

1. 先移動 `Generate()` 到 `MonthlyUsage`，藉此簡化 switch 的條件

    ``` csharp
    public class MonthlyStatement
    {
        public float CallCost { get; set; }
        public float SmsCost { get; set; }
        public float TotalCost { get; set; }
    }

    public class MonthlyUsage
    {
        public Customer Customer { get; set; }
        public int CallMinutes { get; set; }
        public int SmsCount { get; set; }

        // 改成回傳 MonthlyStatement，且不需要傳入參數
        public MonthlyStatement Generate()
        {
            var statement = new MonthlyStatement();

            // 簡化 switch 的條件
            switch (Customer.Type)
            {
                case CustomerType.PayAsYouGo:
                    statement.CallCost = 0.12f * CallMinutes;
                    statement.SmsCost = 0.12f * SmsCount;
                    statement.TotalCost = statement.CallCost + statement.SmsCost;
                    break;

                case CustomerType.Unlimited:
                    statement.TotalCost = 54.90f;
                    break;

                default:
                    throw new NotSupportedException("The current customer type is not supported");
            }

            return statement;
        }
    }

    public class Customer
    {
        public CustomerType Type { get; set; }
    }

    public enum CustomerType
    {
        PayAsYouGo = 1,
        Unlimited
    }
    ```

2. 再移動 `Generate()` 到 `Customer`，藉此簡化 switch 的條件

    ``` csharp
    public class MonthlyStatement
    {
        public float CallCost { get; set; }
        public float SmsCost { get; set; }
        public float TotalCost { get; set; }
    }

    public class MonthlyUsage
    {
        public Customer Customer { get; set; }
        public int CallMinutes { get; set; }
        public int SmsCount { get; set; }
    }

    public class Customer
    {
        public CustomerType Type { get; set; }

        // 傳入 MonthlyUsage
        public MonthlyStatement Generate(MonthlyUsage usage)
        {
            var statement = new MonthlyStatement();

            // 簡化 switch 的條件成單一屬性後，便可以改寫成多型
            switch (Type)
            {
                case CustomerType.PayAsYouGo:
                    statement.CallCost = 0.12f * usage.CallMinutes;
                    statement.SmsCost = 0.12f * usage.SmsCount;
                    statement.TotalCost = statement.CallCost + statement.SmsCost;
                    break;

                case CustomerType.Unlimited:
                    statement.TotalCost = 54.90f;
                    break;

                default:
                    throw new NotSupportedException("The current customer type is not supported");
            }

            return statement;
        }
    }

    public enum CustomerType
    {
        PayAsYouGo = 1,
        Unlimited
    }
    ```

3. 將 `switch(Type)` 改寫成多型

    ``` csharp
    public class MonthlyStatement
    {
        public float CallCost { get; set; }
        public float SmsCost { get; set; }
        public float TotalCost { get; set; }
    }

    public class MonthlyUsage
    {
        public Customer Customer { get; set; }
        public int CallMinutes { get; set; }
        public int SmsCount { get; set; }
    }

    // 將 Customer 作成抽象類別
    public abstract class Customer
    {
        // 不再需要
        //public CustomerType Type { get; set; }

        // 將 Generate 作成抽象方法
        public abstract MonthlyStatement Generate(MonthlyUsage usage);
    }

    // 不再需要
    //public enum CustomerType
    //{
    //    PayAsYouGo = 1,
    //    Unlimited
    //}

    // 新增 PayAsYouGoCustomer 類別，並繼承自 Customer
    public class PayAsYouGoCustomer : Customer
    {
        // 覆寫 Generate()
        public override MonthlyStatement Generate(MonthlyUsage usage)
        {
            var statement = new MonthlyStatement();

            // 取代原本的 switch，只保留此實作相關的邏輯
            statement.CallCost = 0.12f * usage.CallMinutes;
            statement.SmsCost = 0.12f * usage.SmsCount;
            statement.TotalCost = statement.CallCost + statement.SmsCost;

            return statement;
        }
    }

    // 新增 UnlimitedCustomer 類別，並繼承自 Customer
    public class UnlimitedCustomer : Customer
    {
        // 覆寫 Generate()
        public override MonthlyStatement Generate(MonthlyUsage usage)
        {
            var statement = new MonthlyStatement();

            // 取代原本的 switch，只保留此實作相關的邏輯
            statement.TotalCost = 54.90f;

            return statement;
        }
    }
    ```

## 重複的程式碼

重複的程式碼戶造成維護困難，改一處、漏 N 處，應擷取成私用函式，或是獨立成公用類別的方法
