## Содержание
1. [Аргументы в Java передаются по значению](#аргументы-в-java-передаются-по-значению)

## Аргументы в Java передаются по значению
Да, в Java аргументы передаются по значению. Это означает, что при вызове метода передается копия значения аргумента, а не сам аргумент. Давайте рассмотрим несколько примеров, чтобы понять, как это работает.

### Пример с примитивными типами данных
Когда передается примитивный тип данных, метод получает копию значения, и любые изменения внутри метода не влияют на оригинальную переменную.

```java
public class PassByValueExample {
    public static void main(String[] args) {
        int number = 10;
        modifyNumber(number);
        System.out.println("Number after modification: " + number); // Выведет 10
    }

    public static void modifyNumber(int num) {
        num = num + 10;
    }
}
```

В этом примере значение переменной `number` не изменится, так как метод `modifyNumber` работает с копией значения.

### Пример с объектами
Когда передается объект, метод получает копию ссылки на объект. Это означает, что изменения в свойствах объекта внутри метода будут видны снаружи метода, так как обе ссылки указывают на один и тот же объект.

```java
public class PassByValueExample {
    public static void main(String[] args) {
        Person person = new Person("John");
        modifyPerson(person);
        System.out.println("Person's name after modification: " + person.getName()); // Выведет "Doe"
    }

    public static void modifyPerson(Person p) {
        p.setName("Doe");
    }
}

class Person {
    private String name;

    public Person(String name) {
        this.name = name;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }
}
```

В этом примере имя объекта `person` изменится, так как метод `modifyPerson` работает с копией ссылки на объект, а не с копией самого объекта.

### Пример с изменением ссылки на объект
Если попытаться изменить саму ссылку на объект внутри метода, это не повлияет на оригинальную ссылку.

```java
public class PassByValueExample {
    public static void main(String[] args) {
        Person person = new Person("John");
        changeReference(person);
        System.out.println("Person's name after changing reference: " + person.getName()); // Выведет "John"
    }

    public static void changeReference(Person p) {
        p = new Person("Doe");
    }
}
```

В этом примере ссылка на объект `person` не изменится, так как метод `changeReference` работает с копией ссылки и изменения этой копии не влияют на оригинальную ссылку.

Эти примеры демонстрируют, как работает передача аргументов по значению в Java. Если у тебя есть еще вопросы или нужны дополнительные примеры, дай знать!

Источник: беседа с Copilot, 06.10.2024
(1) Java Pass By Reference And Pass By Value With Examples. https://www.softwaretestinghelp.com/java/java-pass-by-reference-and-pass-by-value/.
(2) Pass-By-Value as a Parameter Passing Mechanism in Java. https://www.baeldung.com/java-pass-by-value-or-pass-by-reference.
(3) How to Pass Arguments by Value in Java - Squash. https://www.squash.io/how-to-pass-arguments-by-value-in-java/.
(4) Java Pass by Value - Javatpoint. https://www.javatpoint.com/java-pass-by-value.
(5) Different Ways to Achieve Pass By Reference in Java. https://www.geeksforgeeks.org/different-ways-to-achieve-pass-by-reference-in-java/.