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

- 私有欄位 (Private Field)
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
// 方法叫 GetCustomer，結果回傳 Orange，
// airplane 根本不是一個數字
Orange GetCustomer(int airplane);
```

``` csharp
// 一般而言，方法參數有 boolean flag 便容易有 code smell，
// 代表裡面會根據此 flag，作不一樣的事情，
// 所以應該拆成兩個方法，讓他們各作各的邏輯
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
