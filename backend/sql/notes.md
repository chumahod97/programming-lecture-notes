# SQL Notes

## Important notes and links

---

```mysql
create table users(
  `id` int not null AUTO_INCREMENT, 
  first_name varchar(255), 
  last_name varchar(255), 
  email varchar(255), 
  password varchar(255), 
  role varchar(255), 
  dob datetime, 
  gender char(1), 
  PRIMARY KEY(`id`)
);
```

---

## 2.2 Реляционные и нереляционные базы данных

Имена столбцов пишутся в **snake_case**.

В SQL строки называются **записями**, а столбцы **полями**.

Реляционные базы данных также принято называть **базами данных SQL**.

**Популярные SQL DB**:

* MySQL
* PostgreSQL
* MicrosoftSQL - Используется среди разработчиков на стеке Microsoft
* SQLite - В мобильных и Desktop приложениях
* Oracle - В крупных корпоративных системах

Нереляционные базы данных позволяют упростить и ускорить работу
с данными, которые плохо структурированы.

**Форматы хранения данных в NoSQL-базах:**

* Пары "ключ - значение" (**Redis**, **Memcached**, **Amazon DynamoDB**)
* JSON-документы (**mongoDB**, **CouchDB**, **Elasticsearch**)
* Семейства столбцов
* Графы и т.д.

## 2.3 Типы полей в реляционных базах данных

**Типы полей (данных):**

* Числовые данные
  * `TINYINT` (от -128 до 127)
  * `INT` (от -2 млрд до 2 млрд)
  * `BIGINT` (-2^63 - 2^63)
  * `FLOAT`
  * `DOUBLE`
* Данные о дате и времени
  * `DATETIME`
  * `DATE`
  * `TIMESTAMP` (количество секунд с 1970 года)
* Строковые данные
  * `CHAR(N)` (Максимальная длина 255)
  * `VARCHAR`
  * `TINYTEXT` (Максимальная длина 255)
  * `TEXT` (до 65 535)
  * `MEDIUMTEXT` (до 16 Mb)
  * `LONGTEXT` (до 4 Gb)
  * `ENUM`
* Пространственные данные - В полях этого типа можно хранить \
  геометрические фигуры, координаты и всё что связанно с пространством.
* JSON-данные
* Специальное значение NULL

## 2.4 Типы связей в реляционных базах данных

Существует 3 типа связей в SQL:

* **Один ко многим** \
  Пример: 1 **автор** и несколько **книг** \
  Данные могут ссылаться на свою же таблицу
* **Один к одному** \
  реализуется с помощью метки `UNIQUE`
* **Многие ко многим** \
  Пример: несколько **книг** и несколько **заказов**

## 3.2 SELECT-запросы

`SELECT * FROM tableName;` \
Показать все поля из таблицы

`SELECT 'field1', 'field2' FROM tableName`

`SELECT 'field1' AS 'f1' from tableName` \
Переименовывание поля таблицы

`SELECT (field1 + field2) AS 'total' FROM tableName;` \
Изменение данных

## 3.3 Фильтрация данных, оператор WHERE

Фильтрация записей

`SELECT * FROM 'good' WHERE 'count' = 0;` \
Равенство

`SELECT * FROM 'good' WHERE 'count' < 50;` \
Больше или меньше

`SELECT * FROM 'order_status' WHERE 'code' != 'NEW';` \
Неравенство

```mysql
SELECT * FROM 'user'
WHERE
'reg_date' >= '2021-09-08 00:00:00'
AND
 'reg_date' <= '2021-09-08 23:59:59'
```

Диапазон дат

```mysql
SELECT * FROM `user`
WHERE `reg_date` BETWEEN
 '2021-09-08 00:00:00' AND
 '2021-09-08 23:59:59'
```

То же самое, но с помощью BETWEEN

## 3.5 Оператор WHERE и сложные условия

Маски:

* '%' - 0 или более символов
* '-' - один символ

```mysql
SELECT * FROM `good`
WHERE `name` LIKE '%substring%'
```

Получение данных с подстрокой в имени

`IS NULL` и `IS NOT NULL` \
Сравнение с NULL

`SELECT * FROM 'order' WHERE 'status_id' IN (7, 8);` \
Запрос на несколько значений

Для объединения нескольких условий можно использовать **круглые скобки ()**

## 3.7 Сортировка и ограничение количества результатов

`SELECT * FROM 'good' ORDER BY 'name'` \
Сортировка по имени по возрастанию

`SELECT * FROM 'good' ORDER BY 'name' DESC` \
Сортировка по имени по убыванию

Сортировка возможна по нескольким полям.

`SELECT * FROM 'good' LIMIT 10, 20;` \
Ограничение запроса \
Выдача результатов с 10 по 30

```mysql
SELECT
`good_category` . `name`
categoryName,
`good` . `name`
goodName
FROM `good`
JOIN `good_category` ON
`good_category` . `id` =
`good` . `category_id`
```

