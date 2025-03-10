### 1. Конфиг docker-compose.yml
```yml
services:
	mailhog:
	image: mailhog/mailhog
	container_name: mailhog
	ports:
		- "8025:8025"
		- "1025:1025"
	restart: always
	networks:
		- tonar

networks:
	tonar:
		external: true
```