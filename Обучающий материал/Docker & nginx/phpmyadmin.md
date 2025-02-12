1. Создаем docker-compose.yml 
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