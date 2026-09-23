# Movie Database — Hibernate Mapping (Module 4, Project 2)

Учебный проект для 4-го модуля курса JavaRush («Работа с БД»). 
Маппинг JPA-сущностей на существующую схему `movie` из тестовой БД MySQL Sakila, плюс транзакционные операции: создание покупателя, возврат аренды, новая аренда с оплатой, добавление нового фильма.

## Что сделано

- 15 JPA-сущностей с маппингом на таблицы схемы `movie`: `Actor`, `Address`, `Category`, `City`, `Country`, `Customer`, `Film`, `FilmText`, `Inventory`, `Language`, `Payment`, `Rental`, `Staff`, `Store` и связи между ними (`@OneToOne`, `@OneToMany`, `@ManyToOne`, `@ManyToMany` через промежуточные таблицы).
- `GenericDAO<T>` — базовый класс с CRUD-операциями через Hibernate `Session`.
- 15 DAO-классов, по одному на каждую сущность.
- 2 `AttributeConverter`:
  - `RatingConverter` — enum `Rating` ↔ строка в БД (`G`, `PG`, `PG-13`, `R`, `NC-17`).
  - `YearAttributeConverter` — `java.time.Year` ↔ `SMALLINT`.
- 2 enum'а: `Rating`, `Feature`.
- 4 транзакционных метода в `Main`:
  1. **Создание покупателя** — с адресом, привязкой к магазину.
  2. **Возврат арендованного фильма** — заполнение `return_date` в `rental`.
  3. **Новая аренда** — поиск доступного фильма, создание `inventory`, `rental`, `payment`.
  4. **Новый фильм** — создание `film` + `film_text` с актёрами, категориями, языком.
- P6Spy — логирование всех SQL-запросов с реальными параметрами.
- `HBM2DDL_AUTO = "validate"` — Hibernate проверяет, что маппинг совпадает со схемой, но не изменяет её.

## Стек

- Java 24
- Hibernate 5.6 (hibernate-core-jakarta)
- MySQL 8 (mysql-connector-java)
- P6Spy 3.9
- Maven 3

## Требования

- JDK 24+
- Maven 3.6+
- MySQL 8 с развёрнутым дампом схемы `movie`

## Как запустить

1. Разверните дамп БД `movie` на локальном MySQL.
2. В классе `Main` в методе `getProperties()` укажите свои данные:
   ```java
   properties.put(Environment.USER, "ваш_user");
   properties.put(Environment.PASS, "ваш_пароль");
3. Соберите и запустите: mvn clean compile exec:java -Dexec.mainClass="Main".
