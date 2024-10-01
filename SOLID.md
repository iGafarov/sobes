Принципы SOLID — это набор из пяти принципов объектно-ориентированного программирования и проектирования, которые помогают создавать более понятный, гибкий и поддерживаемый код. Давайте рассмотрим каждый из них с примерами на Java.

Вот что входит в принципы SOLID:
- Single Responsibility Principle (Принцип единственной ответственности) - **никогда не должно быть больше одной причины изменить класс**
- Open Closed Principle (Принцип открытости/закрытости). - **программные сущности (классы, модули, функции и т.п.) должны быть открыты для расширения, но закрыты для изменения.**
- Liskov’s Substitution Principle (Принцип подстановки Барбары Лисков). - **объекты в программе можно заменить их наследниками без изменения свойств программы.**
- Interface Segregation Principle (Принцип разделения интерфейса). - **клиенты не должны быть вынуждены реализовывать методы, которые они не будут использовать.**
- Dependency Inversion Principle (Принцип инверсии зависимостей). - **зависимости внутри системы строятся на основе абстракций**

### 1. Single Responsibility Principle (Принцип единственной ответственности)
**Принцип:** У класса должна быть только одна причина для изменения, то есть у него должна быть только одна ответственность.

На каждый объект возлагается одна обязанность, полностью инкапсулированная в класс. Все сервисы класса направлены на обеспечение этой обязанности.

Такие классы всегда будет просто изменять, если это понадобится, потому что понятно, за что класс отвечает, а за что — нет. 
То есть можно будет вносить изменения и не бояться последствий — влияния на другие объекты. 
А еще подобный код гораздо проще тестировать, ведь вы покрываете тестами одну функциональность в изоляции от всех остальных.

**Пример:**
```java
class User {
    private String name;
    private String email;

    // Конструктор, геттеры и сеттеры
}

class UserRepository {
    public void save(User user) {
        // Логика сохранения пользователя в базу данных
    }
}

class UserService {
    private UserRepository userRepository;

    public UserService(UserRepository userRepository) {
        this.userRepository = userRepository;
    }

    public void registerUser(User user) {
        // Логика регистрации пользователя
        userRepository.save(user);
    }
}
```
**Объяснение:** В данном примере класс `User` отвечает только за данные пользователя, `UserRepository` — за сохранение пользователя, а `UserService` — за бизнес-логику регистрации пользователя.

### 2. Open-Closed Principle (Принцип открытости-закрытости)
**Принцип:** Программные сущности должны быть открыты для расширения, но закрыты для модификации.

Это означает, что должна быть возможность изменять внешнее поведение класса, не внося физические изменения в сам класс. 
Следуя этому принципу, классы разрабатываются так, чтобы для подстройки класса к конкретным условиям применения было достаточно расширить его и переопределить некоторые функции.

Поэтому система должна быть гибкой, с возможностью работы в переменных условиях без изменения исходного кода.


**Пример:**
```java
abstract class Shape {
    abstract double area();
}

class Circle extends Shape {
    private double radius;

    public Circle(double radius) {
        this.radius = radius;
    }

    @Override
    double area() {
        return Math.PI * radius * radius;
    }
}

class Rectangle extends Shape {
    private double width;
    private double height;

    public Rectangle(double width, double height) {
        this.width = width;
        this.height = height;
    }

    @Override
    double area() {
        return width * height;
    }
}
```
**Объяснение:** В данном примере класс `Shape` открыт для расширения (можно добавлять новые фигуры), но закрыт для модификации (не нужно изменять существующий код для добавления новых фигур).

### 3. Liskov Substitution Principle (Принцип подстановки Барбары Лисков)
**Принцип:** Объекты в программе должны быть заменяемы экземплярами их подтипов без изменения правильности выполнения программы.

Это означает, что класс, разработанный путем расширения на основании базового класса, должен переопределять его методы так, чтобы не нарушалась функциональность с точки зрения клиента. 
То есть, если разработчик расширяет ваш класс и использует его в приложении, он не должен изменять ожидаемое поведение переопределенных методов.

Подклассы должны переопределять методы базового класса так, чтобы не нарушалась функциональность с точки зрения клиента.

