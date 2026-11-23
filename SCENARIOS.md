# Типичные сценарии и решения

Решения для распространённых ситуаций при работе с Git.

## Я забыл добавить файл в последний коммит

```bash
git add forgotten_file.txt
git commit --amend --no-edit
```

`--amend` добавляет изменения в последний коммит, `--no-edit` сохраняет старое сообщение.

## Мне нужно отменить опубликованный коммит

```bash
git revert <commit-hash>
```

`revert` создаёт новый коммит, который отменяет изменения. Это безопаснее, чем `reset`, так как не переписывает историю.

## Как изменить сообщение последнего коммита

```bash
git commit --amend -m "Новое сообщение"
```

## Я случайно коммитил в неправильную ветку

```bash
# Сохранить коммит в новую ветку
git branch new-branch
git reset --hard HEAD~1
git checkout new-branch
```

## Как перенести коммиты с одной ветки на другую

```bash
git cherry-pick <commit-hash>
```

## Мне нужно объединить несколько коммитов в один

```bash
git rebase -i HEAD~3
```

Откроется редактор, где вы можете выбрать, какие коммиты объединить (squash).

## Я случайно удалил ветку, как её восстановить

```bash
git reflog
git checkout -b recovered-branch <commit-hash>
```

`reflog` показывает историю всех операций.

## Как восстановить удалённый файл

```bash
git log -- deleted_file.txt
git show <commit-hash>:deleted_file.txt > deleted_file.txt
```

## Работа с конфликтами слияния

Когда вы выполняете `git merge` и возникает конфликт:

```bash
# 1. Откройте файлы с конфликтом и отредактируйте их
# 2. Удалите маркеры конфликта: <<<<<<<, =======, >>>>>>>

# 3. Добавьте разрешённые файлы
git add .

# 4. Завершите слияние
git commit -m "Resolve merge conflict"
```

## Как просмотреть все изменения перед пушем

```bash
git diff main..HEAD
```

## Я запушил неправильный коммит в main

```bash
# 1. Отменить коммит локально
git reset --soft HEAD~1

# 2. Переписать историю на сервере (БУДЬТЕ ОСТОРОЖНЫ!)
git push origin main --force-with-lease
```

⚠️ Используйте `--force-with-lease` вместо `--force` — это безопаснее в команде.

## Как найти коммит, где была введена ошибка

```bash
git bisect start
git bisect bad HEAD
git bisect good v1.0.0
# Git автоматически переключится на коммит посередине
# Проверьте, есть ли ошибка
git bisect good  # или git bisect bad
# Повторяйте, пока не найдёте виноватый коммит
git bisect reset
```

## Как синхронизировать мою ветку с main

```bash
git fetch origin
git rebase origin/main
```

или (если работаете в команде):

```bash
git merge origin/main
```

## Я случайно коммитил пароль или API ключ

```bash
# Удалить файл из истории (тяжёлая операция)
git filter-branch --tree-filter 'rm -f secret_file.txt' HEAD

# или использовать BFG Repo-Cleaner
bfg --delete-files secret_file.txt

# Обновить пароль/ключ немедленно!
# Запушить в удалённый репозиторий
git push origin main --force-with-lease
```

## Как просмотреть, кто изменял конкретную строку

```bash
git blame file.txt
```

Также можно указать диапазон строк:

```bash
git blame -L 10,20 file.txt
```

## Как откатить все локальные изменения

```bash
# Откатить все файлы в текущей директории
git checkout .

# Откатить конкретный файл
git checkout file.txt

# Удалить все неслежённые файлы
git clean -fd
```

## Быстрый switch между ветками

```bash
git checkout -
```

Переключится на предыдущую ветку (как `cd -` в bash).

---

**Помните**: перед выполнением опасных операций (force push, filter-branch) убедитесь, что вы знаете, что делаете!