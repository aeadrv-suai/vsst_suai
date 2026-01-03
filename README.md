# dbt Course Practice Project

> **Исследовательская работа по инструменту dbt (data build tool) для автоматизированной трансформации и валидации данных в системах обработки больших объёмов информации**

[![GitHub](https://img.shields.io/badge/GitHub-Mrx112426/dbt_course_practice-181717?style=flat-square&logo=github)](https://github.com/Mrx112426/dbt_course_practice)
[![dbt Version](https://img.shields.io/badge/dbt-v1.10.17-FF6B35?style=flat-square&logo=dbt)](https://docs.getdbt.com/)
[![PostgreSQL](https://img.shields.io/badge/PostgreSQL-13+-336791?style=flat-square&logo=postgresql)](https://www.postgresql.org/)
[![Python](https://img.shields.io/badge/Python-3.11+-3776AB?style=flat-square&logo=python)](https://www.python.org/)
[![License](https://img.shields.io/badge/License-MIT-green?style=flat-square)](LICENSE)

## 📋 Описание проекта

Полнофункциональный ETL/ELT pipeline, разработанный с использованием инструмента **dbt (data build tool)** на примере базы данных авиарейсов. Проект демонстрирует:

- 🏗️ **Архитектура Medallion** (Bronze → Silver → Gold слои)
- 📊 **12 моделей данных** (7 staging + 5 mart/fact моделей)
- ✅ **42 встроенных теста** (26 generic + 16 unit-тестов)
- 🔗 **Граф зависимостей (DAG)** с автоматическим управлением
- 📦 **Переиспользуемые макросы** на Jinja2
- 📚 **Автоматическая документация** и lineage
- 🔄 **Инкрементальные обновления** для оптимизации
- 🌳 **Git версионирование** (~20 коммитов)

## 🎯 Цель исследования

Провести комплексное исследование архитектурных и методологических особенностей инструмента dbt, а также оценить его эффективность при разработке современных конвейеров трансформации данных в контексте вычислительных систем.

## 📊 Ключевые метрики

| Метрика | Значение |
|---------|----------|
| **Моделей** | 12 (7 staging + 5 mart) |
| **Тестов** | 42 (26 generic + 16 unit) |
| **Строк обработано** | 2.4+ млн |
| **Время выполнения** | 28 сек (полное) / 3 сек (инкрементальное) |
| **Git коммитов** | ~20 |
| **Макросов** | 2+ |
| **Соответствие тестам** | 100% ✅ |

## 🏗️ Архитектура проекта

```
sources (raw data)
    ↓
staging models (stg_flights__*)
    ↓
mart/fact models (fct_*, dim_*, dm_*)
    ↓
tests (generic + unit)
    ↓
analytics views & documentation
```

### Medallion Architecture

```
┌─────────────────────────────────────┐
│        BRONZE LAYER (Raw Data)      │
│  - Minimal transformations          │
│  - As-is structure from source      │
│  - 2.4M rows processed              │
└─────────────────────────────────────┘
           ↓
┌─────────────────────────────────────┐
│    SILVER LAYER (Cleaned Data)      │
│  - Data cleansing                   │
│  - Standardization (snake_case)     │
│  - 7 staging models                 │
│  - 26 generic tests                 │
└─────────────────────────────────────┘
           ↓
┌─────────────────────────────────────┐
│    GOLD LAYER (Analytics Views)     │
│  - Business logic applied           │
│  - 5 fact & dimension tables        │
│  - 16 unit tests                    │
│  - Ready for BI tools               │
└─────────────────────────────────────┘
```

## 📁 Структура проекта

```
dbt_course_practice/
├── models/                          # dbt models (12 total)
│   ├── staging/                     # Silver layer (7 models)
│   │   ├── stg_flights__flights.sql
│   │   ├── stg_flights__bookings.sql
│   │   ├── stg_flights__tickets.sql
│   │   ├── stg_flights__boarding_passes.sql
│   │   ├── stg_flights__ticket_flights.sql
│   │   ├── stg_flights__aircrafts.sql
│   │   ├── stg_flights__airports.sql
│   │   └── stg_flights__seats.sql
│   ├── fct_flights/                 # Gold layer (fact tables)
│   │   ├── fct_flights.sql
│   │   ├── fct_tickets.sql
│   │   ├── fct_bookings.sql
│   │   └── fct_ticket_flights.sql
│   └── dim_marts/                   # Gold layer (dimensions/marts)
│       ├── dim_flights__airports.sql
│       └── dm_seats_occupied.sql
├── tests/                           # Test definitions
│   ├── generic/                     # Generic tests (26)
│   ├── unit/                        # Unit tests (16)
│   └── singular/                    # Singular tests
├── seeds/                           # Static reference data
│   ├── city_region.csv
│   └── dictionaries__seeds.yml
├── snapshots/                       # SCD Type 2 tracking
│   ├── snap_city_region.sql
│   └── (for historical changes)
├── macros/                          # Jinja2 macros
│   ├── concat_columns.sql
│   ├── seat_no_pattern.sql
│   └── (reusable SQL logic)
├── analyses/                        # Ad-hoc analytics queries
│   └── (exploratory SQL)
├── dbt_project.yml                  # Project configuration
├── packages.yml                     # dbt packages (dbt-utils)
├── package-lock.yml                 # Package versions lock
├── .dbtignore                       # Files to ignore
├── .gitignore                       # Git ignore rules
└── README.md                        # This file
```

## 🚀 Быстрый старт

### Требования

- **Python** 3.9+ 
- **dbt** 1.10.17+
- **PostgreSQL** 13+ (или Docker)
- **Git**

### Установка

#### 1. Клонирование репозитория

```bash
git clone https://github.com/Mrx112426/dbt_course_practice.git
cd dbt_course_practice
```

#### 2. Создание виртуального окружения (опционально)

```bash
python -m venv venv
source venv/bin/activate  # macOS/Linux
# или
venv\Scripts\activate     # Windows
```

#### 3. Установка зависимостей

```bash
pip install dbt-postgres==1.10.17
# или для других СУБД:
# pip install dbt-snowflake
# pip install dbt-bigquery
```

#### 4. Установка dbt пакетов

```bash
dbt deps
```

#### 5. Настройка подключения к БД

Создайте файл `~/.dbt/profiles.yml`:

```yaml
dbt_course_practice:
  target: dev
  outputs:
    dev:
      type: postgres
      host: localhost
      user: postgres
      password: postgres
      port: 5432
      dbname: dbt_course_practice
      schema: analytics
      threads: 4
      keepalives_idle: 0
```

#### 6. Запуск проекта

```bash
# Проверка подключения
dbt debug

# Выполнение всех моделей
dbt run

# Выполнение тестов
dbt test

# Генерация документации
dbt docs generate
dbt docs serve  # Откроется на http://localhost:8000
```

## 🐳 Запуск с Docker (PostgreSQL)

```bash
# Запуск PostgreSQL контейнера
docker run --name postgres_dbt \
  -e POSTGRES_PASSWORD=postgres \
  -e POSTGRES_DB=dbt_course_practice \
  -p 5432:5432 \
  -d postgres:13

# Загрузка demo данных (если требуется)
# docker exec postgres_dbt psql -U postgres -d dbt_course_practice < demo_data.sql

# Затем следуйте шагам выше (дб должна быть доступна на localhost:5432)
```

## 📚 Основные компоненты

### 1. Models (12 моделей)

#### Staging Models (7 - Silver layer)
- `stg_flights__flights` – информация о рейсах
- `stg_flights__bookings` – бронирования
- `stg_flights__tickets` – билеты
- `stg_flights__boarding_passes` – посадочные талоны
- `stg_flights__ticket_flights` – связь билетов и рейсов
- `stg_flights__aircrafts` – информация о самолётах
- `stg_flights__airports` – информация об аэропортах
- `stg_flights__seats` – места в самолётах

#### Fact & Dimension Models (5 - Gold layer)
- `fct_flights` – факт-таблица рейсов с задержками
- `fct_tickets` – факт-таблица проданных билетов
- `fct_bookings` – факт-таблица бронирований
- `fct_ticket_flights` – связь между билетами и рейсами
- `dim_flights__airports` – измерение аэропортов
- `dm_seats_occupied` – витрина занятых мест

### 2. Tests (42 теста)

#### Generic Tests (26)
```yaml
# Примеры встроенных тестов
tests:
  - not_null:        # проверка на NULL
  - unique:          # проверка уникальности
  - relationships:   # проверка внешних ключей
  - accepted_values: # проверка допустимых значений
```

**Покрытие тестами:**
- ✅ Все ключевые поля проверены на NULL
- ✅ ID поля проверены на уникальность
- ✅ Все foreign keys валидны
- ✅ Статусы и коды в допустимых значениях

#### Unit Tests (16)
```bash
# Примеры unit-тестов
- Проверка логики вычисления задержек
- Проверка форматирования дат
- Проверка конкатенации строк
- Проверка статусов бронирования
```

### 3. Macros (2+)

```sql
-- Макрос для конкатенации столбцов
{{ concat_columns(['first_name', 'last_name']) }}

-- Макрос для валидации формата номера места
{{ seat_no_pattern(seat_column) }}
```

### 4. Seeds (Статические данные)

```csv
# city_region.csv
city,region
Moscow,Western Russia
St. Petersburg,Northwestern Russia
...
```

### 5. Snapshots (История изменений)

```sql
-- Отслеживание истории изменений города/региона (SCD Type 2)
-- snap_city_region.sql
```

## 📊 Граф зависимостей (DAG)

Проект использует направленный ациклический граф для управления зависимостями:

```
sources (flights, bookings, tickets, etc.)
    ↓
stg_flights__flights ──┐
stg_flights__bookings ┼─→ fct_flights ──→ [Tests] ──→ Analytics
stg_flights__tickets  │
stg_flights__seats ───┘
    ↓
dim_flights__airports
    ↓
dm_seats_occupied
    ↓
[16 Unit Tests]
    ↓
Documentation + Lineage
```

Просмотреть граф:
```bash
dbt docs generate && dbt docs serve
# Откроется в браузере на http://localhost:8000
# Вкладка "Lineage" показывает полный DAG
```

## 🔄 Рабочий процесс разработки

### Создание новой модели

```bash
# 1. Создайте файл модели
echo "SELECT * FROM {{ source('flights', 'flights') }}" > models/staging/stg_my_model.sql

# 2. Обновите yml конфигурацию
# models/stg_flights__models.yml

# 3. Выполните модель
dbt run --select stg_my_model

# 4. Добавьте тесты
dbt test --select stg_my_model

# 5. Обновите документацию
dbt docs generate
```

### Инкрементальные обновления

```sql
{{ config(
    materialized = 'incremental',
    unique_key = 'flight_id',
    on_schema_change = 'fail'
) }}

SELECT *
FROM {{ source('flights', 'flights') }}

{% if execute %}
    WHERE departure_time > (SELECT MAX(departure_time) FROM {{ this }})
{% endif %}
```

### CI/CD интеграция

```yaml
# .github/workflows/dbt-test.yml (пример для GitHub Actions)
on: [push, pull_request]
jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - name: dbt run & test
        run: |
          pip install dbt-postgres==1.10.17
          dbt deps
          dbt run
          dbt test
```

## 📈 Производительность

| Операция | Время | Примечание |
|----------|-------|-----------|
| `dbt run` (full) | 28 сек | Все 12 моделей |
| `dbt test` | 12 сек | 42 теста |
| Инкрементальный запуск | 3 сек | Только новые данные |
| `dbt docs generate` | 5 сек | Генерация документации |

**Оптимизация:**
- Используется параллельное выполнение (threads: 4)
- Инкрементальные модели для больших таблиц
- Эффективное использование SQL индексов

## 🔐 Безопасность и Best Practices

✅ **Git игнорирование чувствительных данных**
```
.gitignore содержит:
- dbt_packages/
- logs/
- target/
- profiles.yml
- *.dbtignore
```

✅ **Управление доступом**
```sql
-- Используйте отдельные пользователи БД
CREATE USER dbt_user WITH PASSWORD 'secure_password';
GRANT USAGE ON SCHEMA analytics TO dbt_user;
```

✅ **Документирование**
```yaml
# Все модели и поля задокументированы в YAML
description: "Факт-таблица рейсов с информацией о задержках"
columns:
  - name: flight_id
    tests:
      - not_null
      - unique
    description: "Уникальный идентификатор рейса"
```

## 📖 Используемые источники данных

**Demo база авиарейсов (Postgres Professional)**

Таблицы:
- `flights` – информация о рейсах
- `bookings` – бронирования билетов
- `tickets` – проданные билеты
- `boarding_passes` – посадочные талоны
- `aircrafts` – информация о самолётах
- `airports` – информация об аэропортах
- `seats` – конфигурация мест в самолётах

**Объём данных:** 2.4+ млн строк

## 🛠️ Расширение функциональности

### Добавление нового пакета dbt

```bash
# Обновите packages.yml
echo "packages:
  - package: dbt-labs/dbt_utils
    version: 1.1.1
  - package: dbt-labs/dbt_expectations
    version: 0.5.0" > packages.yml

# Установите пакеты
dbt deps
```

### Использование пакетов

```sql
-- Использование макросов из dbt-utils
{{ dbt_utils.surrogate_key(['column1', 'column2']) }}

-- Использование тестов из dbt-expectations
- dbt_expectations.expect_column_values_to_be_in_set:
    value_set: ['active', 'inactive']
```

## 📚 Документация и Ресурсы

### Локальная документация
```bash
dbt docs generate
dbt docs serve  # http://localhost:8000
```

### Официальная документация
- [dbt Documentation](https://docs.getdbt.com/)
- [dbt Community Discourse](https://discourse.getdbt.com/)
- [dbt Hub - Packages](https://hub.getdbt.com/)
- [Jinja2 Template Engine](https://jinja.palletsprojects.com/)

### Полезные команды

```bash
# Выполнение конкретной модели
dbt run --select fct_flights

# Выполнение модели и её зависимостей
dbt run --select +fct_flights

# Выполнение только тестов для модели
dbt test --select fct_flights

# Генерация селектора для сложных запросов
dbt run --select tag:critical

# Сухой запуск (без выполнения)
dbt run --dry-run

# Очистка артефактов
dbt clean

# Профилирование выполнения
dbt run --profiles-dir ~/.dbt --debug
```

