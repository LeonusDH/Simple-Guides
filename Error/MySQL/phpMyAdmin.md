# Ваши настройки будут сохранены только для текущей сессии. Для постоянного хранения требуется подключение модуля хранения настроек phpMyAdmin.

Скорее всего, во время установки **phpMyAdmin**, у вас не были созданы таблица и пользователь для неё.

> Или вы её случайно удалили... Или сделали настройку `dbconfig-common` пропустив все пункты...

Решается это банально: **создать всё ручками!**

### Входим от рута

```bash
mysql -u root -p
```

### Создаём базу данных

> `phpmyadmin` - стандартное имя для базы данных **phpMyAdmin**

```sql
CREATE DATABASE phpmyadmin;
```

### Создаём пользователя

> `pma` - имя пользователя (имени по умолчанию я не знаю, пусть будет такое)

> `localhost` - адрес хоста (Если **PMA** на том же сервере, что и **MySQL/MariaDB**, то должен быть **localhost**)

> `password` - пароль пользователя (можно придумать самому, но лучше сгенерировать)

```sql
CREATE USER 'pma'@'localhost' IDENTIFIED BY 'password';
```

### Выдаём привилегии пользователю

Выдаём права для работы с базой `phpmyadmin`:

> `phpmyadmin` - стандартное имя для базы данных **phpMyAdmin**

> `pma` - имя пользователя

> `localhost` - адрес хоста

```sql
GRANT ALL PRIVILEGES ON `phpmyadmin`.* TO 'pma'@'localhost';
```

Или все привилегии на все базы:

> `pma` - имя пользователя

> `localhost` - адрес хоста

> `*.*` - Все права на все базы данных

```sql
GRANT ALL PRIVILEGES ON *.* TO 'pma'@'localhost' WITH GRANT OPTION;
```

### Не забудем применить все изменения в привилегиях

```sql
FLUSH PRIVILEGES;
```

### Можем выйти

```sql
exit
```

# Заполним таблицы

> `phpmyadmin` - имя базы данных

> `create_tables.sql` - файл дамп со всеми нужными таблицами. Он идёт в комплекте с **phpMyAdmin**

```bash
mysql -u root -p phpmyadmin < /usr/share/phpmyadmin/sql/create_tables.sql
```

# Настроим **phpMyAdmin**

```bash
dpkg-reconfigure phpmyadmin
```

Вводим все наши данные.

> Вы так же можете сделать это вручную: `nano /etc/dbconfig-common/phpmyadmin.conf`

# Если решение не помогло, можете идти гуглить
