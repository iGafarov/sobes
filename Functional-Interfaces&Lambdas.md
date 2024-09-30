## Содержание
- [Функциональные интерфейсы](#функциональные-интерфейсы)
- [Лямбда-выражения](#лямбда-выражения)
- [Примеры использования](#примеры-использования)
    - [Пример с использованием Runnable](#пример-с-использованием-runnable)
    - [Пример с использованием Comparator](#пример-с-использованием-comparator)
    - [Пример с использованием Function](#пример-с-использованием-function)
- [Заключение](#заключение)


Функциональные интерфейсы и лямбда-выражения тесно связаны в Java, особенно начиная с версии 8. Давайте разберем каждую из этих концепций и посмотрим, как они работают вместе.

### Функциональные интерфейсы
Функциональный интерфейс — это интерфейс, который содержит только один абстрактный метод. Такие интерфейсы могут иметь любое количество методов по умолчанию или статических методов, но только один абстрактный метод. Примеры функциональных интерфейсов в Java включают `Runnable`, `Callable`, `Comparator` и интерфейсы из пакета `java.util.function`.

Пример функционального интерфейса:
```java
@FunctionalInterface
public interface MyFunctionalInterface {
    void execute();
}
```
- **Объяснение:** Аннотация `@FunctionalInterface` не обязательна, но она помогает компилятору и разработчикам понять, что этот интерфейс предназначен для использования в функциональном стиле.

### Лямбда-выражения
Лямбда-выражения — это краткий способ реализации функциональных интерфейсов. Они позволяют писать более компактный и читаемый код, особенно при работе с коллекциями и потоками данных.

Пример лямбда-выражения:
```java
MyFunctionalInterface myFunc = () -> System.out.println("Hello, World!");
myFunc.execute();
```
- **Объяснение:** Лямбда-выражение `() -> System.out.println("Hello, World!")` реализует метод `execute` функционального интерфейса `MyFunctionalInterface`.

### Примеры использования

#### Пример с использованием `Runnable`
До Java 8:
```java
Runnable runnable = new Runnable() {
    @Override
    public void run() {
        System.out.println("Running in a thread");
    }
};
new Thread(runnable).start();
```
С Java 8 и лямбда-выражениями:
```java
Runnable runnable = () -> System.out.println("Running in a thread");
new Thread(runnable).start();
```
- **Объяснение:** Лямбда-выражение `() -> System.out.println("Running in a thread")` заменяет анонимный класс, делая код более компактным.

#### Пример с использованием `Comparator`
До Java 8:
```java
Comparator<String> comparator = new Comparator<String>() {
    @Override
    public int compare(String s1, String s2) {
        return s1.compareTo(s2);
    }
};
List<String> list = Arrays.asList("b", "a", "c");
Collections.sort(list, comparator);
```
С Java 8 и лямбда-выражениями:
```java
Comparator<String> comparator = (s1, s2) -> s1.compareTo(s2);
List<String> list = Arrays.asList("b", "a", "c");
Collections.sort(list, comparator);
```
- **Объяснение:** Лямбда-выражение `(s1, s2) -> s1.compareTo(s2)` заменяет анонимный класс, делая код более читаемым и кратким.

### Пример с использованием `Function`
Функциональный интерфейс `Function` из пакета `java.util.function`:
```java
import java.util.function.Function;

public class Main {
    public static void main(String[] args) {
        Function<Integer, Integer> square = x -> x * x;
        System.out.println(square.apply(5)); // 25
    }
}
```
- **Объяснение:** Лямбда-выражение `x -> x * x` реализует метод `apply` интерфейса `Function`, который принимает один аргумент и возвращает результат.

### Заключение
Функциональные интерфейсы и лямбда-выражения работают вместе, чтобы сделать код более компактным и выразительным. Лямбда-выражения позволяют легко и быстро реализовывать функциональные интерфейсы, что особенно полезно при работе с коллекциями и потоками данных.

Если у вас есть дополнительные вопросы или нужны еще примеры, дайте знать!

Источник: беседа с Copilot, 30.09.2024
(1) Lambda Expressions and Functional Interfaces: Tips and Best Practices .... https://www.baeldung.com/java-8-lambda-expressions-tips.
(2) Functional Interface and Lambda Expressions in Java 8 - DZone. https://dzone.com/articles/functional-interface-and-lambda-expression-in-java.
(3) Lambda Expression vs. Anonymous Inner Class - Baeldung. https://www.baeldung.com/java-lambdas-vs-anonymous-class.
(4) java.util.function (Java Platform SE 8 ) - Oracle. https://docs.oracle.com/javase/8/docs/api/java/util/function/package-summary.html.
(5) Java Lambda Expressions - Javatpoint. https://www.javatpoint.com/java-lambda-expressions.