Соединение таблиц

```mysql
SELECT
c.`name` categoryName,
g.`name` goodName
FROM `good` g
JOIN `good_category` c ON
 c.`id` = g.`category_id`
```

Таблицам можно задавать псевдонимы

Типы объединений:

* `INNER JOIN` или `JOIN` - выводятся пересекающиеся записи
* `LEFT JOIN` - выводятся все записи из первой таблицы
* `RIGHT JOIN` - выводятся все записи из второй таблицы

`SELECT DISTINCT 'field1', 'field2' FROM 'table_name';` \
Выбрать уникальные записи

```mysql
SELECT 
  `category_id`,
  COUNT(*)
FROM `good`
GROUP BY `category_id`;
```

Группировка полей и также вывод числа строк в определенной категории

## 3.13 Объединение результатов, оператор UNION

Union позволяет объединять несколько таблиц в одну

Чтобы объединять таблицы с помощью Union,
нужно чтобы таблицы возвращали одни и те же поля.

## 3.15 Запросы INSERT, UPDATE и DELETE

```mysql
INSERT INTO `good` (
 `id`,
 `category_id`,
 `name`,
 `count`,
 `price`
)
VALUES (
 2088,
 '6',
 'Белый чай с вишней',
 '50',
 '344'
),
(
 2089,
 '6',
 'Чёрный чай с вишней',
 '50',
 '344'
);
```

Создание новой строки с помощью INSERT \
Те поля что были не указаны будут иметь значения по умолчанию

AUTO_INCREMENT - Специальный флаг для автоматического создания \
инкрементных чисел

```mysql
UPDATE `order` SET
 `user_id` = NULL,
 `status_id` = 1
WHERE
 `user_id` > 10 AND
 `user_id` < 50 AND
 `creation_date` > '2019-01-01';
```

Изменение существующих строк

```mysql
DELETE FROM table_name
WHERE some_condition;
```

Удаление строки из таблицы

## 4.2 Понятие выражения и функции в SQL-запросах

```mysql
SELECT
`name`,
`price` * `count` AS cost
FROM `good`;
```

Математическая операция на данными

```mysql
SELECT
`name`,
IF (
  `count` > 100,
  'enough',
  'not_enough'
) AS isEnough
FROM `good`;
```

Условный оператор

```mysql
SELECT
  `category_id`,
  COUNT(DISTINCT `name`)
FROM `good`
GROUP BY `category_id`;
```

Функция подсчета по уникальным именам

**Функции:**

* Функции работы со строками
* Функции работы с датами и временем
* Агрегатные функции

## 4.3 Условные операторы и булевы выражения

```mysql
IF(condition, expressionOnTrue, expressionOnFalse);
```

Оператор IF

```mysql
SELECT
  `id`,
  `name`,
CASE
WHEN `count` < 20 THEN 'Мало'
WHEN `count` > 500 THEN 'Много'
ELSE 'Нормально'
END AS `count`
FROM `good` WHERE 1
```

Оператор CASE аналог switch

## 4.5 Функции работы со строками

`CHAR_LENGTH('stringField')`
Функция для получения длины строки

`SUBSTRING(str, start, length)` \
или `SUBSTR(str, start, length)`
Получение подстроки

В MySQL отсчет начинается с 1, а не с нуля

`CONCAT(exp1, exp2, ...)` \
Конкатенация строк

`GROUP_CONCAT(expression SEPARATOR separator_string)` \
Конкатенация полей при группировке

`TRIM(expression)` \
Обрезка строки

`REPLACE(field, from, to)` \
Замена подстроки

## 4.7 Функции работы с датами

`DATE_FORMAT(field, '%d.%m.%Y')` \
Функция форматирования даты

**Символы формата**

* `%d` - день месяца
* `%m` - месяц
* `%Y` - год (4 цифры)
* `%H` - часы
* `%i` - минуты
* `%s` - секунды
* `%w` - день недели
* `%j` - день года

`DAYOFWEEK(date)` \
`DAYOFYEAR(date)` \
`NOW()` - текущая дата и время \
`CURDATE()` - текущая дата \
`DATEDIFF(date2, date1)` - разница между датами в днях

**Функции с timestamps:** \
`UNIX_TIMESTAMP(date)` - преобразование даты в timestamp \
`FROM_UNIXTIME(timestamp)` - обратное преобразование \

## 4.9 Агрегатные функции

`SUM(field)` - возвращает сумму переданных полей \
`MIN(field)`, `MAX(field)` \
`AVG(field)` - среднее значение

## 4.10 Фильтрация после группировки, оператор HAVING

`HAVING condition` - условие для агрегирующих функций \
`HAVING` - должен идти сразу после `GROUP BY`

## 5.2 Скорость выполнения запросов, индексы

**От чего зависит скорость?**

* Размер таблицы
* Скорость поиска

