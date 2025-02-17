### Принцип разделения интерфейсов.

## КРАТКО:
Когда не нужно "Заставлять" классы реализовывать методы интерфейсов, если они им не нужны

## ПОДРОБНО:

```java
public interface Payments {
    void payWebMoney();
    void payCreditCard();
    void payPhoneNumber();
}
```

```java
public class InternetPaymentService implements Payments{
    @Override
    public void payWebMoney() {
        //logic
    }
    @Override
    public void payCreditCard() {
        //logic
    }
    @Override
    public void payPhoneNumber() {
        //logic
    }
}
```

```java
public class TerminalPaymentService implements Payments{
    @Override
    public void payWebMoney() {
        //logic
    }
    @Override
    public void payCreditCard() {
        //logic
    }
    @Override
    public void payPhoneNumber() {
        //???????
    }
}
```
### Нарушили принцип, т.к. Terminal не нужен метод payPhoneNumber, но ему приходится его реализовывать 

## ПРАВИЛЬНО

Для того чтобы этого не происходило необходимо разделить наш исходный интерфейс Payments на несколько и, создавая классы, имплементить в них только те интерфейсы с методами, которые им нужны.
```java
public interface WebMoneyPayment {
    void payWebMoney();
}
```

```java
public interface CreditCardPayment {
    void payCreditCard();
}
```

```java
public interface PhoneNumberPayment {
    void payPhoneNumber();
}
```

```java
public class InternetPaymentService implements WebMoneyPayment,
                                                CreditCardPayment, 
                                                  PhoneNumberPayment{
    @Override
    public void payWebMoney() {
        //logic
    }
    @Override
    public void payCreditCard() {
        //logic
    }
    @Override
    public void payPhoneNumber() {
        //logic
    }
}
```

```java
public class TerminalPaymentService implements WebMoneyPayment, CreditCardPayment{
    @Override
    public void payWebMoney() {
        //logic
    }
    @Override
    public void payCreditCard() {
        //logic
    }
}
```