**Пример:**
```java
class Bird {
    public void fly() {
        System.out.println("Flying");
    }
}

class Sparrow extends Bird {
    // Наследует метод fly() от Bird
}

class Ostrich extends Bird {
    @Override
    public void fly() {
        throw new UnsupportedOperationException("Ostriches can't fly");
    }
}
```
**Объяснение:** В данном примере `Ostrich` нарушает принцип Лисков, так как не может летать, несмотря на то, что является подтипом `Bird`. Лучше было бы создать отдельный класс для нелетающих птиц.

### 4. Interface Segregation Principle (Принцип разделения интерфейса)
**Принцип:** Клиенты не должны зависеть от интерфейсов, которые они не используют.

Принцип разделения интерфейсов говорит о том, что слишком «толстые» интерфейсы необходимо разделять на более мелкие и специфические, 
чтобы клиенты мелких интерфейсов знали только о методах, необходимых в работе. В итоге, при изменении метода интерфейса не должны меняться клиенты, которые этот метод не используют.

**Пример:**
```java
interface Worker {
    void work();
}

interface Eater {
    void eat();
}

class Human implements Worker, Eater {
    @Override
    public void work() {
        System.out.println("Human working");
    }

    @Override
    public void eat() {
        System.out.println("Human eating");
    }
}

class Robot implements Worker {
    @Override
    public void work() {
        System.out.println("Robot working");
    }
}
```
**Объяснение:** В данном примере интерфейсы `Worker` и `Eater` разделены, чтобы `Robot` не был вынужден реализовывать метод `eat()`, который ему не нужен.

### 5. Dependency Inversion Principle (Принцип инверсии зависимостей)
**Принцип:** Модули верхнего уровня не должны зависеть от модулей нижнего уровня. Оба должны зависеть от абстракций. Абстракции не должны зависеть от деталей. Детали должны зависеть от абстракций.

Программное обеспечение нужно разрабатывать так, чтобы различные модули были автономными и соединялись друг с другом с помощью абстракции.

Классическое применение этого принципа — Spring framework. В рамках Spring framework все модули выполнены в виде отдельных компонентов, которые могут работать вместе. 
Они настолько автономны, что могут быть быть с такой же легкостью задействованы в других программных модулях помимо Spring framework.

Это достигнуто за счет зависимости закрытых и открытых принципов. Все модули предоставляют доступ только к абстракции, которая может использоваться в другом модуле.

**Пример:**
```java
interface MessageService {
    void sendMessage(String message);
}

class EmailService implements MessageService {
    @Override
    public void sendMessage(String message) {
        System.out.println("Email sent: " + message);
    }
}

class SMSService implements MessageService {
    @Override
    public void sendMessage(String message) {
        System.out.println("SMS sent: " + message);
    }
}

class Notification {
    private MessageService messageService;

    public Notification(MessageService messageService) {
        this.messageService = messageService;
    }

    public void notifyUser(String message) {
        messageService.sendMessage(message);
    }
}
```
**Объяснение:** В данном примере класс `Notification` зависит от абстракции `MessageService`, а не от конкретных реализаций `EmailService` или `SMSService`, что позволяет легко менять способ отправки сообщений.

Эти принципы помогают создавать более гибкие и поддерживаемые системы. Если у вас есть конкретные вопросы по какому-либо из примеров, дайте знать!

Источник: беседа с Copilot, 01.10.2024
(1) Принципы SOLID, о которых должен знать каждый разработчик. https://habr.com/ru/companies/ruvds/articles/426413/.
(2) Все, что нужно знать о принципах SOLID в Java. https://devtoday.ru/tpost/0v1bb0ie21-printsipi-solid-v-java.
(3) Руководство по принципам Solid в java - javascopes.com. https://javascopes.com/a-guide-to-solid-principles-in-java-17ne-df09a2f0/.
(4) Принципы SOLID на примерах / Хабр - Habr. https://habr.com/ru/articles/688530/.
(5) Принципы SOLID с примерами на Java - ALLINEED.RU. https://www.allineed.ru/development/java-development/82-java-solid-principles.
(6) Принципы SOLID, только понятно / Хабр - Habr. https://habr.com/ru/articles/811305/.