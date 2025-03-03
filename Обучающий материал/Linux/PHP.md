### 1. Установка PHP 8.3

Установите PHP 8.3 и наиболее часто используемые расширения:
```bash
sudo apt install php8.3
```

### 2. Установка дополнительных расширений PHP

```bash
sudo apt install php8.3-cli php8.3-fpm php8.3-mysql php8.3-curl php8.3-gd php8.3-mbstring php8.3-xml php8.3-zip php8.3-bcmath php8.3-intl php8.3-soap php8.3-opcache
```

### 3. Тест PHP-FPM (если используется Nginx)

```bash
sudo systemctl status php8.3-fpm
```

### 4. Настройка PHP (опционально)

Вы можете настроить параметры PHP, отредактировав файл конфигурации:
```bash
sudo nano /etc/php/8.3/fpm/php.ini
```

Например, измените следующие параметры:
- `memory_limit = 256M`
- `upload_max_filesize = 64M`
- `post_max_size = 64M`
- `max_execution_time = 300`

После внесения изменений перезапустите PHP-FPM:
```bash
sudo nano /var/www/ваш_сайт/public/index.php
```