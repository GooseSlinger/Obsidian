### 1. Создаем docker-compose.yml 
```yml
services:
	phpmyadmin:
		image: phpmyadmin/phpmyadmin
		container_name: phpmyadmin
		restart: always
		ports:
		- "8080:80"
		volumes:
		- ./config.inc.php:/etc/phpmyadmin/config.inc.php
		networks:
		- tonar
		- 
networks:
	tonar:
		external: true
```

### 2. Конфиг для config.inc.php пример
```php
<?php
	$i = 0;
		
	// Первый сервер
	$i++;
	$cfg['Servers'][$i]['host'] = '10.11.7.251';
	$cfg['Servers'][$i]['user'] = 'artem';
	$cfg['Servers'][$i]['password'] = 'test1234';
	$cfg['Servers'][$i]['connect_type'] = 'tcp';
	$cfg['Servers'][$i]['auth_type'] = 'config';
```