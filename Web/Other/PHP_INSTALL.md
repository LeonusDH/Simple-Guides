# Устанавливаем **PHP** на **Ubuntu**

### Требования:

- Прямые руки
- Умение гуглить

# Подготовка

### Установим необходимые пакеты

```bash
sudo apt-get install -y software-properties-common apt-transport-https
```

### Добавим репозиторий

```bash
LC_ALL=C.UTF-8 add-apt-repository -y ppa:ondrej/php
```

### Обновим репозитории

```bash
sudo apt-get update
```

# Ставим нужную вам версию **PHP**

> В моём случае - **PHP 8.3**

```bash
sudo apt-get install -y php8.3 php8.3-{common,cli,gd,mysql,mbstring,bcmath,xml,fpm,curl,zip,pdo}
```
