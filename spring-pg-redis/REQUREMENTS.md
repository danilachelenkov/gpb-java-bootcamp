# 🛠️ Требования к ПО для курса Java Spring Boot

## 📋 Обязательное ПО

### 1. Java Development Kit (JDK) 21
- **Описание:** Основной инструмент разработки на Java
- **Рекомендуемые дистрибутивы:**
  - [Amazon Corretto 21](https://aws.amazon.com/corretto/)
  - [Oracle JDK 21](https://www.oracle.com/java/technologies/downloads/#java21)
  - [OpenJDK 21](https://jdk.java.net/21/)

**Проверка установки:**
```bash
java -version
# Ожидаемый вывод: java version 21.x.x
```

### 2. IntelliJ IDEA
- **Версия:**  Ultimate
- **Скачать:** [Официальная ссылка на IDEA](jetbrains.com/idea)

#### Необходимые плагины (устанавливаются внутри IDEA):

- **Lombok**
- **Docker**
- **Spring Boot Assistant (опционально)**

```bash
Примечание: IntelliJ IDEA включает встроенную поддержку Gradle — отдельная установка НЕ требуется.
```
### 3. Docker Desktop
- **Описание:** Для запуска PostgreSQL и Redis в контейнерах
- **Скачать:**  [Сам Docker](docker.com/products/docker-desktop)

Проверка установки:

```
bash

docker --version
docker-compose --version
```

**Минимальные требования:**

- Windows: Windows 10/11 Pro, Enterprise, Education (с WSL 2)
- macOS: 10.15 или новее
- Linux: Поддерживаемый дистрибутив

### 4. Git
-  **Описание:** Система контроля версий
- **Скачать:** [ссыль на Git](git-scm.com/downloads)

Проверка установки:
```
bash

git --version
```
Базовая настройка:

```
bash

git config --global user.name "Ваше Имя"
git config --global user.email "your.email@example.com"
```

### 5. Postman
- **Описание:** Инструмент для тестирования REST API
- **Скачать:** [ссыль на Postman](postman.com/downloads)

Альтернативы:

- Insomnia
- IntelliJ IDEA HTTP Client (встроен)
- curl (командная строка)

**🔧 Рекомендуемое ПО**

### 6. DBeaver или pgAdmin
**Описание:** GUI для работы с PostgreSQL
- **DBeaver Community:**  [Универсальный клиент БД](dbeaver.io/download)
- **pgAdmin:** [специализированный для PostgreSQL](pgadmin.org/download)

### 7. Redis Insight (опционально)
- **Описание:** GUI для просмотра и управления данными в Redis
- **Скачать:** [ссыль](redis.com/redis-enterprise/redis-insight)

Альтернатива: redis-cli (встроен в Docker контейнер Redis)

```
bash

docker exec -it redis-container redis-cli
```

### 8. Gradle
- **Описание:** Система сборки проектов (используется вместо Maven)
- **Примечание:** IntelliJ IDEA использует встроенный Gradle Wrapper — отдельная установка НЕ требуется

Проверка Gradle Wrapper в проекте:

```
bash

./gradlew --version    # macOS/Linux
gradlew.bat --version  # Windows
```

**Опциональная установка Gradle глобально:**

- **Скачать:** [ссыль](gradle.org/install)

**Проверка:**

```
bash

gradle --version
```

## ⚙️ Настройка переменных окружения

**JAVA_HOME**

**Windows:**

1. Панель управления → Система → Дополнительные параметры системы
2. Переменные среды → Создать
3. Имя: JAVA_HOME
4. Значение: C:\Program Files\Java\jdk-21 (путь к вашей JDK)

Проверка:
```
cmd

echo %JAVA_HOME%
```

- macOS/Linux:

1.Добавить в ~/.bashrc или ~/.zshrc:

```
bash

export JAVA_HOME=/usr/lib/jvm/java-21-openjdk
export PATH=$JAVA_HOME/bin:$PATH
```

Проверка:
```
bash

echo $JAVA_HOME
```

### ✅ Финальная проверка готовности
Выполните все команды в терминале:


# 1. Java
```
bash

java -version
```
# 2. Docker
```
bash

docker --version
docker ps
```

# 3. Git

```
bash

git --version
```

# 4. Gradle (через Wrapper в проекте)
```
./gradlew --version
```

**Ожидаемый результат: Все команды выполняются без ошибок и показывают версии программ.**



## 📦 Структура Gradle проекта

После создания проекта через Spring Initializr ожидаемая структура:


```
my-project/
├── gradle/
│   └── wrapper/
│       ├── gradle-wrapper.jar
│       └── gradle-wrapper.properties
├── src/
│   ├── main/
│   │   ├── java/
│   │   └── resources/
│   └── test/
│       └── java/
├── build.gradle
├── settings.gradle
├── gradlew          # Gradle Wrapper для macOS/Linux
└── gradlew.bat      # Gradle Wrapper для Windows
```


### 🔨 Основные команды Gradle

bash

# Сборка проекта
```
./gradlew build
```
# Запуск приложения
```
./gradlew bootRun
```
# Запуск тестов
```
./gradlew test
```
# Очистка build директории
```
./gradlew clean
```

# Просмотр зависимостей
```
./gradlew dependencies
```

# Обновление Gradle Wrapper
```
./gradlew wrapper --gradle-version=8.5
```


## ❌ Что НЕ нужно устанавливать
 - ❌ PostgreSQL напрямую на машину (будет в Docker)
 - ❌ Redis напрямую на машину (будет в Docker)
 - ❌ Tomcat или другой сервер приложений (Spring Boot использует встроенный)
 - ❌ Maven (используем Gradle)

## 💻 Минимальные требования к железу

```
Компонент	                Минимум	            Рекомендуется
RAM	                        8 GB	            16 GB
Свободное место на диске	10 GB               20 GB
Процессор	                Dual-core 2.0 GHz	Quad-core 2.5 GHz+
ОС	                        Windows 10	        Windows 11+
```

### 📚 Дополнительные инструменты (для продвинутых)

**Для работы с API:**

- Bruno — open-source альтернатива Postman
- HTTPie — CLI инструмент

**Для мониторинга:**
- Docker Desktop Dashboard — встроен в Docker Desktop
- Portainer — веб-интерфейс для Docker

**Для командной строки:**
- Windows Terminal (Windows)
- iTerm2 (macOS)
Zsh + Oh My Zsh (macOS/Linux)

## 🆘 Помощь при установке

#### Проблемы с Docker на Windows:

Убедитесь, что включен WSL 2:
- powershell
- wsl --install
- wsl --set-default-version 2

В настройках Docker Desktop выберите WSL 2 backend

#### Проблемы с JAVA_HOME:
Проверьте, что путь указывает на корневую папку JDK, а не на bin
Перезапустите терминал/IDE после изменения переменных

Проблемы с правами доступа (macOS/Linux):
```
bash
# Для Docker

sudo usermod -aG docker $USER
# Перелогиньтесь после выполнения

# Для Gradle Wrapper
chmod +x gradlew
```

Проблемы с Gradle:
```
bash

# Если Gradle Wrapper не работает, пересоздайте его
gradle wrapper --gradle-version=8.5 --distribution-type=bin

# Очистите кэш Gradle
rm -rf ~/.gradle/caches/
```

### 🔍 Проверка настройки IntelliJ IDEA для Gradle
1. File → Settings → Build, Execution, Deployment → Build Tools → Gradle
2. Убедитесь, что:
3. Build and run using: Gradle (рекомендуется) или IntelliJ IDEA
4. Run tests using: Gradle (рекомендуется) или IntelliJ IDEA
5. Gradle JVM: Project SDK (Java 21)

## Полезные ссылки
- Spring Initializr — [генератор проектов Spring Boot](https://start.spring.io/)
- Gradle Plugin Portal — [плагины для Gradle](https://plugins.gradle.org/)