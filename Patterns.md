Конечно! Вот конспект с подробным объяснением паттернов программирования на Java, включая содержание для удобной навигации:

# Конспект по паттернам программирования на Java

## Содержание
1. Введение
2. Порождающие паттерны
   - Singleton
   - Factory Method
   - Abstract Factory
   - Builder
   - Prototype
3. Структурные паттерны
   - Adapter
   - Bridge
   - Composite
   - Decorator
   - Facade
   - Flyweight
   - Proxy
4. Поведенческие паттерны
   - Chain of Responsibility
   - Command
   - Interpreter
   - Iterator
   - Mediator
   - Memento
   - Observer
   - State
   - Strategy
   - Template Method
   - Visitor
5. Заключение

## 1. Введение
Паттерны проектирования — это повторяющиеся решения общих проблем проектирования в программировании. Они помогают сделать код более гибким, модульным и удобным для сопровождения.

## 2. Порождающие паттерны

### Singleton
**Описание:** Гарантирует, что у класса есть только один экземпляр, и предоставляет глобальную точку доступа к нему.
**Пример кода:**
```java
public class Singleton {
    private static Singleton instance;

    private Singleton() {}

    public static Singleton getInstance() {
        if (instance == null) {
            instance = new Singleton();
        }
        return instance;
    }
}
```

### Factory Method
**Описание:** Определяет интерфейс для создания объекта, но позволяет подклассам изменять тип создаваемого объекта.
**Пример кода:**
```java
public abstract class Product {
    public abstract void use();
}

public class ConcreteProduct extends Product {
    @Override
    public void use() {
        System.out.println("Using ConcreteProduct");
    }
}

public abstract class Creator {
    public abstract Product factoryMethod();

    public void someOperation() {
        Product product = factoryMethod();
        product.use();
    }
}

public class ConcreteCreator extends Creator {
    @Override
    public Product factoryMethod() {
        return new ConcreteProduct();
    }
}
```

### Abstract Factory
**Описание:** Предоставляет интерфейс для создания семейств взаимосвязанных или взаимозависимых объектов без указания их конкретных классов.
**Пример кода:**
```java
public interface GUIFactory {
    Button createButton();
    Checkbox createCheckbox();
}

public class WinFactory implements GUIFactory {
    @Override
    public Button createButton() {
        return new WinButton();
    }

    @Override
    public Checkbox createCheckbox() {
        return new WinCheckbox();
    }
}

public class MacFactory implements GUIFactory {
    @Override
    public Button createButton() {
        return new MacButton();
    }

    @Override
    public Checkbox createCheckbox() {
        return new MacCheckbox();
    }
}
```

### Builder
**Описание:** Разделяет создание сложного объекта от его представления, так что один и тот же процесс построения может создавать разные представления.
**Пример кода:**
```java
public class Product {
    private String part1;
    private String part2;

    public void setPart1(String part1) {
        this.part1 = part1;
    }

    public void setPart2(String part2) {
        this.part2 = part2;
    }
}

public abstract class Builder {
    protected Product product = new Product();

    public abstract void buildPart1();
    public abstract void buildPart2();

    public Product getResult() {
        return product;
    }
}

public class ConcreteBuilder extends Builder {
    @Override
    public void buildPart1() {
        product.setPart1("Part1");
    }

    @Override
    public void buildPart2() {
        product.setPart2("Part2");
    }
}

public class Director {
    private Builder builder;

    public void setBuilder(Builder builder) {
        this.builder = builder;
    }

    public Product construct() {
        builder.buildPart1();
        builder.buildPart2();
        return builder.getResult();
    }
}
```

### Prototype
**Описание:** Указывает виды создаваемых объектов с помощью экземпляра-прототипа и создает новые объекты путем копирования этого прототипа.
**Пример кода:**
```java
public abstract class Prototype implements Cloneable {
    public Prototype clone() throws CloneNotSupportedException {
        return (Prototype) super.clone();
    }
}

public class ConcretePrototype extends Prototype {
    private String field;

    public void setField(String field) {
        this.field = field;
    }

    public String getField() {
        return field;
    }
}
```

## 3. Структурные паттерны

### Adapter
**Описание:** Преобразует интерфейс класса в другой интерфейс, который ожидают клиенты.
**Пример кода:**
```java
public interface Target {
    void request();
}

public class Adaptee {
    public void specificRequest() {
        System.out.println("Specific request");
    }
}

public class Adapter implements Target {
    private Adaptee adaptee;

    public Adapter(Adaptee adaptee) {
        this.adaptee = adaptee;
    }

    @Override
    public void request() {
        adaptee.specificRequest();
    }
}
```

