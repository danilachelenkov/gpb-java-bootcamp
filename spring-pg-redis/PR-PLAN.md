# PR Plan Project

## 🛠 Техническая организация Code Review для 3 независимых проектов

---

## 📁 Структура репозиториев

### Вариант A: Отдельные репозитории (рекомендуется)

- `github.com/Kharin/redis-spring-project`
- `github.com/Gaskarow/redis-spring-project`
- `github.com/Chelenkov/redis-spring-project`

### Вариант B: Организация на GitHub
```
github.com/your-team/
├── user1-redis-project
├── user2-redis-project
└── user3-redis-project
```
---

## 🔧 Настройка процесса

### 1. Создание репозиториев:

```bash
# Каждый создает свой репозиторий
git init redis-spring-project
git remote add origin https://github.com/yourname/redis-spring-project

###  2. Настройка доступа:
Каждый добавляет двух других как Collaborators в настройках репозитория
Или создаете организацию и добавляете всех участников

### 3. Workflow для ревью:
Developer делает:
```
# Работает в своей ветке
```
git checkout -b feature/redis-integration
```
# Коммитит изменения
```
git add .
git commit -m "Implement Redis caching"
```
# Пушит в свой репозиторий
```
git push origin feature/redis-integration
```

**Создает Pull Request:**
- Заходит на GitHub → Pull Requests → New Pull Request
- Выбирает feature/redis-integration → main
- Назначает двух ревьюверов из команды
- Добавляет описание что сделано

### 🔄 Процесс ревью
Ревьюверы:
- Получают уведомление о PR
- Проверяют код, оставляют комментарии
- Используют Review Changes → Request changes или Approve

После ревью:
- Developer вносит правки (коммиты в ту же ветку)
- После аппрува от обоих ревьюверов - мержит PR

## ⚙️ Автоматизация и стандарты
#### 1. Единый кодстайл:

<span style="color:yellow">
У меня настроен Google CodeStyle. Тут стоит подумать, может сменить на другой.
Устанавливал google-java-format plugin
</span>



```
<!-- В каждом проекте одинаковый checkstyle.xml -->
<plugin>
    <groupId>org.apache.maven.plugins</groupId>
    <artifactId>maven-checkstyle-plugin</artifactId>
</plugin>
```

#### 2. GitHub Actions для автоматических проверок:
```
# .github/workflows/review.yml
name: Code Review Checks
on: [pull_request]
jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - name: Run tests
        run: mvn test
```

### 🎯 Ротация ревьюверов
- Два ревьюера - ревьюют одного

### 💡 Практические шаги
1. Создайте организацию на GitHub (например redis-learning-team)
2. Каждый создает репозиторий в этой организации
3. Настройте branch protection rules в каждом репозитории:
4. Require pull request before merging
5. Require approvals (2)
6. Include administrators
6. Создайте шаблон PR с чек-листом:
