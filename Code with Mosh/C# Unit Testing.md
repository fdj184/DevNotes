# Unit Testing for C# Developers

![C# Unit Testing](img/20200527_154450.png)

Course Link: <https://codewithmosh.com/p/unit-testing-for-csharp-developers>

---

## Getting Started

### 自動化測試的優點

- 用更少的時間，頻繁測試程式碼
- 部署程式碼時更有信心
- 重構時更有信心

### 測試的類型

- 單元測試 (Unit Test): 測試應用程式中的一個單元，且不包含外在的相依資源
- 整合測試 (Integration Test): 測試應用程式，且包含外在的相依資源
- 端對端測試 (End-to-End Test): 透過 UI 測試整個應用程式，例如 Selenium

  ![20200527_162113](img/20200527_162113.png)

### 測試工具

- NUnit
- MSTest
- xUnit

選哪個工具不重要，重要的是心法

### MSTest

- 測試函式命名習慣為 `{原函式名稱}_{情境}_{預期結果}`，例如 `CanBeCancelledBy_UserIsAdmin_ReturnsTrue()`
- 函式內會依序分三個部分
    1. Arrange: 初始化物件
    2. Act: 對物件採取動作，例如呼叫方法 (Method)
    3. Assert: 驗證結果是否與預期相符
- 測試 class 使用 `[TestClass]` 屬性
- 測試 function 使用 `[TestMethod]` 屬性

    ``` csharp
    [TestClass]
    public class ReservationTests
    {
        [TestMethod]
        public void CanBeCancelledBy_UserIsAdmin_ReturnsTrue()
        {
            // Arrange
            var reservation = new Reservation();

            // Act
            var result = reservation.CanBeCancelledBy(new User() { IsAdmin = true });

            // Assert
            Assert.IsTrue(result);
        }
    }
    ```

### NUnit

- 測試 class 使用 `[TestFixture]` 屬性
- 測試 function 使用 `[Test]` 屬性

    ``` csharp
    [TestFixture]
    public class ReservationTests
    {
        [Test]
        public void CanBeCancelledBy_UserIsAdmin_ReturnsTrue()
        {
            // Arrange
            var reservation = new Reservation();

            // Act
            var result = reservation.CanBeCancelledBy(new User() { IsAdmin = true });

            // Assert 可以從以下三種寫法選一種
            Assert.IsTrue(result);
            Assert.That(result, Is.True);
            Assert.That(result == true);
        }
    }
    ```

- 測試函式彼此間不應該共用實體，以下為錯誤示範

    ``` csharp
    [TestFixture]
    public class MathTests
    {
        // We shouldn't share the same instance between test functions
        private Math _math;

        [Test]
        public void Max_FirstArgumentIsGreater_ReturnTheFirstArgument()
        {
            var result = _math.Max(2, 1);

            Assert.That(result, Is.EqualTo(2));
        }

        [Test]
        public void Max_SecondArgumentIsGreater_ReturnTheSecondArgument()
        {
            var result = _math.Max(1, 2);

            Assert.That(result, Is.EqualTo(2));
        }
    }
    ```

### Test-driven Development (TDD)

在寫正式 code 之前先寫測試專案，會經過以下三個步驟

1. 先寫出一個會失敗的測試案例
2. 寫出最簡單的正式 code 以通過測試案例
3. 需要的話重構正式 code

## Fundamentals of Unit Testing

### 良好的單元測試之特徵

- 乾淨、可讀、可維護
- 不包含任何邏輯，例如 if else 或 for loop

### What to Test and What Not to Test

#### What to Test

大致上可以分為兩類函式

|          |                         Query Function                         |                  Command Function                   |
|----------|----------------------------------------------------------------|-----------------------------------------------------|
|   定義   | 不一定指查詢 DB，只要目的是取得資料的都可以當成 query function | 改變記憶體或 DB 狀態、或操控外部相依服務之 function |
| 測試方法 |                    驗證所有路徑可能回傳的值                    |      驗證物件的狀態、是否正確呼叫外部相依服務       |

#### What Not to Test

- 程式語言本身的功能
- 第三方套件之功能

### 單元測試的命名和架構

|          |  正式專案   |                   測試專案                   |
|----------|-------------|----------------------------------------------|
| 專案名稱 |  TestNinja  |          TestNinja<b>.UnitTests</b>          |
| 類別名稱 | Reservation |           Reservation<b>Tests</b>            |
| 方法名稱 |             | {MethodName}\_{Scenario}\_{ExpectedBehavior} |

### NUnit 常用屬性

#### [SetUp]

NUnit 在進入每個測試函式前，都會先呼叫有 `[SetUp]` 屬性的函式，所以可以在這裡建立新實體，這樣就可以避免在每個測試函式中都要作 new 實體的動作

``` csharp
[TestFixture]
public class MathTests
{
    private Math _math;

    // We can use the [SetUp] function to create a new instance
    // before every time we enter any test function
    [SetUp]
    public void SetUp()
    {
        _math = new Math();
    }

    [Test]
    public void Max_FirstArgumentIsGreater_ReturnTheFirstArgument()
    {
        var result = _math.Max(2, 1);

        Assert.That(result, Is.EqualTo(2));
    }

    [Test]
    public void Max_SecondArgumentIsGreater_ReturnTheSecondArgument()
    {
        var result = _math.Max(1, 2);

        Assert.That(result, Is.EqualTo(2));
    }
}
```

#### [TearDown]

NUnit 在離開每個測試函式後，都會呼叫有 `[TearDown]` 屬性的函式

#### [TestCase]

透過 `[TestCase](arg1, arg2 ..)` 屬性可以將測試函式參數化

``` csharp
[TestFixture]
public class MathTests
{
    private Math _math;

    [SetUp]
    public void SetUp()
    {
        _math = new Math();
    }

    [Test]
    [TestCase(2, 1, 2)]
    [TestCase(1, 2, 2)]
    [TestCase(1, 1, 1)]
    public void Max_WhenCalled_ReturnTheGreaterArguments(int a, int b, int expectedResult)
    {
        var result = _math.Add(a, b);

        Assert.That(result, Is.EqualTo(expectedResult));
    }
}
```

#### [Ignore]

透過 `[Ignore]("reason")` 屬性可以暫時略過該測試函式
