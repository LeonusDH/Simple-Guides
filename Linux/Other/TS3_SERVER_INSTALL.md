# Установка сервера **TeamSpeak 3** на **Linux (Debian/Ubuntu)**

> [!NOTE]  
> Обычно, сервер **TeamSpeak** ставят в директорию `/opt`, но я поставлю в `/home`.  
> В сущности, это ни на что не влияет. Вы можете установить и в `/opt`, если хотите.

### Требования:

- Прямые руки
- Умение гуглить

# Установка

## Скачаем необходимый софт

> В некоторых дистрибутивах, они могут быть установлены по умолчанию, но выполнить это не будет лишним.

```bash
sudo apt-get install -y wget bzip2 nano
```

## Создадим юзера для сервера

```bash
sudo adduser --disabled-login teamspeak
```

### Заходим

```bash
sudo su teamspeak
```

## Скачиваем сервер

> Выполняем от пользователя `teamspeak`

> `3.13.7` - версия сервера (лучше всегда использовать актуальную)

> Прямую ссылку можно взять с официального сайта https://www.teamspeak.com/en/downloads/#server

```bash
wget https://files.teamspeak-services.com/releases/server/3.13.7/teamspeak3-server_linux_amd64-3.13.7.tar.bz2
```

### Распакуем архив

```bash
tar xvfj teamspeak3-server_linux_amd64-*.tar.bz2
```

### Копируем распакованное в папку пользователя

```bash
cp teamspeak3-server_linux_amd64/* -R /home/teamspeak/
```

### Удалим скачанный архив

```bash
rm teamspeak3-server_linux_amd64-*.tar.bz2
```

### Принимаем лицензионное соглашение

```bash
touch .ts3server_license_accepted
```

### Выходим из пользователя

```bash
exit
```

## Настроим автозапуск

### Создадим файл

```bash
sudo nano /lib/systemd/system/ts3server.service
```

### Вставим туда

```service
[Unit]
Description=TeamSpeak 3 Service
Wants=network.target

[Service]
WorkingDirectory=/home/teamspeak
User=teamspeak
ExecStart=/home/teamspeak/ts3server_minimal_runscript.sh
ExecStop=/home/teamspeak/ts3server_startscript.sh stop
ExecReload=/home/teamspeak/ts3server_startscript.sh restart
Restart=always
RestartSec=15

[Install]
WantedBy=multi-user.target
```

> Нажимаем `Ctrl+s` - чтобы сохранить.

> Нажимаем `Ctrl+x` - чтобы выйти.

### Перезапустим **systemd**

```bash
sudo systemctl daemon-reload
```

### Запустим сервис **ts3server**

```bash
sudo systemctl enable --now ts3server
```

### Получим токен

```bash
sudo journalctl -xe -u ts3server | grep token
```

### Получаем данные от пользователя **serveradmin**

```bash
sudo journalctl -xe -u ts3server | grep password
```

# Настройка фаервола

## **UFW**

> Подойдёт, если вы используете **UFW** как **Frontend** для **iptables/nftables**, или только **UFW**

> Голосовой сервер (обязателен)

```bash
sudo ufw allow 9987/udp
```

> Передача файлов (обязателен)

```bash
sudo ufw allow 30033/tcp
```

> ServerQuery (raw) (опционален)

```bash
sudo ufw allow 10011/tcp
```

> ServerQuery (SSH) (опционален)

```bash
sudo ufw allow 10022/tcp
```

> WebQuery (http) (опционален)

```bash
sudo ufw allow 10080/tcp
```

> WebQuery (https) (опционален)

```bash
sudo ufw allow 10443/tcp
```

> TSDNS (опционален)

```bash
sudo ufw allow 41144/tcp
```

# **Fail2ban**

## Создаём фильтр

```bash
sudo nano /etc/fail2ban/filter.d/teamspeak.conf
```

### Вставим туда

```conf
// populate it with this content
[INCLUDES]
before = common.conf
[Definition]
failregex = ^(.*)query from [0-9]{1,} :[0-9]{1,5} attempted to login with account "(.*)" and failed!$
ignoreregex =
```

## Создадим клетку

```bash
sudo nano /etc/fail2ban/jail.d/teamspeak.local
```

### Вставим туда

```conf
[teamspeak]
enabled  = true
port     = 9987,10011,30033
filter   = teamspeak
logpath  = /home/teamspeak/logs/ts3server_*.log
maxretry = 3
bantime  = 86400
findtime = 7800
```