### Bridge
**Описание:** Разделяет абстракцию и реализацию так, чтобы они могли изменяться независимо.
**Пример кода:**
```java
public abstract class Abstraction {
    protected Implementor implementor;

    protected Abstraction(Implementor implementor) {
        this.implementor = implementor;
    }

    public abstract void operation();
}

public interface Implementor {
    void implementation();
}

public class ConcreteImplementorA implements Implementor {
    @Override
    public void implementation() {
        System.out.println("Implementation A");
    }
}

public class ConcreteImplementorB implements Implementor {
    @Override
    public void implementation() {
        System.out.println("Implementation B");
    }
}

public class RefinedAbstraction extends Abstraction {
    public RefinedAbstraction(Implementor implementor) {
        super(implementor);
    }

    @Override
    public void operation() {
        implementor.implementation();
    }
}
```

### Composite
**Описание:** Составляет объекты в древовидные структуры для представления иерархий часть-целое.
**Пример кода:**
```java
public interface Component {
    void operation();
}

public class Leaf implements Component {
    @Override
    public void operation() {
        System.out.println("Leaf operation");
    }
}

public class Composite implements Component {
    private List<Component> children = new ArrayList<>();

    public void add(Component component) {
        children.add(component);
    }

    public void remove(Component component) {
        children.remove(component);
    }

    @Override
    public void operation() {
        for (Component child : children) {
            child.operation();
        }
    }
}
```

### Decorator
**Описание:** Динамически добавляет объектам новые обязанности.
**Пример кода:**
```java
public interface Component {
    void operation();
}

public class ConcreteComponent implements Component {
    @Override
    public void operation() {
        System.out.println("ConcreteComponent operation");
    }
}

public abstract class Decorator implements Component {
    protected Component component;

    public Decorator(Component component) {
        this.component = component;
    }

    @Override
    public void operation() {
        component.operation();
    }
}

public class ConcreteDecorator extends Decorator {
    public ConcreteDecorator(Component component) {
        super(component);
    }

    @Override
    public void operation() {
        super.operation();
        System.out.println("ConcreteDecorator operation");
    }
}
```

### Facade
**Описание:** Предоставляет унифицированный интерфейс к набору интерфейсов в подсистеме.
**Пример кода:**
```java
public class Subsystem1 {
    public void operation1() {
        System.out.println("Subsystem1 operation1");
    }
}

public class Subsystem2 {
    public void operation2() {
        System.out.println("Subsystem2 operation2");
    }
}

public class Facade {
    private Subsystem1 subsystem1 = new Subsystem1();
    private Subsystem2 subsystem2 = new Subsystem2();

    public void operation() {
        subsystem1.operation1();
        subsystem2.operation2();
    }
}
```

### Flyweight
**Описание:** Использует разделение для эффективной поддержки множества мелких объектов.
**Пример кода:**
```java
public interface Flyweight {
    void operation(String extrinsicState);
}

public class ConcreteFlyweight implements Flyweight {
    private String intrinsicState;

    public ConcreteFlyweight(String intrinsicState) {
        this.intrinsicState = intrinsicState;
    }

    @Override
    public void operation(String extrinsicState) {
        System.out.println("IntrinsicState: " + intrinsicState + ", ExtrinsicState: " + extrinsicState);
    }
}

public class FlyweightFactory {
    private Map<String, Flyweight> flyweights = new HashMap<>();

    public Flyweight getFlyweight(String key) {
        if (!flyweights.containsKey(key)) {
            flyweights.put(key, new ConcreteFlyweight(key));
        }
        return flyweights.get(key);
    }
}
```

### Proxy
**Описание:** Предоставляет суррогат или заместителя другого объекта для контроля доступа к нему.
**Пример кода:**
```java
public interface Subject {
    void request();
}

public class RealSubject implements Subject {
    @Override
    public void request() {
        System.out.println("RealSubject request");
    }
}

public class Proxy implements Subject {
    private RealSubject realSubject;

    @Override
    public void request() {
        if (realSubject == null) {
            realSubject = new RealSubject();
        }
        realSubject.request();
    }
}
```

## 4. Поведенческие паттерны

