Конечно! Вот конспект по SQL с подробным объяснением `GROUP BY`, различных видов `JOIN` и уровней изоляции транзакций с примерами на Java. Включено содержание для удобной навигации.

## Содержание
1. [Основы SQL](#основы-sql)
2. [GROUP BY](#group-by)
3. [JOIN](#join)
    - [INNER JOIN](#inner-join)
    - [LEFT JOIN](#left-join)
    - [RIGHT JOIN](#right-join)
    - [FULL JOIN](#full-join)
4. [ACID гарантии](#acid-гарантии)
    - [Atomicity](#atomicity)
    - [Consistency](#consistency)
    - [Isolation](#isolation) 
    - [Durability](#durability)
5. [Уровни изоляции транзакций](#уровни-изоляции-транзакций)
    - [READ UNCOMMITTED](#read-uncommitted)
    - [READ COMMITTED](#read-committed)
    - [REPEATABLE READ](#repeatable-read)
    - [SERIALIZABLE](#serializable)

## Основы SQL
SQL (Structured Query Language) — это язык для управления и манипуляции данными в реляционных базах данных. Основные команды включают `SELECT`, `INSERT`, `UPDATE`, `DELETE`.

## GROUP BY
Команда `GROUP BY` используется для группировки строк, имеющих одинаковые значения в указанных столбцах. Она часто используется с агрегатными функциями (`COUNT`, `SUM`, `AVG`, `MAX`, `MIN`).

### Пример на Java
```java
String query = "SELECT department, COUNT(*) FROM employees GROUP BY department";
try (Connection conn = DriverManager.getConnection(url, user, password);
     Statement stmt = conn.createStatement();
     ResultSet rs = stmt.executeQuery(query)) {
    while (rs.next()) {
        System.out.println("Department: " + rs.getString(1) + ", Count: " + rs.getInt(2));
    }
} catch (SQLException e) {
    e.printStackTrace();
}
```

## JOIN
Команда `JOIN` используется для объединения строк из двух или более таблиц на основе связанного столбца между ними.

### INNER JOIN
Возвращает строки, которые имеют совпадающие значения в обеих таблицах.

### Пример на Java
```java
String query = "SELECT employees.name, departments.name FROM employees INNER JOIN departments ON employees.department_id = departments.id";
try (Connection conn = DriverManager.getConnection(url, user, password);
     Statement stmt = conn.createStatement();
     ResultSet rs = stmt.executeQuery(query)) {
    while (rs.next()) {
        System.out.println("Employee: " + rs.getString(1) + ", Department: " + rs.getString(2));
    }
} catch (SQLException e) {
    e.printStackTrace();
}
```

### LEFT JOIN
Возвращает все строки из левой таблицы и совпадающие строки из правой таблицы. Если совпадения нет, возвращает NULL для правой таблицы.

### Пример на Java
```java
String query = "SELECT employees.name, departments.name FROM employees LEFT JOIN departments ON employees.department_id = departments.id";
try (Connection conn = DriverManager.getConnection(url, user, password);
     Statement stmt = conn.createStatement();
     ResultSet rs = stmt.executeQuery(query)) {
    while (rs.next()) {
        System.out.println("Employee: " + rs.getString(1) + ", Department: " + rs.getString(2));
    }
} catch (SQLException e) {
    e.printStackTrace();
}
```

### RIGHT JOIN
Возвращает все строки из правой таблицы и совпадающие строки из левой таблицы. Если совпадения нет, возвращает NULL для левой таблицы.

### Пример на Java
```java
String query = "SELECT employees.name, departments.name FROM employees RIGHT JOIN departments ON employees.department_id = departments.id";
try (Connection conn = DriverManager.getConnection(url, user, password);
     Statement stmt = conn.createStatement();
     ResultSet rs = stmt.executeQuery(query)) {
    while (rs.next()) {
        System.out.println("Employee: " + rs.getString(1) + ", Department: " + rs.getString(2));
    }
} catch (SQLException e) {
    e.printStackTrace();
}
```

### FULL JOIN
Возвращает все строки, когда есть совпадение в одной из таблиц. Если совпадения нет, возвращает NULL для отсутствующих совпадений.

### Пример на Java
```java
String query = "SELECT employees.name, departments.name FROM employees FULL JOIN departments ON employees.department_id = departments.id";
try (Connection conn = DriverManager.getConnection(url, user, password);
     Statement stmt = conn.createStatement();
     ResultSet rs = stmt.executeQuery(query)) {
    while (rs.next()) {
        System.out.println("Employee: " + rs.getString(1) + ", Department: " + rs.getString(2));
    }
} catch (SQLException e) {
    e.printStackTrace();
}
```

## ACID гарантии

Транзакция в контексте баз данных — это последовательность операций, которые выполняются как единое целое. Транзакция должна обладать четырьмя основными свойствами, известными как ACID:

1. **Atomicity (Атомарность)**: Все операции внутри транзакции либо выполняются полностью, либо не выполняются вовсе.
2. **Consistency (Согласованность)**: Транзакция переводит базу данных из одного согласованного состояния в другое.
3. **Isolation (Изоляция)**: Операции транзакции не должны быть видны другим транзакциям до её завершения.
4. **Durability (Долговечность)**: После завершения транзакции её результаты должны быть сохранены в базе данных даже в случае сбоя системы.

ACID — это аббревиатура, обозначающая четыре ключевых свойства транзакций в базах данных: атомарность, согласованность, изолированность и долговечность. Эти свойства обеспечивают надежность и целостность данных. Давайте рассмотрим каждое из них подробнее:

### Atomicity
**Атомарность** гарантирует, что транзакция либо выполняется полностью, либо не выполняется вовсе. Если какая-либо часть транзакции не удается, все изменения, внесенные в рамках этой транзакции, откатываются.

**Пример:** Представьте, что вы переводите деньги с одного банковского счета на другой. Транзакция включает два шага: списание денег с одного счета и зачисление на другой. Если списание прошло успешно, но зачисление не удалось, атомарность гарантирует, что деньги будут возвращены на исходный счет.

Атомарность гарантирует, что транзакция будет выполнена полностью или не выполнена вовсе. Рассмотрим следующий пример:

```sql
START TRANSACTION;

UPDATE accounts SET balance = balance - 100 WHERE account_id = 1;
UPDATE accounts SET balance = balance + 100 WHERE account_id = 2;

COMMIT;
```

1. **START TRANSACTION**: Начинает новую транзакцию.
2. **UPDATE accounts SET balance = balance - 100 WHERE account_id = 1**: Уменьшает баланс на 100 для аккаунта с `account_id = 1`.
3. **UPDATE accounts SET balance = balance + 100 WHERE account_id = 2**: Увеличивает баланс на 100 для аккаунта с `account_id = 2`.
4. **COMMIT**: Подтверждает транзакцию, делая все изменения постоянными.

Если произойдет ошибка на любом этапе, транзакция будет откатана, и ни одно из изменений не будет применено. Это предотвращает частичные изменения данных.

### Consistency
**Согласованность** гарантирует, что транзакция переводит базу данных из одного согласованного состояния в другое. Это означает, что все правила и ограничения базы данных соблюдаются до и после выполнения транзакции.

**Пример:** В базе данных есть правило, что баланс счета не может быть отрицательным. Если транзакция пытается списать больше денег, чем есть на счете, она будет отклонена, чтобы сохранить согласованность данных.

Согласованность гарантирует, что данные всегда будут соответствовать определенным правилам и ограничениям. Рассмотрим следующий пример:

```sql
CREATE TABLE orders (
    order_id INT PRIMARY KEY,
    customer_id INT,
    order_date DATE,
    CONSTRAINT fk_customer FOREIGN KEY (customer_id) REFERENCES customers(customer_id)
);

INSERT INTO orders (order_id, customer_id, order_date) VALUES (1, 100, '2024-09-30');
```

1. **CREATE TABLE orders**: Создает таблицу `orders` с полями `order_id`, `customer_id` и `order_date`.
2. **PRIMARY KEY (order_id)**: Устанавливает `order_id` как первичный ключ, что гарантирует уникальность каждого заказа.
3. **FOREIGN KEY (customer_id) REFERENCES customers(customer_id)**: Устанавливает внешний ключ, который гарантирует, что `customer_id` в таблице `orders` всегда будет ссылаться на существующий `customer_id` в таблице `customers`.

### Isolation
**Изолированность** гарантирует, что параллельно выполняемые транзакции не влияют друг на друга. Существует несколько уровней изолированности:

1. **Read Uncommitted:** Транзакции могут видеть незавершенные изменения других транзакций.
2. **Read Committed:** Транзакции видят только завершенные изменения других транзакций.
3. **Repeatable Read:** Транзакции видят только те данные, которые были зафиксированы до начала транзакции.
4. **Serializable:** Транзакции выполняются так, как если бы они были последовательными, одна за другой.

**Примеры:**
- **Read Uncommitted:** Транзакция A читает данные, которые транзакция B еще не зафиксировала. Это может привести к "грязным" чтениям.
- **Read Committed:** Транзакция A не видит изменений, внесенных транзакцией B, пока B не завершится.
- **Repeatable Read:** Транзакция A видит одни и те же данные при каждом чтении, даже если транзакция B вносит изменения параллельно.
- **Serializable:** Транзакция A и транзакция B выполняются так, как если бы они были последовательными, исключая любые параллельные изменения.

Изолированность гарантирует, что параллельные транзакции не будут влиять друг на друга. Рассмотрим следующий пример:

```sql
SET TRANSACTION ISOLATION LEVEL REPEATABLE READ;

START TRANSACTION;

SELECT balance FROM accounts WHERE account_id = 1;

-- Выполнение других операций

COMMIT;
```

1. **SET TRANSACTION ISOLATION LEVEL REPEATABLE READ**: Устанавливает уровень изоляции транзакции на `REPEATABLE READ`, что предотвращает неповторяющееся чтение.
2. **START TRANSACTION**: Начинает новую транзакцию.
3. **SELECT balance FROM accounts WHERE account_id = 1**: Читает баланс аккаунта с `account_id = 1`.
4. **COMMIT**: Подтверждает транзакцию.

Уровень изоляции `REPEATABLE READ` гарантирует, что данные, прочитанные в транзакции, не изменятся до завершения транзакции.

### Durability
**Долговечность** гарантирует, что после завершения транзакции ее результаты сохраняются даже в случае сбоя системы. Это достигается за счет записи изменений на постоянный носитель.

**Пример:** После успешного перевода денег между счетами информация о транзакции записывается на диск. Даже если система выйдет из строя сразу после этого, данные о транзакции не будут потеряны.

Рассмотрим следующий пример:

```sql
START TRANSACTION;

INSERT INTO transactions (transaction_id, amount, transaction_date) VALUES (1, 100, '2024-09-30');

COMMIT;
```

1. **START TRANSACTION**: Начинает новую транзакцию.
2. **INSERT INTO transactions (transaction_id, amount, transaction_date) VALUES (1, 100, '2024-09-30')**: Вставляет новую запись в таблицу `transactions`.
3. **COMMIT**: Подтверждает транзакцию, записывая данные на диск.

После выполнения команды `COMMIT` данные будут сохранены на постоянном носителе, что гарантирует их долговечность.

### Реализация ACID гарантий
ACID гарантии реализуются за счет различных механизмов, таких как журналы транзакций, блокировки и контроль версий. Например, журналы транзакций позволяют откатывать изменения в случае сбоя, а блокировки и контроль версий обеспечивают изолированность транзакций.

### Базы данных, поддерживающие ACID
Многие реляционные базы данных поддерживают ACID гарантии, включая:
- **MySQL**
- **PostgreSQL**
- **Oracle**
- **Microsoft SQL Server**

Эти базы данных используют различные механизмы для обеспечения ACID свойств, такие как журналы транзакций, блокировки и контроль версий.


## Уровни изоляции транзакций
![image](https://github.com/user-attachments/assets/9e2ae05e-e350-4109-a12d-459d5f1a2c9d)

Уровни изоляции транзакций определяют, как транзакции взаимодействуют друг с другом. Существует четыре основных уровня изоляции:

### READ UNCOMMITTED
Позволяет транзакциям читать данные, которые еще не были зафиксированы другими транзакциями. Это может привести к "грязным" чтениям.

### Пример на Java
```java
try (Connection conn = DriverManager.getConnection(url, user, password)) {
    conn.setTransactionIsolation(Connection.TRANSACTION_READ_UNCOMMITTED);
    conn.setAutoCommit(false);
    // выполнение операций
    conn.commit();
} catch (SQLException e) {
    e.printStackTrace();
}
```

### Пример на SQL
Самый низкий уровень изоляции, позволяющий читать необработанные изменения других транзакций. Это может привести к "грязным чтениям" (dirty reads).

```sql
SET TRANSACTION ISOLATION LEVEL READ UNCOMMITTED;

START TRANSACTION;

SELECT balance FROM accounts WHERE account_id = 1;

COMMIT;
```


### READ COMMITTED
Позволяет транзакциям читать только те данные, которые были зафиксированы другими транзакциями. Это предотвращает "грязные" чтения.

### Пример на Java
```java
try (Connection conn = DriverManager.getConnection(url, user, password)) {
    conn.setTransactionIsolation(Connection.TRANSACTION_READ_COMMITTED);
    conn.setAutoCommit(false);
    // выполнение операций
    conn.commit();
} catch (SQLException e) {
    e.printStackTrace();
}
```

### Пример на SQL
Гарантирует, что транзакция читает только подтвержденные изменения. Это предотвращает грязные чтения, но не защищает от "неповторяющихся чтений" (non-repeatable reads).

```sql
SET TRANSACTION ISOLATION LEVEL READ COMMITTED;

START TRANSACTION;

SELECT balance FROM accounts WHERE account_id = 1;

COMMIT;
```

### REPEATABLE READ
Гарантирует, что если транзакция читает данные, то эти данные не изменятся до завершения транзакции. Это предотвращает "неповторяющиеся" чтения.

### Пример на Java
```java
try (Connection conn = DriverManager.getConnection(url, user, password)) {
    conn.setTransactionIsolation(Connection.TRANSACTION_REPEATABLE_READ);
    conn.setAutoCommit(false);
    // выполнение операций
    conn.commit();
} catch (SQLException e) {
    e.printStackTrace();
}
```

### Пример на SQL
Гарантирует, что если транзакция читает данные, то она увидит их в том же виде при последующих чтениях в рамках той же транзакции. Это предотвращает грязные и неповторяющиеся чтения, но не защищает от "фантомных чтений" (phantom reads).

```sql
SET TRANSACTION ISOLATION LEVEL REPEATABLE READ;

START TRANSACTION;

SELECT balance FROM accounts WHERE account_id = 1;

-- Выполнение других операций

COMMIT;
```

### SERIALIZABLE
Самый строгий уровень изоляции, который гарантирует полную изоляцию транзакций. Это предотвращает "фантомные" чтения.
Фантомное - когда читаем и ожидаем 10 элементов и тут резко приходит 11, где-то добавился элемент в коллекцию.

### Пример на Java
```java
try (Connection conn = DriverManager.getConnection(url, user, password)) {
    conn.setTransactionIsolation(Connection.TRANSACTION_SERIALIZABLE);
    conn.setAutoCommit(false);
    // выполнение операций
    conn.commit();
} catch (SQLException e) {
    e.printStackTrace();
}
```

### Пример на SQL
Самый высокий уровень изоляции, предотвращающий грязные, неповторяющиеся и фантомные чтения. Транзакции выполняются таким образом, что результат эквивалентен последовательному выполнению транзакций.

```sql
SET TRANSACTION ISOLATION LEVEL SERIALIZABLE;

START TRANSACTION;

SELECT balance FROM accounts WHERE account_id = 1;

-- Выполнение других операций

COMMIT;
```

Надеюсь, этот конспект будет полезен! Если у тебя есть еще вопросы или нужны дополнительные примеры, дай знать.

Источник: беседа с Copilot, 29.09.2024
(1) Database Isolation Levels: A Practical Guide - DEV Community. https://dev.to/rivelles/database-isolation-levels-a-practical-guide-5fdc.
(2) Isolation levels and concurrency - Oracle. https://docs.oracle.com/javadb/10.8.3.0/devguide/cdevconcepts15366.html.
(3) Transaction Propagation and Isolation in Spring @Transactional. https://www.baeldung.com/spring-transactional-propagation-isolation.
(4) Transaction Isolation Levels in DBMS - GeeksforGeeks. https://www.geeksforgeeks.org/transaction-isolation-levels-dbms/.
(5) java - MySQL Transaction isolation levels - Stack Overflow. https://stackoverflow.com/questions/12609263/mysql-transaction-isolation-levels.

Источник: беседа с Copilot, 30.09.2024
(1) Базы данных ACID: гарантия целостности данных. https://hr-portal.ru/story/bazy-dannyh-acid-garantiya-celostnosti-dannyh.
(2) Требования ACID на простом языке / Хабр - Habr. https://habr.com/ru/articles/555920/.
(3) ACID в базах данных: гарантии надежности и целостности данных. https://bing.com/search?q=ACID+%d0%b3%d0%b0%d1%80%d0%b0%d0%bd%d1%82%d0%b8%d0%b8+%d0%b2+%d0%b1%d0%b0%d0%b7%d0%b0%d1%85+%d0%b4%d0%b0%d0%bd%d0%bd%d1%8b%d1%85.
