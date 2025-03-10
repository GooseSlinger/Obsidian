## Установка 

1. Команда для установки
```bash
sudo apt install redis-server
```

2. Проверка статуса
```bash
sudo systemctl status redis
```

3. Установка PHP расширения для Redis
```bash
sudo apt install php-redis -y
```
После установки перезапустите веб-сервер:
```bash
sudo systemctl restart apache2   # Для Apache
# или
sudo systemctl restart nginx     # Для Nginx
```
