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

> `password` - пароль нового пользователя

```sql
CREATE USER 'username'@'127.0.0.1' IDENTIFIED BY 'password';
```

### Выдаём привилегии пользователю

> `username` - имя нового пользователя

> `*.*` - Все права на все базы данных (формат: база_данных.право)

```sql
GRANT ALL PRIVILEGES ON *.* TO 'username'@'127.0.0.1' WITH GRANT OPTION;
```

### Обновляем и применяем изменения в привилегиях, чтобы не перезагружать сервер базы данных

```sql
FLUSH PRIVILEGES;
```

### Выходим из  **MySQL**/**MariaDB**

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