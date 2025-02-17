### Принцип единственной ответственности

## КРАТКО:
У классов должна быть только одна зона ответственности

## ПОДРОБНО:
```java
public class RentCarService {

    public Car findCar(String carNo) {
        //find car by number
        return car;
    }

    public Order orderCar(String carNo, Client client) {
        //client order car
        return order;
    }

    public void printOrder(Order order) {
        //print order
    }
    public void getCarInterestInfo(String carType) {
        if (carType.equals("sedan")) {
            //do some job
        }
        if (carType.equals("pickup")) {
            //do some job
        }
        if (carType.equals("van")) {
            //do some job
        }
    }
    public void sendMessage(String typeMessage, String message) {
        if (typeMessage.equals("email")) {
            //write email
            //use JavaMailSenderAPI
        }
    }
}
```
## ПРАВИЛЬНО
Разделяем:

```java
public class PrinterService {
    public void printOrder(Order order) {
        //print order
    }
}
```

```java
public class CarInfoService {
    public void getCarInterestInfo(String carType) {
        if (carType.equals("sedan")) {
            //do some job
        }
        if (carType.equals("pickup")) {
            //do some job
        }
        if (carType.equals("van")) {
            //do some job
        }
    }
}
```

```java
public class NotificationService {
    public void sendMessage(String typeMessage, String message) {
        if (typeMessage.equals("email")) {
            //write email
            //use JavaMailSenderAPI
        }
    }
}
```

```java
public class CarService {
    public Car findCar(String carNo) {
        //find car by number
        return car;
    }
}
```

В итоге в RentCarService остается только один метод и одна зона ответственности:

```java
public class RentCarService {
    public Order orderCar(String carNo, Client client) {
        //client order car
        return order;
    }
  }
```