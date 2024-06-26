# Создание нового пользователя **MySQL**/**MariaDB**

### Требования:

- Прямые руки
- Умение гуглить
- Установленный сервер **MySQL**/**MariaDB**

# Создание

### Входим от рута

```bash
mysql -u root -p
```

### Создаём пользователя

> `username` - имя нового пользователя

> `127.0.0.1` - адрес хоста пользователя (в данном случае, локальный)

> `password` - пароль нового пользователя

```sql
CREATE USER 'username'@'127.0.0.1' IDENTIFIED BY 'password';
```

### Выдаём привилегии пользователю

> `username` - имя нового пользователя

> `127.0.0.1` - адрес хоста пользователя (в данном случае, локальный)

> `*.*` - Все права на все базы данных (формат: база_данных.право)

```sql
GRANT ALL PRIVILEGES ON *.* TO 'username'@'127.0.0.1' WITH GRANT OPTION;
```

### Обновляем и применяем изменения в привилегиях, чтобы не перезагружать сервер базы данных

```sql
FLUSH PRIVILEGES;
```

### Выходим из **MySQL**/**MariaDB**

```sql
exit
```

# Пробуем войти

> `username` - имя пользователя

> `database_name` - имя базы данных

```bash
mysql -u username database_name -p
```

### Или

> `username` - имя пользователя

> `database_name` - имя базы данных

> `password` - пароль пользователя

```bash
mysql -u username database_name --password="password"
```

# Меняем пароль пользователя

### Входим от рута

> Можно и от пользователя, если у него есть привилегии

```bash
mysql -u root -p
```

### Меняем пароль

> `username` - имя пользователя

> `127.0.0.1` - адрес хоста пользователя (в данном случае, локальный)

> `password` - пароль пользователя

```sql
SET PASSWORD FOR 'username'@'127.0.0.1' = 'password';
```

Есть и другой запрос:

```sql
ALTER USER 'username'@'127.0.0.1' IDENTIFIED BY 'password';
```

### Сбросим кэш привилегий

```sql
FLUSH PRIVILEGES;
```

# Удалить пользователя (нафига его было создавать?)

> `username` - имя пользователя

> `127.0.0.1` - адрес хоста пользователя (в данном случае, локальный)

```sql
DROP USER 'username'@'127.0.0.1';
```

# Выводим пользователей **MySQL**/**MariaDB**

### Все пользователи

> Запрос обращается к колонке `user` в таблице пользователей **MySQL**/**MariaDB**

```sql
SELECT user FROM mysql.user;
```

### Посмотреть активных пользователей

```sql
SELECT SUBSTRING_INDEX(host, ':', 1) AS host_short, GROUP_CONCAT(DISTINCT user) AS users, COUNT(*) AS threads FROM information_schema.processlist GROUP BY host_short ORDER BY COUNT(*), host_short;
```

### Посмотреть активного пользователя

> Ну... может вы забыли через кого зашли...

```sql
SELECT user();
```
