📦 Модуль 2: Spring Boot проект — создание и настройка

🎯 **Цель модуля**  
Создать Spring Boot приложение с нуля, подключить его к PostgreSQL и Redis, настроить Liquibase для миграций БД и проверить работоспособность через Actuator.

**Результат:** У тебя будет работающий Spring Boot сервис, который подключается к инфраструктуре из Модуля 1 и готов для разработки бизнес-логики.

⏱️ **Время выполнения:** 3–4 часа

📋 **Чек-лист модуля**  
- [ ] Spring Boot проект создан (через IDEA или Spring Initializr)  
- [ ] Gradle настроен (dependencies, version catalog)  
- [ ] Понятна структура проекта (`src/main/java`, `resources` и т.д.)  
- [ ] `application.yml` настроен с профилями `dev` и `prod`  
- [ ] Подключение к PostgreSQL работает  
- [ ] Подключение к Redis работает  
- [ ] Liquibase настроен и применяет миграции  
- [ ] Создана первая миграция — таблица `users`  
- [ ] Spring Boot Actuator показывает `health` status  
- [ ] Приложение успешно стартует без ошибок

---

🚀 **Задание 1: Создание Spring Boot проекта**  
У тебя есть два варианта создания проекта. Выбери тот, который удобнее.

### Вариант А: Создание через IntelliJ IDEA (рекомендуется)

**Шаг 1.1: Открытие существующего проекта**  
- Открой IntelliJ IDEA  
- `File → Open`  
- Выбери папку `p2p-payment-system` (где у тебя `docker-compose.yml` из Модуля 1)  
- Нажми `OK`  
- IDEA откроет проект как обычную папку (пока без Java кода)

**Шаг 1.2: Добавление Spring Boot модуля**  
- `File → New → Module...`  
- В левом меню выбери `Spring Initializr`  
- Настрой параметры:

| Параметр       | Значение                        |
|----------------|---------------------------------|
| Name           | payment-service                 |
| Location       | `<твоя папка>/p2p-payment-system/payment-service` |
| Language       | Java                            |
| Type           | Gradle - Groovy                 |
| Group          | com.p2p                         |
| Artifact       | payment-service                 |
| Package name   | com.p2p.payment                 |
| JDK            | 21                              |
| Java           | 21                              |
| Packaging      | Jar                             |


Нажми `Next`


### Шаг 1.3: Выбор зависимостей  
В окне выбора `dependencies` отметь следующие:

**Developer Tools:**  
✅ Spring Boot DevTools  
✅ Lombok  

**Web:**  
✅ Spring Web  

**SQL:**  
✅ Spring Data JPA  
✅ PostgreSQL Driver  
✅ Liquibase Migration  

**NoSQL:**  
✅ Spring Data Redis (Access+Driver)  

**Ops:**  
✅ Spring Boot Actuator  

Нажми `Create`  
IDEA создаст модуль и начнет скачивать зависимости (это займет 1–3 минуты).

### Шаг 1.4: Проверка структуры  
После создания твой проект должен выглядеть так:

```
p2p-payment-system/
├── docs/
├── payment-service/ ← НОВЫЙ МОДУЛЬ
│ ├── src/
│ │ ├── main/
│ │ │ ├── java/
│ │ │ │ └── com/p2p/payment/
│ │ │ │ └── PaymentServiceApplication.java
│ │ │ └── resources/
│ │ │ └── application.properties
│ │ └── test/
│ ├── build.gradle
│ └── settings.gradle
├── docker-compose.yml
├── .gitignore
└── README.md
```

✅ **Критерий выполнения:**  
Видишь папку `payment-service` с Java кодом внутри. Gradle синхронизировался без ошибок.


