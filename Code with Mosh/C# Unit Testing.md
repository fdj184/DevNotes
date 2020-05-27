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

### Test-driven Development (TDD)

在寫正式 code 之前先寫測試專案，會經過以下三個步驟

1. 先寫出一個會失敗的測試案例
2. 寫出最簡單的正式 code 以通過測試案例
3. 需要的話重構正式 code
