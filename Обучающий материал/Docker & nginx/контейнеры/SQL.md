### 1. Конфиг docker-compose.yml
```yml
services:
	mysql:
	image: mysql:5.7
	container_name: sql_gate
	restart: always
	environment:
	MYSQL_ROOT_PASSWORD: rootpassword
	volumes:
		- mysql_data:/var/lib/mysql
		- ./mysql-init:/docker-entrypoint-initdb.d
	networks:
		- tonar
	ports:
		- "3308:3306"  

volumes:
	mysql_data:

networks:
	tonar:
		external: true
```

### Конфиг для init пример
```sql
-- Создаем пользователя, если он еще не создан
CREATE USER IF NOT EXISTS 'admin'@'%' IDENTIFIED BY 'test1234';  

-- Предоставляем все права пользователю root
GRANT ALL PRIVILEGES ON *.* TO 'admin'@'%' WITH GRANT OPTION;  

-- Применяем изменения
FLUSH PRIVILEGES;
```