**Способы поиска в таблице:**

* Прямой перебор
* Умный поиск

Полученные данные кэшируются для ускорения повторного запроса.

**Типы индексов в MySQL:**

* `BTREE` - для поиска по диапазонам
* `HASH` - для поиска по совпадениям

```mysql
CREATE TABLE `good_type`(
`id` INT NOT NULL AUTO_INCREMENT,
`name` VARCHAR(255),
PRIMARY KEY(`id`)
);
```

```mysql
ALTER TABLE `good`
ADD PRIMARY KEY (`id`);
```

Установка индекса при создании или обновлении таблицы

`UNIQUE` - ограничение на уникальность ключа

**Отличия `UNIQUE` от `PRIMARY KEY`:**

* `PRIMARY KEY` используется для идентификации записей
* `PRIMARY KEY` не может иметь значение NULL
* `PRIMARY KEY` может быть только один в таблице

**Индексы можно устанавливать:**

* По одному полю
* По нескольким полям
* С указанием длины (для строк)

## 5.3 Связи, внешние ключи и ограничения

Решением проблемы целостности данных является установка связей с ограничениями.

```mysql
ALTER TABLE `good`
  ADD FOREIGN KEY (`category_id`)
    REFERENCES `good_category` (`id`);
```

Для связи "Один ко многим"

```mysql
ALTER TABLE `countries` ADD UNIQUE (`capital_id`);

ALTER TABLE `countries`
ADD FOREIGN KEY (`capital_id`)
REFERENCES `capitals`(`id`);
```

Для связи "Один к одному"

```mysql
CREATE TABLE `book2order`(
 `id` INT NOT NULL AUTO_INCREMENT,
 `book_id` INT NOT NULL,
 `order_id` INT NOT NULL,
 PRIMARY KEY (`id`),
 FOREIGN KEY (`book_id`)
 REFERENCES `books`(`id`),
 FOREIGN KEY (`order_id`)
 REFERENCES `orders`(`id`),
 UNIQUE(`book_id`, `order_id`)
);
```

Создание таблицы со связью "Многие ко многим"

```mysql
ALTER TABLE `book2order`
  ADD CONSTRAINT `book2order_fk_1`
    FOREIGN KEY (`order_id`)
    REFERENCES `order` (`id`),
  ADD CONSTRAINT `book2order_fk_2`
    FOREIGN KEY (`book_id`)
    REFERENCES `book` (`id`);
```

```mysql
ALTER TABLE `book2order`
  DROP INDEX `book2order_fk_2`;
```

Именование ключей и ограничений

**Действия при нарушении целостности:**

`ON DELETE` и `ON UPDATE`

* `RESTRICT`, `NO ACTION` (по умолчанию) - ограничение действия, если оно
  нарушает целостность данных
* `CASCADE` - каскадное изменение или удаление
* `SET NULL`

## 5.4 Вложенные запросы

```mysql
SELECT
  SUM(IF(totalPrice > 10000, totalPrice, 0))
  highPriceTotal,
  SUM(IF(totalPrice <= 10000, totalPrice, 0))
  lowPriceTotal
FROM (
  SELECT
    `id`,
    (`count` * `price`) totalPrice
    FROM `good`
    GROUP BY `id`
) t
```

```mysql
SELECT
  `id`,
  (`count` * `price`) totalPrice
FROM `good`
WHERE `category_id` IN(
  SELECT `category_id`
    FROM `good`
    GROUP BY `category_id`
    HAVING COUNT(*) > 50
)
GROUP BY `id`;
```

Вложенный запрос

**Подзапросы являются ресурсоёмкими -**
поэтому рекомендуется получать данные внутренней таблицы в программе

## 5.6 Структурные запросы

`SHOW DATABASES;` - список баз данных
`CREATE DATABASE 'basename'` - создать базу данных
`DROP DATABASE 'basename'` - удалить базу данных

```mysql
CREATE TABLE `good_type`(
  `id` INT NOT NULL AUTO_INCREMENT,
  `sort_index` INT,
  `name` VARCHAR(255),
  PRIMARY KEY(`id`)
);
```

Создание таблицы

```mysql
ALTER TABLE `good_type`
  DROP COLUMN `name`,
  ADD `code` TEXT NOT NULL
    AFTER `id`;
```

Изменение таблицы

`TRUNCATE 'tableName;` - очистить таблицу

## 5.7 Представления

Представления - это виртуальные таблицы они удобны если нужно часто
обращаться к какой то выборке

```mysql
CREATE VIEW `ending_goods` AS
  SELECT * FROM `good`
  WHERE `count` < 10 AND `price` > 200;
```

Создание представления

```mysql
CREATE OR REPLACE VIEW
  `ending_goods` AS
  SELECT * FROM `good`
  WHERE `count` < 10 AND `price` > 200;
```

Замена представления

`DROP VIEW 'viewName'` - удаление представления
