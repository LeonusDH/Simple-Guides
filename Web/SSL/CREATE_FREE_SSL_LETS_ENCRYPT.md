# Создаём бесплатный сертификат Let's Encrypt на Ubuntu (есть доступ к настройке DNS)

> В принципе, это подходит не только для **Ubuntu**.

> Просто найдите нужный вам способ установки [SNAP](https://snapcraft.io/docs/installing-snapd)

### Требования:

- Прямые руки
- Умение гуглить
- Установленный пакетный менеджер `SNAP`

Установить **SNAP** на **Ubuntu/Debian**:

```bash
sudo apt-get install snapd
```

# Установка

## Устанавливаем **Certbot**

```bash
sudo snap install --classic certbot
```

## Делаем **Certbot** командой

```
sudo ln -s /snap/bin/certbot /usr/bin/certbot
```

# Генерируем сертификат

> `<domain>` - домен (формат: example.com)

> Все сертификаты будут по пути `/etc/letsencrypt/live/<domain>/`

## Для домена:

```bash
sudo certbot certonly --authenticator manual --preferred-challenges dns
```

## Для домена и поддоменов:

```bash
sudo certbot certonly --manual --preferred-challenges=dns --server <https://acme-v02.api.letsencrypt.org/directory> --agree-tos -d <domain> -d *.<domain>
```
