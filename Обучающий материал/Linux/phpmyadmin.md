### 1. Установка phpMyAdmin

Установите phpMyAdmin:
```bash
sudo apt install phpmyadmin
```

Во время установки вам будет предложено выбрать веб-сервер (Apache или Nginx):
- Если вы используете **Apache**, выберите его с помощью пробела и нажмите Enter.
- Если вы используете **Nginx**, не выбирайте никакой сервер (просто нажмите Enter).

Настройте базу данных для phpMyAdmin:
- Выберите **Yes**, чтобы настроить базу данных с помощью `dbconfig-common`.
- Введите пароль для пользователя `phpmyadmin` в MySQL.

### 2. Конфиг для nginx
```conf
server {
    listen 8080;
    server_name phpmyadmin;

    root /usr/share/phpmyadmin;
    index index.php index.html index.htm;

    access_log /var/log/nginx/phpmyadmin.access.log;
    error_log /var/log/nginx/phpmyadmin.error.log;

    client_max_body_size 100M;

    location / {
        try_files $uri $uri/ /index.php?$query_string;
    }

    location /phpmyadmin {
        root /usr/share;
        index index.php;

        location ~ ^/phpmyadmin/(.+\.php)$ {
            include snippets/fastcgi-php.conf;
            fastcgi_pass unix:/var/run/php/php8.3-fpm.sock;
            fastcgi_param SCRIPT_FILENAME $document_root/$1;
            include fastcgi_params;
        }

        location ~ ^/phpmyadmin/(.+\.jpg|.+\.jpeg|.+\.gif|.+\.png|.+\.css|.+\.js)$ {
            root /usr/share/phpmyadmin;
        }
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

    error_page 404 /index.php;
}

```