#### Шаг 1.1: Генерация проекта
- Открой браузер: [https://start.spring.io](https://start.spring.io)  
- Настрой параметры:

| Параметр         | Значение                        |
|------------------|---------------------------------|
| Project          | Gradle - Groovy                 |
| Language         | Java                            |
| Spring Boot      | 3.2.x (последняя стабильная)    |
| **Project Metadata** |                                 |
| - Group          | com.p2p                         |
| - Artifact       | payment-service                 |
| - Name           | payment-service                 |
| - Description    | P2P Payment Service             |
| - Package name   | com.p2p.payment                 |
| Packaging        | Jar                             |
| Java             | 21                              |

- Нажми `ADD DEPENDENCIES` и добавь:
  - Spring Web
  - Spring Data JPA
  - PostgreSQL Driver
  - Spring Data Redis (Access+Driver)
  - Liquibase Migration
  - Spring Boot Actuator
  - Spring Boot DevTools
  - Lombok
- Нажми `GENERATE` (скачается архив `payment-service.zip`)

#### Шаг 1.2: Распаковка проекта
- Распакуй `payment-service.zip`
- Скопируй папку `payment-service` в корень `p2p-payment-system/`

**Windows (PowerShell):**
```powershell
cd C:\Projects\p2p-payment-system
Expand-Archive -Path "C:\Downloads\payment-service.zip" -DestinationPath .
```

#### Шаг 1.3: Открытие в IDEA
- `File → Open`
- Выбери папку `p2p-payment-system/payment-service`
- Нажми `OK`
- IDEA предложит импортировать Gradle проект — соглашайся

✅ **Критерий выполнения:**  
Проект открылся в IDEA, Gradle синхронизировался, видишь класс `PaymentServiceApplication.java`.

---

🔗 **Интеграция с репозиторием из Модуля 1**  
После создания проекта любым способом выполни:

```bash
cd p2p-payment-system

# Проверь статус Git
git status
```

Ожидаемый вывод:
```
On branch main
Untracked files:
  payment-service/
```
Добавь новый модуль в Git:

```bash
# Обнови .gitignore (он уже настроен из Модуля 1)
git add .

# Коммит
git commit -m "feat: Add Spring Boot payment-service module"

# Отправка на GitHub
git push
```

Обнови README.md в корне проекта:
```
# 💳 P2P Payment System

## 📁 Структура проекта
```
```
p2p-payment-system/
├── payment-service/ # Spring Boot микросервис
├── docs/ # Документация
└── docker-compose.yml # PostgreSQL + Redis
```
```

## 🚀 Запуск

### 1. Инфраструктура
```bash
docker compose up -d
```
### 2. Backend сервис
```bash
cd payment-service
./gradlew bootRun
```

```
git add README.md
git commit -m "docs: Update README with project structure"
git push
```

## 📚 Лекция: Структура Gradle проекта

### Что такое Gradle?
Gradle — инструмент автоматизации сборки проектов (build tool). Он:

- Управляет зависимостями (библиотеками)
- Компилирует Java код
- Запускает тесты
- Создает JAR/WAR файлы для деплоя

**Альтернативы:** Maven, Ant (устарел).

### Основные файлы Gradle
Открой `payment-service/` и изучи файлы:

#### 1. `settings.gradle`
Определяет имя проекта:
```groovy
rootProject.name = 'payment-service'
```
Если у тебя multi-module проект (несколько сервисов), здесь перечисляются все модули.

#### 2. `build.gradle`
Главный файл конфигурации. Содержит:

- Плагины (plugins)
- Версию Java
- Зависимости (dependencies)
- Настройки сборки

**Пример структуры:**
```groovy
plugins {
    id 'java'
    id 'org.springframework.boot' version '3.2.1'
    id 'io.spring.dependency-management' version '1.1.4'
}

group = 'com.p2p'
version = '0.0.1-SNAPSHOT'

java {
    sourceCompatibility = '21'
}

repositories {
    mavenCentral()  // Откуда скачивать библиотеки
}

dependencies {
    // Spring Boot стартеры
    implementation 'org.springframework.boot:spring-boot-starter-web'
    implementation 'org.springframework.boot:spring-boot-starter-data-jpa'
    
    // БД драйверы
    runtimeOnly 'org.postgresql:postgresql'
    
    // Тесты
    testImplementation 'org.springframework.boot:spring-boot-starter-test'
}

tasks.named('test') {
    useJUnitPlatform()
}
```

#### 3. `gradlew` и `gradlew.bat`
Gradle Wrapper — скрипты для запуска Gradle без его установки в систему.
```cmd
gradlew.bat build
```
Gradle Wrapper автоматически скачает нужную версию Gradle при первом запуске.

### Структура папок

```
payment-service/
├── gradle/              # Gradle Wrapper файлы
├── src/
│   ├── main/
│   │   ├── java/        # Исходный код Java
│   │   └── resources/   # Конфигурации, SQL, статика
│   └── test/
│       ├── java/        # Тесты
│       └── resources/   # Тестовые конфигурации
├── build/               # Скомпилированные файлы (git ignore)
├── .gitignore
├── build.gradle         # Конфигурация Gradle
├── settings.gradle
├── gradlew              # Gradle Wrapper (Unix)
└── gradlew.bat          # Gradle Wrapper (Windows)
```

### Важные папки:

### Структура папок в Gradle проекте

| Папка                   | Назначение |
|-------------------------|-----------|
| `src/main/java`         | Твой Java код (контроллеры, сервисы, entities) |
| `src/main/resources`    | `application.yml`, Liquibase миграции, шаблоны |
| `src/test/java`         | Юнит и интеграционные тесты |
| `build/`                | Временные файлы сборки (НЕ коммитить в Git!) |

---

### Команды Gradle  
Открой терминал в папке `payment-service/`:

```bash
# Сборка проекта
./gradlew build

# Запуск приложения
./gradlew bootRun

# Очистка build папки
./gradlew clean

# Запуск тестов
./gradlew test

# Обновление зависимостей
./gradlew --refresh-dependencies
```

### Комбинация команд:

```bash
# Очистить + собрать + запустить
./gradlew clean build bootRun
```

## 🛠️ Задание 2: Настройка Gradle (dependencies)

### Шаг 2.1: Открытие `build.gradle`
Открой файл `payment-service/build.gradle` в IDEA.

### Шаг 2.2: Анализ зависимостей
Найди блок `dependencies`:
```groovy
dependencies {
    implementation 'org.springframework.boot:spring-boot-starter-web'
    implementation 'org.springframework.boot:spring-boot-starter-data-jpa'
    implementation 'org.springframework.boot:spring-boot-starter-data-redis'
    implementation 'org.springframework.boot:spring-boot-starter-actuator'
    
    implementation 'org.liquibase:liquibase-core'
    
    runtimeOnly 'org.postgresql:postgresql'
    
    compileOnly 'org.projectlombok:lombok'
    annotationProcessor 'org.projectlombok:lombok'
    
    developmentOnly 'org.springframework.boot:spring-boot-devtools'
    
    testImplementation 'org.springframework.boot:spring-boot-starter-test'
    testRuntimeOnly 'org.junit.platform:junit-platform-launcher'
}
```

**Объяснение scope (области видимости):**

| Scope | Когда используется | Пример |
|-------|-------------------|--------|
| `implementation` | Основной код приложения | Spring Web, JPA |
| `runtimeOnly` | Нужно только во время выполнения | PostgreSQL драйвер |
| `compileOnly` | Нужно только при компиляции | Lombok (генерирует код) |
| `annotationProcessor` | Обработка аннотаций при компиляции | Lombok |
| `developmentOnly` | Только для разработки | DevTools (автоперезагрузка) |
| `testImplementation` | Только для тестов | JUnit, Mockito |

### Шаг 2.3: Добавление дополнительных зависимостей
Добавь в блок `dependencies`:
```groovy
dependencies {
    // ... существующие зависимости ...
    
    // Validation API
    implementation 'org.springframework.boot:spring-boot-starter-validation'
    
    // MapStruct для маппинга DTO
    implementation 'org.mapstruct:mapstruct:1.5.5.Final'
    annotationProcessor 'org.mapstruct:mapstruct-processor:1.5.5.Final'
     // Lettuce (Redis client) - уже включен в spring-boot-starter-data-redis
    // но если нужна явная версия:
    // implementation 'io.lettuce:lettuce-core:6.3.0.RELEASE'
}
```
### Шаг 2.4: Настройка Version Catalog (современный подход)
Version Catalog — это способ централизованного управления версиями зависимостей в Gradle.

Создай файл `payment-service/gradle/libs.versions.toml`:
```toml
[versions]
spring-boot = "3.2.1"
mapstruct = "1.5.5.Final"
lombok = "1.18.30"

[libraries]
# Spring Boot
spring-boot-starter-web = { module = "org.springframework.boot:spring-boot-starter-web" }
spring-boot-starter-data-jpa = { module = "org.springframework.boot:spring-boot-starter-data-jpa" }
spring-boot-starter-data-redis = { module = "org.springframework.boot:spring-boot-starter-data-redis" }
spring-boot-starter-actuator = { module = "org.springframework.boot:spring-boot-starter-actuator" }
spring-boot-starter-validation = { module = "org.springframework.boot:spring-boot-starter-validation" }

# Database
postgresql = { module = "org.postgresql:postgresql" }
liquibase = { module = "org.liquibase:liquibase-core" }

# Mapping
mapstruct = { module = "org.mapstruct:mapstruct", version.ref = "mapstruct" }
mapstruct-processor = { module = "org.mapstruct:mapstruct-processor", version.ref = "mapstruct" }

# Tools
lombok = { module = "org.projectlombok:lombok", version.ref = "lombok" }

[plugins]
spring-boot = { id = "org.springframework.boot", version.ref = "spring-boot" }
```

**Преимущества:**
✅ Одно место для управления версиями  
✅ Легко обновлять все зависимости  
✅ Переиспользование между модулями (если их несколько)  

**Использование в `build.gradle`:**
```groovy
plugins {
    id 'java'
    alias(libs.plugins.spring.boot)
    id 'io.spring.dependency-management' version '1.1.4'
}

dependencies {
    implementation libs.spring.boot.starter.web
    implementation libs.spring.boot.starter.data.jpa
    // ... и т.д.
}
```

Для нашего курса мы пока будем использовать обычный способ (без version catalog), чтобы не усложнять. Но знай, что это best practice для больших проектов.

### Шаг 2.5: Синхронизация Gradle
После изменений в `build.gradle`:

- В IDEA справа появится иконка Gradle 🐘
- Кликни `Reload All Gradle Projects` (иконка обновления)
- Или нажми `Ctrl+Shift+O` (Windows/Linux) / `Cmd+Shift+I` (Mac)
- Gradle скачает новые зависимости (это может занять 1–2 минуты).

✅ **Критерий выполнения:**  
Gradle синхронизировался без ошибок. В разделе External Libraries видишь все зависимости (Spring Boot, PostgreSQL, Lombok и т.д.).

## ⚙️ Задание 3: Настройка `application.yml`

### Шаг 3.1: Переименование файла
По умолчанию Spring Initializr создает `application.properties`. Мы будем использовать YAML формат (более читаемый).

- В папке `src/main/resources/` найди файл `application.properties`
- ПКМ → `Refactor` → `Rename`
- Измени имя на `application.yml`

### Шаг 3.2: Базовая конфигурация
Открой `application.yml` и добавь:
```yaml
# Общие настройки приложения
spring:
  application:
    name: payment-service
  
  # Профиль по умолчанию (локальная разработка)
  profiles:
    active: dev

# Настройки сервера
server:
  port: 8080
  shutdown: graceful

# Логирование
logging:
  level:
    root: INFO
    com.p2p.payment: DEBUG
  pattern:
    console: "%d{yyyy-MM-dd HH:mm:ss} - %msg%n"

# Actuator endpoints
management:
  endpoints:
    web:
      exposure:
        include: health,info,metrics
  endpoint:
    health:
      show-details: always
```

### Шаг 3.3: Создание профилей (dev, prod)
**Зачем нужны профили?**

- `dev` — локальная разработка (подробные логи, H2 console, автоперезагрузка)
- `prod` — продакшн (минимум логов, отключены debug endpoints)

Создай файлы:

#### `application-dev.yml` (для локальной разработки)
Создай файл `src/main/resources/application-dev.yml`:
```yaml
# DEV PROFILE - Локальная разработка

spring:
  # Подключение к PostgreSQL из Docker
  datasource:
    url: jdbc:postgresql://localhost:5432/p2p_payment_db
    username: p2p_user
    password: p2p_password
    driver-class-name: org.postgresql.Driver
    
    # HikariCP настройки (connection pool)
    hikari:
      maximum-pool-size: 5
      minimum-idle: 2
      connection-timeout: 30000
      idle-timeout: 600000
      max-lifetime: 1800000

  # JPA/Hibernate
  jpa:
    database-platform: org.hibernate.dialect.PostgreSQLDialect
    show-sql: true  # Показывать SQL запросы в консоли
    properties:
      hibernate:
        format_sql: true  # Форматировать SQL для читаемости
        use_sql_comments: true
    hibernate:
      ddl-auto: validate  # ВАЖНО! Liquibase управляет схемой, не Hibernate

  # Redis подключение
  data:
    redis:
      host: localhost
      port: 6379
      password: redis_password
      timeout: 2000ms
      lettuce:
        pool:
          max-active: 8
          max-idle: 8
          min-idle: 2
          max-wait: -1ms

  # Liquibase миграции
  liquibase:
    enabled: true
    change-log: classpath:db/changelog/db.changelog-master.yaml
    default-schema: public

# Логирование SQL с параметрами
logging:
  level:
    org.hibernate.SQL: DEBUG
    org.hibernate.type.descriptor.sql.BasicBinder: TRACE
    com.p2p.payment: DEBUG
```

#### `application-prod.yml` (для продакшна)
Создай файл `src/main/resources/application-prod.yml`:
```yaml
# PROD PROFILE - Production окружение

spring:
  # Подключение к PostgreSQL из переменных окружения
  datasource:
    url: ${DATABASE_URL}
    username: ${DATABASE_USERNAME}
    password: ${DATABASE_PASSWORD}
    driver-class-name: org.postgresql.Driver
    
    hikari:
      maximum-pool-size: 20
      minimum-idle: 5
      connection-timeout: 30000
      idle-timeout: 600000
      max-lifetime: 1800000

  jpa:
    database-platform: org.hibernate.dialect.PostgreSQLDialect
    show-sql: false  # НЕ показывать SQL в продакшне
    hibernate:
      ddl-auto: validate

  data:
    redis:
      host: ${REDIS_HOST}
      port: ${REDIS_PORT}
      password: ${REDIS_PASSWORD}
      timeout: 2000ms
      lettuce:
        pool:
          max-active: 50
          max-idle: 20
          min-idle: 5

  liquibase:
    enabled: true
    change-log: classpath:db/changelog/db.changelog-master.yaml

# Минимальное логирование
logging:
  level:
    root: WARN
    com.p2p.payment: INFO

# Actuator - только health для продакшна
management:
  endpoints:
    web:
      exposure:
        include: health
  endpoint:
    health:
      show-details: when-authorized
```
### Шаг 3.4: Объяснение ключевых параметров

#### DataSource (PostgreSQL)
```yaml
spring:
  datasource:
    url: jdbc:postgresql://localhost:5432/p2p_payment_db
    username: p2p_user
    password: p2p_password
```

`url` — строка подключения к БД (host:port/database)  
`username/password` — из `docker-compose.yml` (Модуль 1)

#### HikariCP (Connection Pool)
Connection Pool — механизм переиспользования соединений с БД (не создавать новое для каждого запроса).

```yaml
hikari:
  maximum-pool-size: 5  # Максимум 5 одновременных соединений
  minimum-idle: 2       # Минимум 2 всегда открытых
  connection-timeout: 30000  # Таймаут получения соединения (30 сек)
```

#### JPA/Hibernate
```yaml
jpa:
  hibernate:
    ddl-auto: validate  # Проверять схему БД, но НЕ изменять
```

Возможные значения `ddl-auto`:

| Значение   | Что делает |
|------------|------------|
| `validate` | Проверяет что схема БД соответствует Entity классам |
| `update`   | Автоматически изменяет схему (ОПАСНО!) |
| `create`   | Удаляет и пересоздает схему при старте |
| `none`     | Ничего не делает |

Для продакшна ВСЕГДА используй `validate` — схемой управляет Liquibase.

#### Redis (Lettuce)
Lettuce — это современный асинхронный Redis клиент для Java (по умолчанию в Spring Boot).

Альтернатива: Jedis (старый, синхронный).

```yaml
data:
  redis:
    host: localhost
    port: 6379
    password: redis_password
    lettuce:
      pool:
        max-active: 8  # Максимум 8 соединений в pool
```
**Преимущества Lettuce:**

✅ Асинхронный (non-blocking I/O)  
✅ Поддержка reactive programming (Spring WebFlux)  
✅ Автореконнект при обрывах соединения  

### Шаг 3.5: Выбор активного профиля
**Способ 1:** В `application.yml` (уже настроено)
```yaml
spring:
  profiles:
    active: dev
```
**Способ 2:** Переменная окружения
```bash
# Windows (PowerShell)
$env:SPRING_PROFILES_ACTIVE="prod"
gradlew.bat bootRun
```

**Способ 3:** Аргумент командной строки

```bash
./gradlew bootRun --args='--spring.profiles.active=prod'
```

**Способ 4:** В IDEA (для разработки)

- `Run → Edit Configurations...`
- Выбери конфигурацию `PaymentServiceApplication`
- В поле `VM options` добавь: `-Dspring.profiles.active=dev`
- `Apply → OK`

✅ **Критерий выполнения:**  
Созданы файлы `application.yml`, `application-dev.yml`, `application-prod.yml`. Активный профиль — `dev`.

## 🗄️ Задание 4: Подключение к PostgreSQL

### Шаг 4.1: Проверка что БД запущена
```bash
docker compose ps
```
Контейнер p2p-postgres должен быть в статусе Up.

Если нет:
```bash
docker compose up -d
```

### Шаг 4.2: Создание Entity класса (пока простой)
Создай класс `src/main/java/com/p2p/payment/entity/User.java`:
```java
package com.p2p.payment.entity;

import jakarta.persistence.*;
import lombok.*;

import java.time.LocalDateTime;

@Entity
@Table(name = "users")
@Getter
@Setter
@NoArgsConstructor
@AllArgsConstructor
@Builder
public class User {
    
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    
    @Column(nullable = false, unique = true, length = 50)
    private String email;
    
    @Column(name = "created_at", nullable = false, updatable = false)
    private LocalDateTime createdAt;
    
    @PrePersist
    protected void onCreate() {
        createdAt = LocalDateTime.now();
    }
}
```

**Объяснение аннотаций:**

- `@Entity` — JPA класс, соответствует таблице в БД
- `@Table(name = "users")` — имя таблицы (по умолчанию = имени класса)
- `@Id` — первичный ключ
- `@GeneratedValue` — автоинкремент ID
- `@Column` — настройки столбца (nullable, unique, length)
- `@PrePersist` — метод вызывается перед сохранением в БД

**Lombok аннотации:**

- `@Getter/@Setter` — генерирует геттеры/сеттеры
- `@NoArgsConstructor` — пустой конструктор
- `@AllArgsConstructor` — конструктор со всеми полями
- `@Builder` — паттерн Builder для создания объектов

**Пример использования Builder:**
```java
User user = User.builder()
    .email("john@example.com")
    .build();
```

### Шаг 4.3: Создание Repository
Создай интерфейс `src/main/java/com/p2p/payment/repository/UserRepository.java`:
```java
package com.p2p.payment.repository;

import com.p2p.payment.entity.User;
import org.springframework.data.jpa.repository.JpaRepository;
import org.springframework.stereotype.Repository;

import java.util.Optional;

@Repository
public interface UserRepository extends JpaRepository<User, Long> {   
    Optional<User> findByEmail(String email);
    
    boolean existsByEmail(String email);
}
```

**Объяснение:**

- `JpaRepository<User, Long>` — Spring Data JPA автоматически создаст реализацию
  - `User` — тип Entity
  - `Long` — тип ID

**Встроенные методы (из `JpaRepository`):**
- `save(User)` — создать/обновить
- `findById(Long)` — найти по ID
- `findAll()` — получить все записи
- `deleteById(Long)` — удалить
- `count()` — количество записей

**Кастомные методы (Spring Data создаст SQL автоматически):**
- `findByEmail` — `SELECT * FROM users WHERE email = ?`
- `existsByEmail` — `SELECT COUNT(*) > 0 FROM users WHERE email = ?`

### Шаг 4.4: Создание тестового компонента для проверки
Создай класс `src/main/java/com/p2p/payment/config/DatabaseHealthCheck.java`:
```java
package com.p2p.payment.config;

import lombok.RequiredArgsConstructor;
import lombok.extern.slf4j.Slf4j;
import org.springframework.boot.CommandLineRunner;
import org.springframework.jdbc.core.JdbcTemplate;
import org.springframework.stereotype.Component;

@Component
@RequiredArgsConstructor
@Slf4j
public class DatabaseHealthCheck implements CommandLineRunner {

    private final JdbcTemplate jdbcTemplate;

    @Override
    public void run(String... args) {
        try {
            String result = jdbcTemplate.queryForObject(
                "SELECT current_database()", 
                String.class
            );
            log.info("✅ PostgreSQL connection successful! Database: {}", result);
            
            // Проверка версии PostgreSQL
            String version = jdbcTemplate.queryForObject(
                "SELECT version()", 
                String.class
            );
            log.info("📊 PostgreSQL version: {}", version);
            
        } catch (Exception e) {
            log.error("❌ PostgreSQL connection failed: {}", e.getMessage());
        }
    }
}
```
**Что делает этот класс:**

- `CommandLineRunner` — выполняется после старта приложения
- `JdbcTemplate` — утилита Spring для выполнения SQL запросов
- `@Slf4j` — Lombok аннотация, добавляет переменную `log`

### Шаг 4.5: Запуск приложения
```bash
cd payment-service
./gradlew bootRun
```

**Ожидаемый вывод в консоли:**
```
2024-01-15 10:30:45 - Starting PaymentServiceApplication
2024-01-15 10:30:46 - Tomcat started on port(s): 8080 (http)
2024-01-15 10:30:46 - ✅ PostgreSQL connection successful! Database: p2p_payment_db
2024-01-15 10:30:46 - 📊 PostgreSQL version: PostgreSQL 15.5 on x86_64-pc-linux-gnu
2024-01-15 10:30:46 - Started PaymentServiceApplication in 2.345 seconds
```

### Шаг 4.6: Проверка в DBeaver (опционально)
- Открой DBeaver
- Подключись к БД `p2p_payment_db`
- Открой SQL консоль и выполни:
```sql
-- Проверка активных соединений
SELECT 
    application_name,
    state,
    query
FROM pg_stat_activity
WHERE datname = 'p2p_payment_db';
```

Ты увидишь соединение от PaymentServiceApplication (через HikariCP).

✅ Критерий выполнения:
Приложение запустилось, в логах видно сообщение "✅ PostgreSQL connection successful!".



## 🔴 Задание 5: Подключение к Redis

### Шаг 5.1: Проверка что Redis запущен
```bash
docker compose ps
```
Контейнер p2p-redis должен быть в статусе Up.

Тест подключения:

```bash
# Windows (PowerShell)
docker exec -it p2p-redis redis-cli -a redis_password PING
```

**Ожидаемый ответ:** `PONG`

### Шаг 5.2: Теория — RedisTemplate vs. Lettuce

**Lettuce (низкоуровневый клиент)**
- Библиотека для работы с Redis
- Асинхронный, reactive
- Используется Spring Boot под капотом

**RedisTemplate (Spring абстракция)**
- High-level API от Spring
- Работает поверх Lettuce
- Предоставляет удобные методы для разных типов данных

**Структура RedisTemplate:**

```
RedisTemplate
├── opsForValue() → String операции (SET, GET)
├── opsForList()  → List операции (LPUSH, RPOP)
├── opsForSet()   → Set операции (SADD, SMEMBERS)
├── opsForHash()  → Hash операции (HSET, HGET)
└── opsForZSet()  → Sorted Set операции
```

### Шаг 5.3: Конфигурация RedisTemplate
Создай класс `src/main/java/com/p2p/payment/config/RedisConfig.java`:
```java
package com.p2p.payment.config;

import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.data.redis.connection.RedisConnectionFactory;
import org.springframework.data.redis.core.RedisTemplate;
import org.springframework.data.redis.serializer.GenericJackson2JsonRedisSerializer;
import org.springframework.data.redis.serializer.StringRedisSerializer;

@Configuration
public class RedisConfig {

    @Bean
    public RedisTemplate<String, Object> redisTemplate(
            RedisConnectionFactory connectionFactory) {
        
        RedisTemplate<String, Object> template = new RedisTemplate<>();
        template.setConnectionFactory(connectionFactory);
        
        // Сериализация ключей как String
        template.setKeySerializer(new StringRedisSerializer());
        template.setHashKeySerializer(new StringRedisSerializer());
        
        // Сериализация значений как JSON
        template.setValueSerializer(new GenericJackson2JsonRedisSerializer());
        template.setHashValueSerializer(new GenericJackson2JsonRedisSerializer());
        
        template.afterPropertiesSet();
        return template;
    }
}
```

**Объяснение:**

- `StringRedisSerializer` — ключи хранятся как строки (например: `"user:123"`)
- `GenericJackson2JsonRedisSerializer` — значения хранятся как JSON
  - Можно сохранять сложные объекты (`User`, `Transaction` и т.д.)
  - Redis автоматически сериализует/десериализует через Jackson

**Альтернативы сериализации:**

| Сериализатор | Применение |
|--------------|------------|
| `StringRedisSerializer` | Простые строки |
| `GenericJackson2JsonRedisSerializer` | JSON (рекомендуется) |
| `JdkSerializationRedisSerializer` | Java Serializable (НЕ рекомендуется) |

### Шаг 5.4: Создание Health Check для Redis
Добавь в класс `DatabaseHealthCheck` проверку Redis:
```java
package com.p2p.payment.config;

import lombok.RequiredArgsConstructor;
import lombok.extern.slf4j.Slf4j;
import org.springframework.boot.CommandLineRunner;
import org.springframework.data.redis.core.RedisTemplate;
import org.springframework.jdbc.core.JdbcTemplate;
import org.springframework.stereotype.Component;

@Component
@RequiredArgsConstructor
@Slf4j
public class DatabaseHealthCheck implements CommandLineRunner {

    private final JdbcTemplate jdbcTemplate;
    private final RedisTemplate<String, Object> redisTemplate;

    @Override
    public void run(String... args) {
        checkPostgreSQL();
        checkRedis();
    }

    private void checkPostgreSQL() {
        try {
            String result = jdbcTemplate.queryForObject(
                "SELECT current_database()", 
                String.class
            );
            log.info("✅ PostgreSQL connection successful! Database: {}", result);
        } catch (Exception e) {
            log.error("❌ PostgreSQL connection failed: {}", e.getMessage());
        }
    }

    private void checkRedis() {
        try {
            // Тестовая запись
            String testKey = "health:check";
            String testValue = "OK";
            
            redisTemplate.opsForValue().set(testKey, testValue);
            String result = (String) redisTemplate.opsForValue().get(testKey);
            
            if (testValue.equals(result)) {
                log.info("✅ Redis connection successful! Test key: {}", testKey);
                redisTemplate.delete(testKey); // Удаляем тестовый ключ
            }
        } catch (Exception e) {
            log.error("❌ Redis connection failed: {}", e.getMessage());
        }
    }
}
```

### Шаг 5.5: Создание Redis Service (пример использования)
Создай класс `src/main/java/com/p2p/payment/service/CacheService.java`:
```java
package com.p2p.payment.service;

import lombok.RequiredArgsConstructor;
import lombok.extern.slf4j.Slf4j;
import org.springframework.data.redis.core.RedisTemplate;
import org.springframework.stereotype.Service;

import java.time.Duration;
import java.util.concurrent.TimeUnit;

@Service
@RequiredArgsConstructor
@Slf4j
public class CacheService {

    private final RedisTemplate<String, Object> redisTemplate;

    /**
     * Сохранить значение с TTL (Time To Live)
     */
    public void set(String key, Object value, Duration ttl) {
        redisTemplate.opsForValue().set(key, value, ttl);
        log.debug("📝 Redis SET: {} = {}, TTL: {}", key, value, ttl);
    }

    /**
     * Получить значение по ключу
     */
    public Object get(String key) {
        Object value = redisTemplate.opsForValue().get(key);
        log.debug("📖 Redis GET: {} = {}", key, value);
        return value;
    }

    /**
     * Удалить ключ
     */
    public void delete(String key) {
        redisTemplate.delete(key);
        log.debug("🗑️ Redis DELETE: {}", key);
    }

    /**
     * Проверить существование ключа
     */
    public boolean exists(String key) {
        Boolean exists = redisTemplate.hasKey(key);
        return Boolean.TRUE.equals(exists);
    }

    /**
     * Установить TTL для существующего ключа
     */
    public void expire(String key, Duration ttl) {
        redisTemplate.expire(key, ttl);
        log.debug("⏱️ Redis EXPIRE: {} = {}", key, ttl);
    }
}
```
**Примеры использования:**
```java
// Кэширование данных пользователя на 1 час
cacheService.set("user:123", userObject, Duration.ofHours(1));

// Получение из кэша
User cachedUser = (User) cacheService.get("user:123");

// Проверка существования
if (cacheService.exists("user:123")) {
    // ...
}
```
### Шаг 5.6: Запуск и проверка
```bash
./gradlew bootRun
```

### Ожидаемый вывод:
```
2024-01-15 10:35:12 - ✅ PostgreSQL connection successful! Database: p2p_payment_db
2024-01-15 10:35:12 - ✅ Redis connection successful! Test key: health:check
```
### Шаг 5.7: Проверка в Redis CLI (опционально)

```bash

docker exec -it p2p-redis redis-cli -a redis_password

# Внутри redis-cli:
127.0.0.1:6379> KEYS *
127.0.0.1:6379> SET test:key "Hello Redis"
127.0.0.1:6379> GET test:key
127.0.0.1:6379> TTL test:key
127.0.0.1:6379> DEL test:key
127.0.0.1:6379> exit
```

✅ Критерий выполнения:
Приложение запустилось, в логах видно "✅ Redis connection successful!".

## 🗂️ Задание 6: Настройка Liquibase

### Шаг 6.1: Теория — Зачем нужен Liquibase?
**Проблема:** В команде разработчиков каждый вносит изменения в структуру БД (добавляет таблицы, колонки, индексы). Как синхронизировать изменения между dev/staging/prod?

**Решение:** Liquibase — инструмент версионирования схемы БД (database migration tool).

**Принцип работы:**

- Каждое изменение БД описывается в файле миграции (changeset)
- Liquibase отслеживает какие миграции применены (таблица `databasechangelog`)
- При старте приложения применяет только новые миграции

**Альтернативы:** Flyway (проще, но менее гибкий).

### Шаг 6.2: Создание структуры папок
Создай следующую структуру в `src/main/resources/`:
```
resources/
└── db/
    └── changelog/
        ├── db.changelog-master.yaml    ← главный файл
        └── changes/
            └── v1/
                └── 001-create-users-table.yaml
```
Команды для создания:

**Windows (PowerShell)**
```powershell
cd src\main\resources
New-Item -ItemType Directory -Path "db\changelog\changes\v1" -Force
```

### Шаг 6.3: Создание master файла
Создай src/main/resources/db/changelog/db.changelog-master.yaml:

```yaml

databaseChangeLog:
  - include:
      file: db/changelog/changes/v1/001-create-users-table.yaml
```

### Шаг 6.4: Формат миграций (YAML vs SQL vs XML)
Liquibase поддерживает 3 формата:

| Формат | Преимущества | Недостатки |
|--------|--------------|------------|
| YAML | Читаемый, декларативный, независимый от БД | Нет автодополнения IDE |
| SQL | Привычный синтаксис, поддержка в IDE | Зависит от конкретной БД |
| XML | Строгая валидация, автодополнение | Многословный |

Для курса будем использовать YAML — золотая середина.

### Шаг 6.5: Создание первой миграции
Создай файл `src/main/resources/db/changelog/changes/v1/001-create-users-table.yaml`:

```yaml

databaseChangeLog:
  - changeSet:
      id: 001-create-users-table
      author: your_name
      changes:
        - createTable:
            tableName: users
            columns:
              - column:
                  name: id
                  type: BIGINT
                  autoIncrement: true
                  constraints:
                    primaryKey: true
                    primaryKeyName: pk_users
              - column:
                  name: email
                  type: VARCHAR(50)
                  constraints:
                    nullable: false
                    unique: true
                    uniqueConstraintName: uq_users_email
              - column:
                  name: phone_number
                  type: VARCHAR(20)
                  constraints:
                    nullable: false
                    unique: true
                    uniqueConstraintName: uq_users_phone
              - column:
                  name: first_name
                  type: VARCHAR(100)
                  constraints:
                    nullable: false
              - column:
                  name: last_name
                  type: VARCHAR(100)
                  constraints:
                    nullable: false
              - column:
                  name: status
                  type: VARCHAR(20)
                  defaultValue: ACTIVE
                  constraints:
                    nullable: false
              - column:
                  name: created_at
                  type: TIMESTAMP
                  defaultValueComputed: CURRENT_TIMESTAMP
                  constraints:
                    nullable: false
              - column:
                  name: updated_at
                  type: TIMESTAMP
                  defaultValueComputed: CURRENT_TIMESTAMP
                  constraints:
                    nullable: false

        - createIndex:
            indexName: idx_users_email
            tableName: users
            columns:
              - column:
                  name: email

        - createIndex:
            indexName: idx_users_phone
            tableName: users
            columns:
              - column:
                  name: phone_number

      rollback:
        - dropTable:
            tableName: users
```

### Шаг 6.6: Разбор структуры changeSet
Обязательные поля:

```yaml

- changeSet:
    id: 001-create-users-table        # Уникальный ID миграции
    author: your_name                  # Автор изменения
    changes:                           # Список изменений
      - createTable: ...
```

```yaml
databaseChangeLog:
  - changeSet:
      id: 001-create-users-table
      author: your_name
      description: Create users table for storing user information
      changes:
        - createTable:
            tableName: users
            columns:
              - column:
                  name: id
                  type: BIGSERIAL
                  autoIncrement: true
                  constraints:
                    primaryKey: true
                    nullable: false
              - column:
                  name: email
                  type: VARCHAR(50)
                  constraints:
                    nullable: false
                    unique: true
              - column:
                  name: created_at
                  type: TIMESTAMP
                  defaultValueComputed: CURRENT_TIMESTAMP
                  constraints:
                    nullable: false
              - column:
                  name: updated_at
                  type: TIMESTAMP
                  defaultValueComputed: CURRENT_TIMESTAMP
```

**Важно:** Комбинация `id + author + filepath` должна быть уникальной!

### Типы изменений (`changes`):

**Создание объектов:**
- `createTable` — создать таблицу
- `createIndex` — создать индекс
- `createSequence` — создать sequence (для Oracle)
- `createView` — создать представление

**Изменение объектов:**
- `addColumn` — добавить колонку
- `modifyDataType` — изменить тип данных
- `addNotNullConstraint` — добавить NOT NULL
- `addForeignKeyConstraint` — добавить внешний ключ

**Удаление объектов:**
- `dropTable` — удалить таблицу
- `dropColumn` — удалить колонку
- `dropIndex` — удалить индекс

**Данные:**
- `insert` — вставить строки
- `update` — обновить строки
- `delete` — удалить строки
- `loadData` — загрузить из CSV

Rollback:

```yaml


rollback:
  - dropTable:
      tableName: users
```

Rollback — откат миграции (если что-то пошло не так).

Команда для отката:

```bash
./gradlew liquibaseRollback -PliquibaseCommandValue=001-create-users-table
```

Шаг 6.7: Best Practices для миграций
1. Именование файлов
Используй формат: <номер>-<описание>.yaml

```
001-create-users-table.yaml
002-create-wallets-table.yaml
003-add-kyc-status-to-users.yaml
004-create-transactions-table.yaml
```

2. Один changeSet = одна логическая операция
❌ Плохо:

```yaml
- changeSet:
    id: initial-schema
    changes:
      - createTable: users
      - createTable: wallets
      - createTable: transactions
```

✅ Хорошо:
```yaml

# 001-create-users-table.yaml
- changeSet:
    id: 001-create-users-table
    changes:
      - createTable: users

# 002-create-wallets-table.yaml
- changeSet:
    id: 002-create-wallets-table
    changes:
      - createTable: wallets
```

**Почему? ❌ Если wallets упадет, users уже будет создан. Откатить будет сложнее.**

### 3. Всегда добавляй rollback

```yaml

rollback:
  - dropTable:
      tableName: users
```


### 4. Используй preconditions (проверки перед выполнением)

```yaml

- changeSet:
    id: 002-add-middle-name
    author: your_name
    preconditions:
      - onFail: MARK_RAN  # Пропустить если условие не выполнено
      - not:
          - columnExists:
              tableName: users
              columnName: middle_name
    changes:
      - addColumn:
          tableName: users
          columns:
            - column:
                name: middle_name
                type: VARCHAR(100)
```

Значения `onFail`:

- `HALT` — остановить выполнение (по умолчанию)
- `MARK_RAN` — пометить как выполненный, но пропустить
- `WARN` — показать предупреждение, продолжить

**5. Не изменяй старые миграции**
❌ Никогда не делай:

```yaml

# Файл 001-create-users-table.yaml УЖЕ применен на prod
# И ты решил добавить колонку:
- column:
    name: middle_name  # ← НЕПРАВИЛЬНО!
```

✅ Правильно: Создай новую миграцию 002-add-middle-name-to-users.yaml:1ms
```yaml

- changeSet:
    id: 002-add-middle-name
    changes:
      - addColumn:
          tableName: users
          columns:
            - column:
                name: middle_name
                type: VARCHAR(100)
```

Почему? Liquibase хранит checksum (хэш) каждой миграции в таблице databasechangelog. ❌ Если файл изменился — приложение не запустится.

### Шаг 6.8: Обновление Entity класса User
Обнови `src/main/java/com/p2p/payment/entity/User.java`:
```java
package com.p2p.payment.entity;

import jakarta.persistence.*;
import lombok.*;

import java.time.LocalDateTime;

@Entity
@Table(name = "users")
@Getter
@Setter
@NoArgsConstructor
@AllArgsConstructor
@Builder
public class User {
    
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    
    @Column(nullable = false, unique = true, length = 50)
    private String email;
    
    @Column(name = "phone_number", nullable = false, unique = true, length = 20)
    private String phoneNumber;
    
    @Column(name = "first_name", nullable = false, length = 100)
    private String firstName;
    
    @Column(name = "last_name", nullable = false, length = 100)
    private String lastName;
    
    @Enumerated(EnumType.STRING)
    @Column(nullable = false, length = 20)
    private UserStatus status = UserStatus.ACTIVE;
    
    @Column(name = "created_at", nullable = false, updatable = false)
    private LocalDateTime createdAt;
    
    @Column(name = "updated_at", nullable = false)
    private LocalDateTime updatedAt;
    
    @PrePersist
    protected void onCreate() {
        createdAt = LocalDateTime.now();
        updatedAt = LocalDateTime.now();
    }
    
    @PreUpdate
    protected void onUpdate() {
        updatedAt = LocalDateTime.now();
    }
}
```
Создай enum src/main/java/com/p2p/payment/entity/UserStatus.java:

```java
package com.p2p.payment.entity;

public enum UserStatus {
    ACTIVE,
    SUSPENDED,
    BLOCKED,
    DELETED
}
```

### Шаг 6.9: Конфигурация Liquibase в application-dev.yml
Проверь что в application-dev.yml есть настройки:

```yaml

spring:
  liquibase:
    enabled: true
    change-log: classpath:db/changelog/db.changelog-master.yaml
    default-schema: public
    drop-first: false  # ВАЖНО! Не удалять схему при старте
```

**Параметры:**

- `enabled: true` — включить Liquibase
- `change-log` — путь к master файлу
- `default-schema` — схема БД (по умолчанию `public`)
- `drop-first: false` — НЕ удалять схему перед миграциями (для dev можно `true` для чистого старта)

### Шаг 6.10: Запуск миграции

```bash
./gradlew bootRun
```

Ожидаемый вывод:
```
2024-01-15 11:00:15 - Liquibase: Reading from databasechangelog
2024-01-15 11:00:15 - Liquibase: db/changelog/changes/v1/001-create-users-table.yaml::001-create-users-table::your_name ran successfully in 45ms
2024-01-15 11:00:15 - ✅ PostgreSQL connection successful! Database: p2p_payment_db
```

### Шаг 6.11: Проверка в DBeaver
Подключись к БД и проверь:

```sql

-- Таблица users должна существовать
SELECT * FROM users;

-- Таблица Liquibase с историей миграций
SELECT * FROM databasechangelog;

-- Структура таблицы users
SELECT 
    column_name,
    data_type,
    is_nullable,
    column_default
FROM information_schema.columns
WHERE table_name = 'users'
ORDER BY ordinal_position;
```

Таблица databasechangelog:

| id | author | filename | dateexecuted | orderexecuted | md5sum |
|----|--------|----------|--------------|---------------|--------|
| 001-create-users-table | your_name | db/changelog/changes/v1/001-... | 2024-01-15 11:00:15 | 1 | 8:d41d8cd98f00b204e980... |

**Поля:**

- `id + author + filename` — уникальный идентификатор миграции
- `dateexecuted` — когда была применена
- `orderexecuted` — порядок выполнения
- `md5sum` — checksum файла (для проверки изменений)

### Шаг 6.12: Добавление тестовых данных (опционально)
Создай файл `src/main/resources/db/changelog/changes/v1/002-insert-test-users.yaml`:

```yaml
databaseChangeLog:
  - changeSet:
      id: 002-insert-test-users
      author: your_name
      context: dev  # Выполнится только в dev окружении
      changes:
        - insert:
            tableName: users
            columns:
              - column:
                  name: email
                  value: john.doe@example.com
              - column:
                  name: phone_number
                  value: "+1234567890"
              - column:
                  name: first_name
                  value: John
              - column:
                  name: last_name
                  value: Doe
              - column:
                  name: status
                  value: ACTIVE

        - insert:
            tableName: users
            columns:
              - column:
                  name: email
                  value: jane.smith@example.com
              - column:
                  name: phone_number
                  value: "+1234567891"
              - column:
                  name: first_name
                  value: Jane
              - column:
                  name: last_name
                  value: Smith
              - column:
                  name: status
                  value: ACTIVE

      rollback:
        - delete:
            tableName: users
            where: email IN ('john.doe@example.com', 'jane.smith@example.com')
```

Добавь в db.changelog-master.yaml:

```yaml

databaseChangeLog:
  - include:
      file: db/changelog/changes/v1/001-create-users-table.yaml
  - include:
      file: db/changelog/changes/v1/002-insert-test-users.yaml
      context: dev  # Только для dev окружения
```

Настройка контекста в application-dev.yml:
```yaml


spring:
  liquibase:
    enabled: true
    change-log: classpath:db/changelog/db.changelog-master.yaml
    default-schema: public
    contexts: dev  # Выполнять миграции с context: dev
```

Перезапусти приложение:
```bash
./gradlew bootRun
```

Проверь в DBeaver:

```sql
SELECT * FROM users;
```

Должно быть 2 тестовых пользователя.

✅ **Критерий выполнения:**
- Liquibase применил миграцию `001-create-users-table`
- Таблица `users` создана в БД
- В таблице `databasechangelog` есть запись о миграции
- (Опционально) Тестовые пользователи вставлены

## 🏥 Задание 7: Настройка Spring Boot Actuator

### Шаг 7.1: Теория — Что такое Actuator?

Actuator — набор готовых endpoints для мониторинга приложения:

- `/actuator/health` — статус приложения (UP/DOWN)
- `/actuator/info` — информация о версии, сборке
- `/actuator/metrics` — метрики (CPU, память, запросы)
- `/actuator/env` — переменные окружения
- `/actuator/loggers` — управление уровнями логирования
- `/actuator/threaddump` — дамп потоков JVM

**Зачем нужен?**

- **Мониторинг:** Prometheus/Grafana собирают метрики из `/actuator/prometheus`
- **Health Checks:** Kubernetes/Docker проверяют `/actuator/health`
- **Debugging:** Изменить уровень логирования без перезапуска
- **Alerting:** Если `/actuator/health` возвращает DOWN — отправить уведомление

### Шаг 7.2: Конфигурация Actuator
Обнови `application-dev.yml`:

```yaml


management:
  endpoints:
    web:
      exposure:
        include: health,info,metrics,env,loggers,prometheus
      base-path: /actuator
  endpoint:
    health:
      show-details: always  # Показывать детали (db, redis статус)
      show-components: always
  health:
    redis:
      enabled: true
    db:
      enabled: true
  info:
    env:
      enabled: true
    java:
      enabled: true
    os:
      enabled: true

info:
  app:
    name: "@project.name@"
    version: "@project.version@"
    description: "P2P Payment Service"
    profile: "@spring.profiles.active@"
```

**Объяснение:**

- `exposure.include` — какие endpoints открыть (по умолчанию только `health` и `info`)
- `show-details: always` — показывать детали для health checks
  - `never` — только статус UP/DOWN
  - `when-authorized` — только для authenticated пользователей
  - `always` — всегда (для dev окружения)
- `info.app` — кастомная информация о приложении
  - `@project.name@` — Maven/Gradle property (из `build.gradle`)

  ### Шаг 7.3: Добавление метаданных в build.gradle
Обнови build.gradle (добавь после dependencies):

```gradle

bootJar {
    archiveBaseName = 'payment-service'
    archiveVersion = project.version
}

springBoot {
    buildInfo()
}
```

### Что делает `buildInfo()`?

Генерирует файл `META-INF/build-info.properties` с информацией о сборке:

- Имя проекта  
- Версия  
- Время сборки  
- Gradle версия  

Эта информация попадет в `/actuator/info`.

### Шаг 7.4: Создание кастомного Health Indicator
Создай класс `src/main/java/com/p2p/payment/health/CustomHealthIndicator.java`:

```java


package com.p2p.payment.health;

import lombok.RequiredArgsConstructor;
import org.springframework.boot.actuate.health.Health;
import org.springframework.boot.actuate.health.HealthIndicator;
import org.springframework.data.redis.core.RedisTemplate;
import org.springframework.jdbc.core.JdbcTemplate;
import org.springframework.stereotype.Component;

import java.util.HashMap;
import java.util.Map;

@Component
@RequiredArgsConstructor
public class CustomHealthIndicator implements HealthIndicator {

    private final JdbcTemplate jdbcTemplate;
    private final RedisTemplate<String, Object> redisTemplate;

    @Override
    public Health health() {
        Map<String, Object> details = new HashMap<>();
        
        // Проверка PostgreSQL
        boolean postgresUp = checkPostgreSQL(details);
        
        // Проверка Redis
        boolean redisUp = checkRedis(details);
        
        // Если хотя бы один DOWN — весь статус DOWN
        if (postgresUp && redisUp) {
            return Health.up().withDetails(details).build();
        } else {
            return Health.down().withDetails(details).build();
        }
    }

    private boolean checkPostgreSQL(Map<String, Object> details) {
        try {
            String database = jdbcTemplate.queryForObject(
                "SELECT current_database()", 
                String.class
            );
            
            Integer connectionCount = jdbcTemplate.queryForObject(
                "SELECT count(*) FROM pg_stat_activity WHERE datname = current_database()",
                Integer.class
            );
            
            details.put("postgresql.status", "UP");
            details.put("postgresql.database", database);
            details.put("postgresql.connections", connectionCount);
            return true;
        } catch (Exception e) {
            details.put("postgresql.status", "DOWN");
            details.put("postgresql.error", e.getMessage());
            return false;
        }
    }

    private boolean checkRedis(Map<String, Object> details) {
        try {
            String testKey = "health:ping";
            redisTemplate.opsForValue().set(testKey, "pong");
            String response = (String) redisTemplate.opsForValue().get(testKey);
            redisTemplate.delete(testKey);
            
            if ("pong".equals(response)) {
                details.put("redis.status", "UP");
                details.put("redis.response_time_ms", "<1");
                return true;
            } else {
                details.put("redis.status", "DOWN");
                details.put("redis.error", "Unexpected response: " + response);
                return false;
            }
        } catch (Exception e) {
            details.put("redis.status", "DOWN");
            details.put("redis.error", e.getMessage());
            return false;
        }
    }
}
```

**Что делает этот класс?**

- Реализует интерфейс `HealthIndicator`
- Spring Boot автоматически подключает его к `/actuator/health`
- Проверяет PostgreSQL (текущая БД, количество соединений)
- Проверяет Redis (ping-pong тест)
- Возвращает `Health.up()` если все OK, иначе `Health.down()`

### Шаг 7.5: Запуск и проверка
```bash
./gradlew bootRun
```

Откройте в браузере или используйте curl:

#### 1. Health Check (базовый)
```bash
curl http://localhost:8080/actuator/health | jq
```

Ожидаемый ответ:

```json
{
  "status": "UP",
  "components": {
    "custom": {
      "status": "UP",
      "details": {
        "postgresql.status": "UP",
        "postgresql.database": "p2p_payment_db",
        "postgresql.connections": 5,
        "redis.status": "UP",
        "redis.response_time_ms": "<1"
      }
    },
    "db": {
      "status": "UP",
      "details": {
        "database": "PostgreSQL",
        "validationQuery": "isValid()"
      }
    },
    "diskSpace": {
      "status": "UP",
      "details": {
        "total": 500000000000,
        "free": 250000000000,
        "threshold": 10485760,
        "path": "/Users/username/.",
        "exists": true
      }
    },
    "ping": {
      "status": "UP"
    },
    "redis": {
      "status": "UP",
      "details": {
        "version": "7.2.4"
      }
    }
  }
}
```

**Статусы:**

- `UP` — все компоненты работают  
- `DOWN` — хотя бы один компонент недоступен  
- `OUT_OF_SERVICE` — сервис намеренно выключен  
- `UNKNOWN` — статус неизвестен  

### 2. Info Endpoint

```bash

curl http://localhost:8080/actuator/info | jq
```

Ожидаемый ответ:

```json

{
  "app": {
    "name": "payment-service",
    "version": "0.0.1-SNAPSHOT",
    "description": "P2P Payment Service",
    "profile": "dev"
  },
  "build": {
    "artifact": "payment-service",
    "name": "payment-service",
    "time": "2024-01-15T08:30:15.123Z",
    "version": "0.0.1-SNAPSHOT",
    "group": "com.p2p.payment"
  },
  "java": {
    "version": "21.0.1",
    "vendor": {
      "name": "Oracle Corporation",
      "version": "Oracle Corporation"
    },
    "runtime": {
      "name": "Java(TM) SE Runtime Environment",
      "version": "21.0.1+12-LTS-29"
    },
    "jvm": {
      "name": "Java HotSpot(TM) 64-Bit Server VM",
      "vendor": "Oracle Corporation",
      "version": "21.0.1+12-LTS-29"
    }
  },
  "os": {
    "name": "Mac OS X",
    "version": "14.2.1",
    "arch": "aarch64"
  }
}
```

### 3. Metrics Endpoint

```bash

curl http://localhost:8080/actuator/metrics | jq
```

Ответ (список доступных метрик):
```json


{
  "names": [
    "application.ready.time",
    "application.started.time",
    "disk.free",
    "disk.total",
    "hikaricp.connections",
    "hikaricp.connections.active",
    "hikaricp.connections.idle",
    "hikaricp.connections.max",
    "hikaricp.connections.min",
    "http.server.requests",
    "jdbc.connections.active",
    "jdbc.connections.idle",
    "jdbc.connections.max",
    "jdbc.connections.min",
    "jvm.buffer.count",
    "jvm.buffer.memory.used",
    "jvm.buffer.total.capacity",
    "jvm.gc.live.data.size",
    "jvm.gc.max.data.size",
    "jvm.gc.memory.allocated",
    "jvm.gc.memory.promoted",
    "jvm.gc.pause",
    "jvm.memory.committed",
    "jvm.memory.max",
    "jvm.memory.used",
    "jvm.threads.daemon",
    "jvm.threads.live",
    "jvm.threads.peak",
    "jvm.threads.states",
    "logback.events",
    "process.cpu.usage",
    "process.start.time",
    "process.uptime",
    "system.cpu.count",
    "system.cpu.usage",
    "tomcat.sessions.active.current",
    "tomcat.sessions.active.max",
    "tomcat.sessions.alive.max",
    "tomcat.sessions.created",
    "tomcat.sessions.expired",
    "tomcat.sessions.rejected"
  ]
}
```

Просмотр конкретной метрики:

```bash

curl http://localhost:8080/actuator/metrics/jvm.memory.used | jq
```

Ответ:

```json


{
  "name": "jvm.memory.used",
  "description": "The amount of used memory",
  "baseUnit": "bytes",
  "measurements": [
    {
      "statistic": "VALUE",
      "value": 157286400
    }
  ],
  "availableTags": [
    {
      "tag": "area",
      "values": ["heap", "nonheap"]
    },
    {
      "tag": "id",
      "values": ["G1 Eden Space", "G1 Old Gen", "G1 Survivor Space"]
    }
  ]
}

```

Метрики HikariCP (Connection Pool):

```bash

curl http://localhost:8080/actuator/metrics/hikaricp.connections.active | jq
```

```json


{
  "name": "hikaricp.connections.active",
  "measurements": [
    {
      "statistic": "VALUE",
      "value": 2
    }
  ]
}
```

### 4. Loggers Endpoint (изменение уровня логирования)
Просмотр текущих уровней:

```bash
curl http://localhost:8080/actuator/loggers | jq '.loggers | to_entries | .[] | select(.key | startswith("com.p2p"))'
```

Изменение уровня на лету (без перезапуска):

```bash
curl -X POST http://localhost:8080/actuator/loggers/com.p2p.payment \
  -H "Content-Type: application/json" \
  -d '{"configuredLevel": "DEBUG"}'
```

Проверка:

```bash

curl http://localhost:8080/actuator/loggers/com.p2p.payment | jq
```

```json

{
  "configuredLevel": "DEBUG",
  "effectiveLevel": "DEBUG"
}
```

### Шаг 7.6: Настройка безопасности Actuator (для production)
Для production окружения обязательно защитить Actuator endpoints!

Создай application-prod.yml:

```yaml


management:
  endpoints:
    web:
      exposure:
        include: health,info,prometheus  # Только публичные endpoints
      base-path: /actuator
  endpoint:
    health:
      show-details: when-authorized  # Детали только для авторизованных
  health:
    redis:
      enabled: true
    db:
      enabled: true

# Spring Security конфигурация (добавится в модуле Security)
spring:
  security:
    user:
      name: admin
      password: ${ACTUATOR_PASSWORD:changeme}
```

**Best Practices:**

- Ограничить доступ — только для мониторинг систем (Prometheus) и админов
- Использовать отдельный порт — `management.server.port=9090` (не 8080)
- Firewall — разрешить доступ только с IP мониторинг серверов
- HTTPS — всегда использовать TLS для production

### Шаг 7.7: Проверка с остановкой Redis (симуляция сбоя)
Остановить Redis контейнер:
```bash
docker compose stop p2p-redis
```
Проверить health:

```bash
curl http://localhost:8080/actuator/health | jq
```

Ожидаемый ответ:
```json

{
  "status": "DOWN",
  "components": {
    "custom": {
      "status": "DOWN",
      "details": {
        "postgresql.status": "UP",
        "postgresql.database": "p2p_payment_db",
        "postgresql.connections": 3,
        "redis.status": "DOWN",
        "redis.error": "Unable to connect to Redis"
      }
    },
        "redis": {
      "status": "DOWN",
      "details": {
        "error": "org.springframework.data.redis.RedisConnectionFailureException: Unable to connect to Redis"
      }
    }
  }
}
```

Запустить Redis обратно:
```bash

docker compose start p2p-redis
```

Проверить восстановление:

```bash
curl http://localhost:8080/actuator/health | jq '.status'
```

Ожидаемый ответ:
```json

"UP"
```


### Шаг 7.8: Интеграция с Prometheus (опционально, для будущего)
Добавь в build.gradle:

```gradle


dependencies {
    // ... existing dependencies
    
    // Prometheus метрики
    implementation 'io.micrometer:micrometer-registry-prometheus'
}
```

Обнови application-dev.yml:
```yaml


management:
  endpoints:
    web:
      exposure:
        include: health,info,metrics,env,loggers,prometheus
  metrics:
    export:
      prometheus:
        enabled: true
    tags:
      application: ${spring.application.name}
      profile: ${spring.profiles.active}
```

Проверить Prometheus endpoint:
```bash

curl http://localhost:8080/actuator/prometheus
```

Ожидаемый ответ (формат Prometheus):
```
# HELP jvm_memory_used_bytes The amount of used memory
# TYPE jvm_memory_used_bytes gauge
jvm_memory_used_bytes{application="payment-service",area="heap",id="G1 Eden Space",profile="dev",} 1.57286400E8
jvm_memory_used_bytes{application="payment-service",area="heap",id="G1 Old Gen",profile="dev",} 5.242880E7
# HELP hikaricp_connections_active Active connections
# TYPE hikaricp_connections_active gauge
hikaricp_connections_active{application="payment-service",pool="HikariPool-1",profile="dev",} 2.0
# HELP http_server_requests_seconds Duration of HTTP server requests
# TYPE http_server_requests_seconds summary
http_server_requests_seconds_count{application="payment-service",method="GET",outcome="SUCCESS",status="200",uri="/actuator/health",profile="dev",} 15.0
http_server_requests_seconds_sum{application="payment-service",method="GET",outcome="SUCCESS",status="200",uri="/actuator/health",profile="dev",} 0.245
```

Этот endpoint можно подключить к Prometheus/Grafana для визуализации метрик.

✅ **Критерий выполнения Задания 7:**
- `/actuator/health` возвращает `UP` при работающих PostgreSQL и Redis
- `/actuator/health` возвращает `DOWN` при остановке Redis
- `/actuator/info` показывает версию приложения и Java
- `/actuator/metrics` показывает метрики JVM и HikariCP
- Кастомный `CustomHealthIndicator` добавляет детали о PostgreSQL и Redis
- (Опционально) `/actuator/prometheus` возвращает метрики в формате Prometheus

## 📂 Структура проекта

```
payment-service/
├── src/
│   ├── main/
│   │   ├── java/com/p2p/payment/
│   │   │   ├── config/           # Конфигурации Spring
│   │   │   ├── controller/       # REST контроллеры
│   │   │   ├── dto/              # Data Transfer Objects
│   │   │   ├── entity/           # JPA сущности
│   │   │   ├── repository/       # Spring Data репозитории
│   │   │   ├── service/          # Бизнес логика
│   │   │   ├── mapper/           # MapStruct маппинг
│   │   │   ├── exception/        # Обработка ошибок
│   │   │   ├── health/           # Кастомные Health Indicators
│   │   │   └── PaymentServiceApplication.java
│   │   └── resources/
│   │       ├── db/changelog/     # Liquibase миграции
│   │       ├── application.yml   # Основная конфигурация
│   │       └── application-dev.yml # Dev профиль
│   └── test/                     # Тесты
├── docker-compose.yml            # Docker инфраструктура
├── build.gradle                  # Gradle конфигурация
├── settings.gradle
├── gradlew                       # Gradle Wrapper (Unix)
├── gradlew.bat                   # Gradle Wrapper (Windows)
└── README.md                     # Документация
```





