### 1. Установка MySQL

Установите MySQL с помощью пакетного менеджера `apt`:
```bash
sudo apt install mysql-server
```

### 2. Настройка безопасности MySQL

MySQL поставляется с утилитой `mysql_secure_installation`, которая помогает настроить базовые параметры безопасности:
```bash
sudo mysql_secure_installation
```
- **Включить проверку сложности пароля** (рекомендуется): Нажмите `Y`.
- **Установить пароль для root-пользователя**: Введите и подтвердите пароль.
- **Удалить анонимных пользователей**: Нажмите `Y`.
- **Запретить удалённый вход для root**: Нажмите `Y`.
- **Удалить тестовую базу данных**: Нажмите `Y`.
- **Перезагрузить привилегии**: Нажмите `Y`.

### 3. Подключение к MySQL

После настройки подключитесь к MySQL:
```bash
sudo mysql -u root -p
```

### 4. Настройка удалённого доступа (опционально)

Откройте конфигурационный файл MySQL:
```bash
sudo nano /etc/mysql/mysql.conf.d/mysqld.cnf
```

Найдите строку `bind-address` и измените её значение:
```bash
bind-address = 0.0.0.0
```

Сохраните файл и перезапустите MySQL:
```bash
sudo systemctl restart mysql
```

Создайте пользователя с доступом с удалённых хостов:
```bash
CREATE USER 'username'@'%' IDENTIFIED BY 'password';
GRANT ALL PRIVILEGES ON *.* TO 'username'@'%' WITH GRANT OPTION;
FLUSH PRIVILEGES;
```

### 5. Изменения пароля рута

```bash
sudo mysql -u root -p

ALTER USER 'root'@'localhost' IDENTIFIED BY 'новый_пароль';
FLUSH PRIVILEGES;
```

### 6. Удаление пользователя

```mysql
DROP USER 'remote_admin'@'%';
FLUSH PRIVILEGES;
```

### 7. Создание пользователя
```mysql
CREATE USER 'имя_пользователя'@'хост' IDENTIFIED BY 'пароль';
GRANT ALL PRIVILEGES ON *.* TO 'newadmin'@'localhost' WITH GRANT OPTION;
FLUSH PRIVILEGES;
```

### 7. Просмотр пользователей
```mysql
SELECT User, Host FROM mysql.user;
```
