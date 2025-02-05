## Конфиг docker compose

```yml
services:
	
	php-fpm:
		build:
			context: .
			dockerfile: Dockerfile
		container_name: php_auth
		working_dir: /var/www/
		volumes:
			- ./app:/var/www/
		networks:
			- auth

	nginx:
		image: nginx:alpine
		container_name: nginx_auth
		volumes:
			- ./app:/var/www/
			- ./nginx/default.conf:/etc/nginx/conf.d/default.conf
		ports:
			- "9090:80"
		depends_on:
			- php-fpm
		networks:
			- auth  

	mysql:	
		image: mysql:5.7
		container_name: sql_auth	
		restart: always	
		environment:	
		MYSQL_ROOT_PASSWORD: rootpassword	
		volumes:	
			- mysql_data:/var/lib/mysql	
			- ./mysql-init:/docker-entrypoint-initdb.d	
		networks:	
			- auth	
		ports:	
			- "3308:3306"

  

	phpmyadmin:	
		image: phpmyadmin/phpmyadmin	
		container_name: phpmyadmin_auth	
		restart: always	
		ports:	
			- "9091:80"	
		environment:	
			PMA_HOST: sql_auth	
			PMA_PORT: 3306	
			PMA_USER: admin	
			PMA_PASSWORD: test1234	
		networks:	
		- auth

volumes:
	mysql_data:
networks:
	auth:
		driver: bridge
```

## PHP-FPM конфиг Dockerfile

```Dockerfile
# Используем базовый образ php:8.3-fpm
FROM php:8.3-fpm  

# Устанавливаем нужные UID и GID через аргументы сборки
ARG USER_ID=1000
ARG GROUP_ID=1000  

# Устанавливаем необходимые зависимости
RUN apt-get update && apt-get install -y \
	libzip-dev \	
	zip \	
	libonig-dev \	
	libxml2-dev \	
	libcurl4-openssl-dev \	
	git \	
	unzip \	
	&& docker-php-ext-install \	
		zip \	
		pdo \	
		pdo_mysql \	
		mbstring \	
		curl \	
		xml \	
	&& pecl install redis \	
	&& docker-php-ext-enable redis

# Создаем группу с уникальным именем и пользователю с нужными UID/GID
RUN groupadd -g ${GROUP_ID} administrator && \
	useradd -u ${USER_ID} -g administrator -m administrator  

# Передача прав пользователю administrator
RUN chown -R administrator:administrator /var/www  

# Переключаемся на пользователя administrator
USER administrator

# Копируем приложение в контейнер
WORKDIR /var/www
COPY --chown=administrator:administrator . .  

# Очистка кэша
RUN rm -rf /var/lib/apt/lists/*
```

## Конфиг nginx

```
server {
    listen 80;
    server_name localhost;

    root /var/www/public;
    index index.php index.html index.htm;

    location / {
        try_files $uri $uri/ /index.php?$query_string;
    }

    location ~ \.php$ {
        include fastcgi_params;
        fastcgi_pass php_auth:9000;
        fastcgi_index index.php;
        fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
    }

    location ~ /\.ht {
        deny all;
    }

    error_log /var/log/nginx/error.log;
    access_log /var/log/nginx/access.log;
}
```

## SQL init

```sql

-- Создаем пользователя, если он еще не создан
CREATE USER IF NOT EXISTS 'admin'@'%' IDENTIFIED BY 'test1234';  

-- Предоставляем все права пользователю root
GRANT ALL PRIVILEGES ON *.* TO 'admin'@'%' WITH GRANT OPTION; 

-- Применяем изменения
FLUSH PRIVILEGES;
```

## Описание

Вообщем для того чтобы этот конфиг завелся а именно nginx нужно сделать так чтобы пользователи по имени совпадали ну и еще и по id в идеале.