# Импорт базы данных **MySQL**/**MariaDB**

### Требования:

- Прямые руки
- Умение гуглить
- Установленный сервер **MySQL**/**MariaDB**
- Вы же залили дамп базы данных (например: в папку пользователя)
- \*Созданный и настроенный пользователь **MySQL**/**MariaDB**

> `*` - опционально

# Импорт

### Входим от рута

```bash
mysql -u root -p
```

### Создаём новую базу

> `database_name` - имя новой базы данных

```sql
CREATE DATABASE database_name;
```

### Выходим из **mysql**

```sql
exit
```

### Импортируем

> `username` - имя пользователя (можно от имени `root`)

> `database_name` - имя созданной базы данных

> `database-dump.sql` - файл дампа (можно использовать путь, но проще выполнить команду из папки с дампом)

```bash
mysql -u username -p database_name < database-dump.sql
```