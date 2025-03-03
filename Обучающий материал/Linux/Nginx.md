### 2. Установка Nginx

Установите Nginx с помощью пакетного менеджера `apt`:
```bash
sudo apt install nginx
```

### 6. Настройка nginx

Создайте файл конфигурации для вашего сайта:
```bash
sudo nano /etc/nginx/sites-available/ваш_сайт
```

```nginx
server {
    listen 80;
	server_name ваш_сайт www.ваш_сайт;

    root /var/www/ваш_сайт/public;
    index index.php index.html index.htm;

    access_log /var/log/nginx/ваш_сайт.access.log;
    error_log /var/log/nginx/ваш_сайт.error.log;

    location / {
        try_files $uri $uri/ /index.php?$query_string;
    }

    location ~ \.php$ {
        include snippets/fastcgi-php.conf;
        fastcgi_pass unix:/var/run/php/php8.3-fpm.sock;
        fastcgi_param SCRIPT_FILENAME $realpath_root$fastcgi_script_name;
        include fastcgi_params;
    }

    location ~ /\.(?!well-known).* {
        deny all;
    }

	# для ларавель
    location ~ ^/(storage|bootstrap|config|database|resources|routes|tests|vendor)/ {
        deny all;
    }

    error_page 404 /index.php;

    client_max_body_size 100M;
}
```