### Chain of Responsibility
**Описание:** Позволяет передавать запросы последовательно по цепочке обработчиков.
**Пример кода:**
```java
public abstract class Handler {
    protected Handler successor;

    public void setSuccessor(Handler successor) {
        this.successor = successor;
    }

    public abstract void handleRequest(String request);
}

public class ConcreteHandler1 extends Handler {
    @Override
    public void handleRequest(String request) {
        if (request.equals("Request1")) {
            System.out.println("Handled by ConcreteHandler1");
        } else if (successor != null) {
            successor.handleRequest(request);
        }
    }
}

public class ConcreteHandler2 extends Handler {
    @Override
    public void handleRequest(String request) {
        if (request.equals("Request2")) {
            System.out.println("Handled by ConcreteHandler2");
        } else if (successor != null) {
            successor.handleRequest(request);
        }
    }
}
```

### Command
**Описание:** Инкапсулирует запрос как объект, позволяя параметризовать клиентов с различными запросами.
**Пример кода:**
```java
public interface Command {
    void execute();
}

public class ConcreteCommand implements Command {
    private Receiver receiver;

    public ConcreteCommand(Receiver receiver) {
        this.receiver = receiver;
    }

    @Override
    public void execute() {
        receiver.action();
    }
}

public class Receiver {
    public void action() {
        System.out.println("Receiver action");
    }
}

public class Invoker {
    private Command command;

    public void setCommand(Command command) {
        this.command = command;
    }

    public void executeCommand() {
        command.execute();
    }
}
```

### Interpreter
**Описание:** Определяет грамматику для языка и интерпретатор предложений этого языка.
**Пример кода:**
```java
public interface Expression {
    boolean interpret(String context);
}

public class TerminalExpression implements Expression {
    private String data;

    public TerminalExpression(String data) {
        this.data = data;
    }

    @Override
    public boolean interpret(String context) {
        return context.contains(data);
    }
}

public class OrExpression implements Expression {
    private Expression expr1;
    private Expression expr2;

    public OrExpression(Expression expr1, Expression expr2) {
        this.expr1 = expr1;
        this.expr2 = expr2;
    }

    @Override
    public boolean interpret(String context) {
        return expr1.interpret(context) || expr2.interpret(context);
    }
}
```

### Iterator
**Описание:** Предоставляет способ последовательного доступа ко всем элементам составного объекта без раскрытия его внутреннего представления.
**Пример кода:**
```java
public interface Iterator {
    boolean hasNext();
    Object next();
}

public class ConcreteIterator implements Iterator {
    private List<Object> items;
    private int position = 0;

    public ConcreteIterator(List<Object> items) {
        this.items = items;
    }

    @Override
    public boolean hasNext() {
        return position < items.size();
    }

    @Override
    public Object next() {
        return items.get(position++);
    }
}

public class Aggregate {
    private List<Object> items = new ArrayList<>();

    public void addItem(Object item) {
        items.add(item);
    }

    public Iterator createIterator() {
        return new ConcreteIterator(items);
    }
}
```

### Mediator
**Описание:** Определяет объект, который инкапсулирует способ взаимодействия множества объектов.
**Пример кода:**
```java
public interface Mediator {
    void notify(Component sender, String event);
}

public class ConcreteMediator implements Mediator {
    private Component1 component1;
    private Component2 component2;

    public void setComponent1(Component1 component1) {
        this.component1 = component1;
    }

    public void setComponent2(Component2 component2) {
        this.component2 = component2;
    }

    @Override
    public void notify(Component sender, String event) {
        if (event.equals("Event1")) {
            component2.doSomething();
        } else if (event.equals("Event2")) {
            component1.doSomething();
        }
    }
}

public class Component {
    protected Mediator mediator;

    public Component(Mediator mediator) {
        this.mediator = mediator;
    }
}

public class Component1 extends Component {
    public Component1(Mediator mediator) {
        super(mediator);
    }

    public void doSomething() {
        System.out.println("Component1 does something");
    }
}

public class Component2 extends Component {
    public Component2(Mediator mediator) {
        super(mediator);
    }

    public void doSomething() {
        System.out.println("Component2 does something");
    }
}
```

### Memento
**Описание:** Захватывает и внешне сохраняет внутреннее состояние объекта так, чтобы позднее можно было восстановить его в это состояние.
**Пример кода:**
```java
public class Memento {
    private String state;

    public Memento(String state) {
        this.state = state;
    }

    public String getState() {
        return state;
    }
}

public class Originator {
    private String state;

    public void setState(String state) {
        this.state = state;
    }

    public String getState() {
        return state;
    }

    public Memento saveStateToMemento() {
        return new Memento(state);
    }

    public void getStateFromMemento(Memento memento) {
        state = memento.getState();
    }
}

public class Caretaker {
    private List<Memento> mementoList = new ArrayList<>();

    public void add(Memento state) {
        mementoList.add(state);
    }

    public Memento get(int index) {
        return mementoList.get(index);
    }
}
```

