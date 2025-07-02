# База данных "periodic_table" (PostgreSQL)

База данных "periodic_table" предназначена для хранения информации о химических элементах, их свойствах и типах. Используется для поиска и вывода информации о любом элементе по имени или атомному номеру.

## Структура базы данных

### Таблицы

- **elements** — элементы
  - `atomic_number` INTEGER PRIMARY KEY
  - `symbol` VARCHAR(2) NOT NULL UNIQUE
  - `name` VARCHAR(40) NOT NULL UNIQUE

- **properties** — свойства элементов
  - `atomic_number` INTEGER PRIMARY KEY REFERENCES elements(atomic_number)
  - `atomic_mass` NUMERIC NOT NULL
  - `melting_point_celsius` NUMERIC NOT NULL
  - `boiling_point_celsius` NUMERIC NOT NULL
  - `type_id` INTEGER NOT NULL REFERENCES types(type_id)

- **types** — типы элементов
  - `type_id` SERIAL PRIMARY KEY
  - `type` VARCHAR(20) NOT NULL

## Связи между таблицами

- Таблица `properties` ссылается на `elements` по `atomic_number` и на `types` по `type_id`.
- Таблица `elements` содержит уникальные символы и имена элементов.

## Пример инициализации базы данных

```sql
CREATE DATABASE periodic_table;
\c periodic_table

CREATE TABLE elements (
  atomic_number INTEGER PRIMARY KEY,
  symbol VARCHAR(2) NOT NULL UNIQUE,
  name VARCHAR(40) NOT NULL UNIQUE
);

CREATE TABLE types (
  type_id SERIAL PRIMARY KEY,
  type VARCHAR(20) NOT NULL
);

CREATE TABLE properties (
  atomic_number INTEGER PRIMARY KEY REFERENCES elements(atomic_number),
  atomic_mass NUMERIC NOT NULL,
  melting_point_celsius NUMERIC NOT NULL,
  boiling_point_celsius NUMERIC NOT NULL,
  type_id INTEGER NOT NULL REFERENCES types(type_id)
);
```

## Пример использования bash-скрипта

Скрипт позволяет искать элемент по имени или атомному номеру и выводит подробную информацию:

- Если передан аргумент — ищет по имени (начало) или по номеру.
- Если элемент найден, выводит: атомный номер, символ, имя, тип, массу, температуру плавления и кипения.
- Если не найден — сообщает об этом.

Пример запуска:
```bash
bash element.sh Hydrogen
bash element.sh 1
```

## Примечания

- Все внешние ключи и уникальные ограничения уже настроены.
- Данные можно импортировать с помощью скрипта или вручную через psql.
- Для расширения можно добавить больше элементов и типов. 