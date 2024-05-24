# Настройка HardStatus в Screen (нижняя полоска, как в tmux)

> Настройка применяется на одного пользователя!

### Требования:

- Прямые руки
- Умение гуглить
- Установленная утилита `Screen`
- Установленная утилита `nano`

# Настройка

## Настройка вывода имени сессии и времени:

```bash
nano ~/.screenrc
```

## Добавляем/Редактируем:

```conf
hardstatus on
hardstatus alwayslastline
hardstatus string "%{= KW}%{= Kw}[%{..G}%S%{= Kw}|%{..Y}%t%{= Kw}]%{..G}%=%c"
```

### С отображением номера окна, на одной сессии:

```conf
hardstatus on
hardstatus alwayslastline
hardstatus string "%{= KW}%{= Kw}[%{..G}%S:%n%{= Kw}|%{..Y}%t%{= Kw}]%{..G}%=%c"
```
