# Установим последнюю версию **MariaDB** на **Ubuntu**

### Требования:

- Прямые руки
- Умение гуглить

# Подготовка

### Установим необходимые пакеты

```bash
sudo apt-get install -y curl
```

### Добавим репозиторий

```bash
curl -sS https://downloads.mariadb.com/MariaDB/mariadb_repo_setup | sudo bash
```

### Обновим репозитории

```bash
sudo apt-get update
```

# Установим саму **MariaDB**

```bash
sudo apt-get install -y mariadb-server
```

# Обновим **MariaDB**, если она уже была установлена

### Обновляем

```bash
sudo apt-get install -y --only-upgrade mariadb-server
```

### Удаляем старые пакеты, если они есть

```bash
sudo apt autoremove
```

### Перезапускаем

```bash
sudo systemctl daemon-reload
```

### Проверим статус

```bash
sudo systemctl status mariadb
```
