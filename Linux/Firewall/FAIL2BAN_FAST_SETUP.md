# Быстрая установка и настройка **Fail2ban**

> Настройка применяется на одного пользователя!

### Требования:

- Прямые руки
- Умение гуглить
- Установленный Firewall **NFTables** (iptables - зло во плоти)

> Крайне желательно, чтобы **NFTables** уже был настроен!

> Если вы используете **NFTables** в качестве бэкенда для **UFW**, всё будет настраиваться так-же.

# Установим **Fail2ban**

### Обновим пакеты

```bash
sudo apt-get update
```

### Установим сам пакет

```bash
sudo apt-get install -y fail2ban
```

# Настройка

### Главный конфиг

Сначала, создадим локальный конфиг, чтобы он не был перезаписан в случае обновления:

```bash
cp /etc/fail2ban/jail.conf /etc/fail2ban/jail.local
```

Внутри ищем `banaction` и `banaction_allports`.
В них вставляем `nftables-multiport` и `nftables-allports` соответственно:

```conf
banaction = nftables-multiport
banaction_allports = nftables-allports
```

Сохраняем.

### Перезагружаем:

```bash
sudo systemctl restart fail2ban
```

### Добавляем в автозапуск:

```bash
sudo systemctl enable fail2ban
```

# Прочие конфиги

> Лучшим вариантом будет создавать отдельные конфиги в `/etc/fail2ban/jail.d`

### Пример для **SSH**

<details>
<summary>Конфиг для SSHD</summary>

> Вместо того, чтобы использовать **fail2ban** для защиты **SSH**, лучше переведите **SSH** на вход по ключу!

> Если злоумышленник знает ваш **IP**, он может послать пакеты с подменёнными заголовками отправителя,
> тем самым лишив вас доступа к серверу

Создадим файл:

```bash
nano /etc/fail2ban/jail.d/sshd.local
```

Вставляем:

```conf
[sshd]
enabled   = true
# Если у вас изменён порт SSH, уберите комментарий и введите свой порт
#port      = 22
filter    = sshd
banaction = nftables
backend   = systemd
maxretry  = 5
findtime  = 1h
bantime   = 1d
ignoreip  = 127.0.0.1/8
```

Сохраняем.

</details>

### Пример для **phpMyAdmin**

<details>
<summary>Конфиг для phpMyAdmin</summary>

Создадим файл:

```bash
nano /etc/fail2ban/jail.d/phpmyadmin.local
```

Вставляем:

```conf
[phpMyAdmin]
enabled   = true
port      = http,https
filter    = phpmyadmin-syslog
# Путь к журналу доступа. В моём случае Nginx
logpath   = /var/log/nginx/pma.access.log
backend   = %(syslog_backend)s
maxretry  = 5
```

Сохраняем.

</details>

### Перезагружаем:

```bash
sudo systemctl restart fail2ban
```

# Посмотреть лог

```bash
nano /var/log/fail2ban.log
```

# Проверим статус

### Посмотрим все активные клетки

```bash
fail2ban-client status
```

### Посмотрим статус конкретной клетки

> `sshd` - название клетки (можно посмотреть прошлой командой)

```bash
fail2ban-client status sshd
```

# Ручной бан

Допустим, вам нужно забанить **IP** для **SSH**

> `sshd` - название клетки

> `192.168.2.1` - **IP**, который мы хотим заблокировать

```bash
fail2ban-client set sshd banip 192.168.2.1
```

# Ручной разбан

Допустим, вам нужно разбанить **IP**, который был заблокирован для **SSH**

> `sshd` - название клетки

> `192.168.2.1` - **IP**, который мы хотим разблокировать

```bash
fail2ban-client set sshd unbanip 192.168.2.1
```
