# Попытаюсь помочь с разными ошибками у **MySQL**/**MariaDB**

### SQLSTATE[HY000] [2054] Server sent charset (0) unknown to the client.

> Эту ошибку можно встретить при установке панели **Pterodactyl**

Измените `character-set-collations` в файле `/etc/mysql/mariadb.conf.d/50-server.cnf` на `utf8mb4=utf8mb4_general_ci`.

Перезагрузите **mariadb** после изменения, чтобы изменения вступили в силу.

```bash
sudo systemctl restart mariadb
```
