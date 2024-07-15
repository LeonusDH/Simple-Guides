# Установка phpMyAdmin на Ubuntu с Nginx

### Требования:

- Прямые руки
- Умение гуглить
- Установленный сервер базы данных **MariaDB** (или **MySQL**)
- Установленный веб сервер **Nginx**

# Установка PHP

> Надеюсь, **Nginx** и **MariaDB** у вас уже установлены...

### **PHP** и модули

Я предпочитаю использовать последнюю версию, но пусть будет **PHP 8.1**

```bash
sudo apt-get install -y php8.1 php8.1-{common,cli,gd,mysql,mbstring,bcmath,xml,fpm,curl,zip,pdo}
```

> Не все эти модули обязательны, но лучше поставить на будущее.

> Для корректной работы **PMA** хватит: `php8.1 php8.1-{common,cli,gd,mysql,fpm}`

### Проверим службу **php-fpm** (просто, на всякий случай)

```bash
sudo systemctl status php8.1-fpm
```

# Установка **phpMyAdmin**

> Настройку `dbconfig-common` можно пропустить.

### В принципе, можно просто прописать:

```bash
sudo apt-get install -y phpmyadmin
```

**НО!** Так мы можем установить не самую новую версию **PMA**.

### Установим самую свежую версию **phpMyAdmin**

Добавляем репозиторий

```bash
sudo add-apt-repository ppa:phpmyadmin/ppa
```

Обновляем пакеты

```bash
sudo apt-get update
```

Устанавливаем **phpMyAdmin**

```bash
sudo apt-get install -y phpmyadmin
```

Если хотите, можете уже убрать репозиторий, он на больше не понадобится:

```bash
sudo add-apt-repository --remove ppa:phpmyadmin/ppa
```

### Разрешения для корневого каталога:

Дабы избежать возможных проблем из за прав, лучше прописать:

```bash
sudo chmod 775 -R /usr/share/phpmyadmin/
```

# Настроим Nginx

> `<domain>` - домен (формат: example.com)

### Обычный конфиг

<details>
<summary>Стандарт</summary>

Идём в `/etc/nginx/sites-available/default` и ищем блок:

```nginx
    #location ~ \.php$ {
    #        include snippets/fastcgi-php.conf;
    #
    #       # With php-fpm (or other unix sockets):
    #        fastcgi_pass unix:/run/php/php7.4-fpm.sock;
    #       # With php-cgi (or other tcp sockets):
    #        fastcgi_pass 127.0.0.1:9000;
    #}
```

Убираем комментарии у строчек:

- location ~ \.php$ {
- include snippets/fastcgi-php.conf;
- fastcgi_pass unix:/run/php/php7.4-fpm.sock;
- fastcgi_pass 127.0.0.1:9000;
- }

И заменяем `unix:/run/php/php7.4-fpm.sock;` на `unix:/run/php/php8.1-fpm.sock;` (или другой версии, если у вас **php** не **8.1**)

### А дальше?

Сделаем символическую ссылку **PMA** на наш сайт, чтобы она была доступна по `<domain_or_ip>/phpmyadmin`:

```bash
sudo ln -s /usr/share/phpmyadmin /var/www/html/phpmyadmin
```

</details>

### Конфиг для поддомена

<details>
<summary>Поддомен</summary>

> Конфиг не учитывает наличие SSL сертификата!

Создаём **Nginx** конфиг для нашей **phpMyAdmin**:

```bash
sudo nano /etc/nginx/sites-available/phpmyadmin.conf
```

И вставляем это (не забудьте поменять домен):

```nginx
server {
    listen 80;
    server_name pma.<domain>;

    # Поскольку мы на поддомене, корнем будет папка с самим PMA
    root /usr/share/phpmyadmin;
    index index.php;

    location / {
        try_files $uri $uri/ /index.php?$args;
    }

    location ~ \.php$ {
        include snippets/fastcgi-php.conf;
        fastcgi_pass unix:/run/php/php8.1-fpm.sock;

        # With php-cgi (or other tcp sockets):
        # fastcgi_pass 127.0.0.1:9000;
    }

    location ~ /\.ht {
        deny all;
    }

    location ~* \.(js|css|png|jpg|jpeg|gif|ico)$ {
        expires max;
        log_not_found off;
    }
}
```

</details>

### Проверяем, что никаких ошибок нет:

```bash
sudo nginx -t
```

Если видим это, значит всё хорошо:

```bash
nginx: the configuration file /etc/nginx/nginx.conf syntax is ok
nginx: configuration file /etc/nginx/nginx.conf test is successful
```

> Если вы использовали конфиг для поддомена, нужно добавить символическую ссылку:

```bash
sudo ln -s /etc/nginx/sites-available/phpmyadmin.conf /etc/nginx/sites-enabled/phpmyadmin.conf
```

### Перезапускаем службу **Nginx**

```bash
sudo systemctl restart nginx
```

# Проверяем

Пробуем зайти в **PMA** от вашего пользователя.

# Удалить **phpMyAdmin**??? (нафига ты его тогда ставил(а)???)

### Удалить пакеты, но не конфиги:

```bash
sudo apt-get remove phpmyadmin
```

### Удалить пакеты и конфиги:

```bash
sudo apt-get purge phpmyadmin
```
