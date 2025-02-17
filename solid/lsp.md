### Принцип подстановки Барбары Лисков

## КРАТКО:
Наследники должны реализовывать все методы родительских классов

Принцип подстановки Барбары Лисков заключается в правильном использовании отношения наследования. Мы должны создавать наследников какого-либо базового класса тогда и только тогда, когда они собираются правильно реализовать его логику, не вызывая проблем при замене родителей на наследников.

## ПОДРОБНО:
Данный принцип непосредственно связан с наследованием классов. Допустим у нас есть базовый класс Счет (Account), в котором есть три метода: просмотр остатка на счете, пополнение счета и оплата.

```java
public class Account {
    public BigDecimal balance(String numberAccount){
        //logic
        return bigDecimal;
    };
    public void refill(String numberAccount, BigDecimal sum){
        //logic
    }
    public void payment(String numberAccount, BigDecimal sum){
        //logic
    }

}
```

```java
public class SalaryAccount extends Account{
    @Override
    public BigDecimal balance(String numberAccount){
        //logic
        return bigDecimal;
    };
    @Override
    public void refill(String numberAccount, BigDecimal sum){
        //logic
    }
    @Override
    public void payment(String numberAccount, BigDecimal sum){
        //logic
    }
}
```

```java
public class DepositAccount extends Account{
    @Override
    public BigDecimal balance(String numberAccount){
        //logic
        return bigDecimal;
    };
    @Override
    public void refill(String numberAccount, BigDecimal sum){
        //logic
    }
    @Override
    public void payment(String numberAccount, BigDecimal sum){
        throw new UnsupportedOperationException("Operation not supported");
    }
}
```
#### payment() метод в наследнике DepositAccount не реализуется - неправильно

### ПРАВИЛЬНО

```java
public class Account {
    public BigDecimal balance(String numberAccount){
        //logic
        return bigDecimal;
    };
    public void refill(String numberAccount, BigDecimal sum){
        //logic
    }
}
```

```java
public class DepositAccount extends Account{
    @Override
    public BigDecimal balance(String numberAccount){
        //logic
        return bigDecimal;
    };
    @Override
    public void refill(String numberAccount, BigDecimal sum){
        //logic
    }
}
```

payment() теперь личный метод SalaryAccount класса

Либо можем создать еще один класс PaymentAccount с payment методом =>
PaymentAccount extends Account
SalaryAccount extends PaymentAccount

```java
public class SalaryAccount extends Account{
    @Override
    public BigDecimal balance(String numberAccount){
        //logic
        return bigDecimal;
    };
    @Override
    public void refill(String numberAccount, BigDecimal sum){
        //logic
    }
    
    public void payment(String numberAccount, BigDecimal sum){
        //logic
    }
}
```