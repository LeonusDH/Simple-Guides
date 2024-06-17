# Безопасное удаление всех прошлых коммитов GIT

> Команды одинаково работают как на **Linux**, так и на **Windows**!

Вводим по очереди:

```bash
git checkout --orphan latest_branch
```

```bash
git add -A
```

```bash
git commit -am "Название коммита"
```

```bash
git branch -D main
```

```bash
git branch -m main
```

```bash
git push -f origin main
```
