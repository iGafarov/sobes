В Java существует несколько методологий тестирования кода, каждая из которых имеет свои особенности и примеры использования. Вот основные из них:

### 1. Unit Testing (Модульное тестирование)
Модульное тестирование направлено на проверку отдельных компонентов или методов в изоляции от остальной части системы. Основной инструмент для модульного тестирования в Java — это **JUnit**.

**Пример:**
```java
import org.junit.jupiter.api.Test;
import static org.junit.jupiter.api.Assertions.assertEquals;

public class CalculatorTest {
    @Test
    public void testAddition() {
        Calculator calculator = new Calculator();
        assertEquals(5, calculator.add(2, 3));
    }
}
```

### 2. Integration Testing (Интеграционное тестирование)
Интеграционное тестирование проверяет взаимодействие между различными компонентами системы. Для этого часто используются фреймворки, такие как **Spring Test** и **TestNG**.

**Пример:**
```java
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.context.SpringBootTest;
import org.springframework.test.context.junit4.SpringRunner;
import org.junit.jupiter.api.Test;
import static org.junit.jupiter.api.Assertions.assertNotNull;

@SpringBootTest
public class UserServiceIntegrationTest {

    @Autowired
    private UserService userService;

    @Test
    public void testUserService() {
        User user = userService.findUserById(1L);
        assertNotNull(user);
    }
}
```

### 3. Mock Testing (Тестирование с использованием моков)
Mock-тестирование используется для имитации поведения зависимостей, что позволяет тестировать компоненты в изоляции. Основной инструмент для этого — **Mockito**.

**Пример:**
```java
import static org.mockito.Mockito.*;
import org.junit.jupiter.api.Test;

public class UserServiceTest {

    @Test
    public void testFindUser() {
        UserRepository mockRepository = mock(UserRepository.class);
        UserService userService = new UserService(mockRepository);

        User user = new User(1L, "John Doe");
        when(mockRepository.findById(1L)).thenReturn(Optional.of(user));

        User result = userService.findUserById(1L);
        assertEquals("John Doe", result.getName());
    }
}
```

### 4. End-to-End Testing (Конечное тестирование)
Конечное тестирование проверяет всю систему целиком, включая пользовательский интерфейс, серверную часть и базу данных. Для этого часто используются инструменты, такие как **Selenium**.

**Пример:**
```java
import org.openqa.selenium.WebDriver;
import org.openqa.selenium.chrome.ChromeDriver;
import org.openqa.selenium.By;
import org.junit.jupiter.api.Test;
import static org.junit.jupiter.api.Assertions.assertEquals;

public class LoginTest {

    @Test
    public void testLogin() {
        WebDriver driver = new ChromeDriver();
        driver.get("http://example.com/login");

        driver.findElement(By.name("username")).sendKeys("user");
        driver.findElement(By.name("password")).sendKeys("password");
        driver.findElement(By.name("submit")).click();

        String welcomeMessage = driver.findElement(By.id("welcome")).getText();
        assertEquals("Welcome, user!", welcomeMessage);

        driver.quit();
    }
}
```

Эти методологии помогают обеспечить высокое качество кода и его надежность. Если у тебя есть вопросы по конкретным инструментам или примерам, дай знать!

Источник: беседа с Copilot, 07.10.2024
(1) Best Practices for Unit Testing in Java - Baeldung. https://www.baeldung.com/java-unit-testing-best-practices.
(2) Java Unit Testing: A Complete Guide with Examples and Best Practices. https://www.softwaretestingstuff.com/java-unit-testing.
(3) Effective Techniques for Java Testing | Java Tech Blog. https://javanexus.com/blog/effective-techniques-java-testing.
(4) Java Testing in Visual Studio Code. https://code.visualstudio.com/docs/java/java-testing.
(5) Java Unit Testing Tutorial - Examples Java Code Geeks. https://examples.javacodegeeks.com/java-unit-testing-tutorial/.