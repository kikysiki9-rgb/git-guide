# Git Best Practices

Рекомендации и best practices для эффективной работы с Git.

## Коммиты

### ✅ Хорошие коммиты

```bash
# Одна логическая единица работы
git commit -m "Add user authentication module"

# Информативное сообщение
git commit -m "Fix bug in payment calculation

- Fixed rounding error in subtotal calculation
- Updated unit tests
- Closes #123"

# Небольшой размер
git add specific_file.js
git commit -m "Update styling for login form"
```

### ❌ Плохие коммиты

```bash
# Слишком общее сообщение
git commit -m "Updates"
git commit -m "Fix"

# Слишком большой коммит (множество разных изменений)
git add .
git commit -m "Various fixes and improvements"

# Коммит в main без review
git push origin main
```

## Правила написания сообщений коммитов

Следуйте **Conventional Commits**:

```
<type>(<scope>): <subject>

<body>

<footer>
```

### Типы коммитов

- `feat` — новая функция
- `fix` — исправление ошибки
- `docs` — изменения документации
- `style` — форматирование, пробелы (не влияет на логику)
- `refactor` — переработка кода (не добавляет фичи, не чинит ошибки)
- `perf` — оптимизация производительности
- `test` — добавление или обновление тестов
- `chore` — обновление зависимостей, конфиг и т.д.

### Примеры

```
feat(auth): add two-factor authentication

Implement TOTP-based 2FA for user accounts.
Users can now enable 2FA in their settings.

Closes #456
```

```
fix(cart): prevent negative quantity input

Validate input before updating cart quantity.
Prevents users from entering negative numbers.
```

```
docs: update API documentation

Add missing endpoints and authentication examples.
```

## Работа с ветками

### Именование веток

Используйте описательные имена:

```bash
# ✅ Хорошо
git checkout -b feat/user-authentication
git checkout -b fix/payment-calculation
git checkout -b docs/api-endpoints

# ❌ Плохо
git checkout -b feature1
git checkout -b bugfix
git checkout -b test
```

### Удаляйте старые ветки

После merge в main удалите ветку:

```bash
git branch -d feature-name
git push origin --delete feature-name
```

### Синхронизируйте с main перед созданием PR

```bash
git fetch origin
git rebase origin/main
```

## Code Review

### Перед созданием Pull Request

- ✅ Убедитесь, что код проходит тесты: `npm test`
- ✅ Выполните линтер: `npm run lint`
- ✅ Ветка актуальна с main: `git rebase origin/main`
- ✅ Коммиты имеют хорошие сообщения
- ✅ Нет конфликтов с main
- ✅ Добавлена документация (если нужна)

### Разумный размер PR

- Старайтесь, чтобы PR содержали 300-400 строк кода
- Если PR слишком большой, разделите на несколько
- Слишком маленькие PR могут быть объединены

### Что включить в описание PR

```markdown
## Description
Краткое описание того, что делает этот PR.

## Changes
- Изменение 1
- Изменение 2
- Изменение 3

## Testing
Как протестировать эти изменения?

## Related Issues
Closes #123
Related to #456
```

## История репозитория

### Чистая история — это хорошо

```bash
# ❌ Плохо: множество коммитов типа "fix typo", "wip"
* fix typo
* wip
* update tests
* fix bug
* add feature

# ✅ Хорошо: логичные, атомарные коммиты
* feat: add user dashboard
* fix: validate email format
* test: add dashboard unit tests
```

### Используйте squash при merge

```bash
# При merge PR в GitHub, выберите "Squash and merge"
# Это объединит все коммиты в один
```

## Безопасность

### Никогда не коммитьте секреты

```bash
# ❌ НИКОГДА
git add .env
git commit -m "Add environment file"

# ✅ Создайте .env.example
# ✅ Добавьте .env в .gitignore
```

### .gitignore обязателен

```bash
# Создайте файл .gitignore в корне проекта
node_modules/
.env
*.log
dist/
build/
.DS_Store
```

### Проверяйте перед пушем

```bash
# Просмотрите свои изменения перед отправкой
git diff

# Проверьте коммиты, которые вы отправляете
git log origin/main..HEAD
```

## Работа в команде

### Коммуникация

- Обсудите большие изменения ДО начала работы
- Используйте issue для обсуждения фич и ошибок
- Оставляйте комментарии в код, если требуется
- Уважайте чужой код при code review

### Синхронизация

```bash
# Регулярно обновляйтесь с основной веткой
git fetch origin
git rebase origin/main

# Не работайте слишком долго в отдельной ветке
# Это может привести к сложным конфликтам
```

### Конфликты — это нормально

Конфликты означают, что разработчики работали в одних и тех же файлах. Это решаемо:

```bash
# 1. Обновитесь с main
git rebase origin/main

# 2. Git уведомит о конфликтах
# 3. Отредактируйте файлы вручную
# 4. Добавьте разрешённые файлы
git add .

# 5. Продолжите rebase
git rebase --continue
```

---

**Помните**: Git — это инструмент для команды. Пишите ясные коммиты, помогайте коллегам и уважайте историю проекта!