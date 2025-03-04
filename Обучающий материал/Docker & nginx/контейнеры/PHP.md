### 1. Конфиг docker-compose.yml
```yml
services:
	php-fpm:
	build:
		context: .
		dockerfile: Dockerfile
	container_name: php
	working_dir: /var/www/
	restart: always
	volumes:
		- ./:/var/www/- 
	networks:
		- tonar

networks:
	tonar:
		external: true
```

### 2. Конфиг для Dockerfile пример
```DockerFile
# Базовый образ
FROM php:8.3-fpm 

# Обновляем систему и устанавливаем необходимые зависимости
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
	&& pecl install redis-5.3.7 \
	&& docker-php-ext-enable redis \
	&& rm -rf /var/lib/apt/lists/*  

# Создаем группу и пользователя с нужными UID/GID
RUN groupadd -g ${GROUP_ID} administrator && \
useradd -u ${USER_ID} -g administrator -m administrator

# Меняем права доступа
RUN mkdir -p /var/www && chown -R administrator:administrator /var/www

# Переключаемся на пользователя administrator
USER administrator

# Копируем приложение
COPY --chown=administrator:administrator . .  

# Проверка установленных модулей (необязательно)
RUN php -m
```