### Observer
**Описание:** Определяет зависимость "один ко многим" между объектами так, что при изменении состояния одного объекта все зависящие от него оповещаются и обновляются автоматически.
**Пример кода:**
```java
public interface Observer {
    void update(String state);
}

public class ConcreteObserver implements Observer {
    private String observerState;

    @Override
    public void update(String state) {
        observerState = state;
        System.out.println("Observer state updated to: " + observerState);
    }
}

public class Subject {
    private List<Observer> observers = new ArrayList<>();
    private String state;

    public void attach(Observer observer) {
        observers.add(observer);
    }

    public void detach(Observer observer) {
        observers.remove(observer);
    }

    public void notifyObservers() {
        for (Observer observer : observers) {
            observer.update(state);
        }
    }

    public void setState(String state) {
        this.state = state;
        notifyObservers();
    }
}
```

### State
**Описание:** Позволяет объекту изменять свое поведение в зависимости от его состояния.
**Пример кода:**
```java
public interface State {
    void handle(Context context);
}

public class ConcreteStateA implements State {
    @Override
    public void handle(Context context) {
        System.out.println("State A handling request.");
        context.setState(new ConcreteStateB());
    }
}

public class ConcreteStateB implements State {
    @Override
    public void handle(Context context) {
        System.out.println("State B handling request.");
        context.setState(new ConcreteStateA());
    }
}

public class Context {
    private State state;

    public Context(State state) {
        this.state = state;
    }

    public void setState(State state) {
        this.state = state;
    }

    public void request() {
        state.handle(this);
    }
}
```

### Strategy
**Описание:** Определяет семейство алгоритмов, инкапсулирует каждый из них и делает их взаимозаменяемыми.
**Пример кода:**
```java
public interface Strategy {
    void execute();
}

public class ConcreteStrategyA implements Strategy {
    @Override
    public void execute() {
        System.out.println("Executing strategy A");
    }
}

public class ConcreteStrategyB implements Strategy {
    @Override
    public void execute() {
        System.out.println("Executing strategy B");
    }
}

public class Context {
    private Strategy strategy;

    public void setStrategy(Strategy strategy) {
        this.strategy = strategy;
    }

    public void executeStrategy() {
        strategy.execute();
    }
}
```

### Template Method
**Описание:** Определяет скелет алгоритма в методе, оставляя определение некоторых шагов подклассам.
**Пример кода:**
```java
public abstract class AbstractClass {
    public final void templateMethod() {
        primitiveOperation1();
        primitiveOperation2();
        concreteOperation();
    }

    protected abstract void primitiveOperation1();
    protected abstract void primitiveOperation2();

    private void concreteOperation() {
        System.out.println("Concrete operation");
    }
}

public class ConcreteClass extends AbstractClass {
    @Override
    protected void primitiveOperation1() {
        System.out.println("Primitive operation 1");
    }

    @Override
    protected void primitiveOperation2() {
        System.out.println("Primitive operation 2");
    }
}
```

### Visitor
**Описание:** Позволяет добавлять в программу новые операции, не изменяя классы объектов, над которыми эти операции могут выполняться.
**Пример кода:**
```java
public interface Visitor {
    void visitConcreteElementA(ConcreteElementA element);
    void visitConcreteElementB(ConcreteElementB element);
}

public class ConcreteVisitor implements Visitor {
    @Override
    public void visitConcreteElementA(ConcreteElementA element) {
        System.out.println("Visiting ConcreteElementA");
    }

    @Override
    public void visitConcreteElementB(ConcreteElementB element) {
        System.out.println("Visiting ConcreteElementB");
    }
}

public interface Element {
    void accept(Visitor visitor);
}

public class ConcreteElementA implements Element {
    @Override
    public void accept(Visitor visitor) {
        visitor.visitConcreteElementA(this);
    }
}

public class ConcreteElementB implements Element {
    @Override
    public void accept(Visitor visitor) {
        visitor.visitConcreteElementB(this);
    }
}

public class ObjectStructure {
    private List<Element> elements = new ArrayList<>();

    public void addElement(Element element) {
        elements.add(element);
    }

    public void accept(Visitor visitor) {
        for (Element element : elements) {
            element.accept(visitor);
        }
    }
}
```

## 5. Заключение
Паттерны проектирования помогают разработчикам создавать гибкие и повторно используемые решения для общих проблем проектирования. Изучение и применение этих паттернов может значительно улучшить качество и поддерживаемость кода.

Надеюсь, этот конспект будет полезен для вас! Если у вас есть еще вопросы или нужна дополнительная информация, не стесняйтесь спрашивать.