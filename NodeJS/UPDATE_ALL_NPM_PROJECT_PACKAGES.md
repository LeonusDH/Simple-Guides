# Обновляем все **npm** пакеты проекта
 
### Требования:

- Прямые руки
- Умение гуглить

# Установим **npm-check-updates**

```bash
npm install -g npm-check-updates
```

## Обновляем пакеты в проекте

Обновляем `package.json`:

```bash
ncu --upgrade
```

Устанавливаем новые версии пакетов:

```bash
npm install
```

## Обновляем глобальные пакеты

```bash
ncu -g -u
```