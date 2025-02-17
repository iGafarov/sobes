### **Принцип открытости-закрытости** 

## КРАТКО:
При добавлении новых фичей мы не должны менять старый функционал, 
а по возможности добавлять новый

## ПОДРОБНО:
рассмотрим на примере только что созданного класса по отправке сообщений.

Допустим нам необходимо кроме отправки сообщения по электронной почте отправлять еще смс сообщения. И мы можем дописать метод sendMessage таким образом:

Но в данном случае мы нарушим второй принцип, потому что класс должен быть закрыт для модификации, но открыт для расширения, а мы модифицируем (изменяем) метод.
```java
public class NotificationService {
    public void sendMessage(String typeMessage, String message) {
        if (typeMessage.equals("email")) {
            //write email
            //use JavaMailSenderAPI
        }
        if (typeMessage.equals("sms")) {
            //write sms
            //send sms

        }

    }
}
```
## ПРАВИЛЬНО
Поэтому создадим интерфейс NotificationService и в нем поместим метод sendMessage.

```java
public interface NotificationService {
    public void sendMessage(String message);
}
```

```java
public class EmailNotification implements NotificationService{
    @Override
    public void sendMessage(String message) {
        //write email
        //use JavaMailSenderAPI
    }
}
```

```java
public class MobileNotification implements NotificationService{
    @Override
    public void sendMessage(String message) {
        //write sms
        //send sms
    }
}
```

Проектируя таким образом код мы не будем нарушать принцип открытости-закрытости, так как мы расширяем нашу функциональность, а не изменяем (модифицируем) наш класс.