### 1. Конфиг docker-compose.yml
```yml
services:
	nginx:
	image: nginx:alpine
	container_name: nginx
	restart: always
	volumes:
		- ./sites-enabled:/etc/nginx/conf.d
		- ./:/var/www/
	ports:
		- "9091:9091"
		- "9092:9092"
		- "9093:9093"
		- "9094:9094"
		- "9095:9095"
	networks:
		- tonar

networks:
	tonar:
		external: true
```

### 2. Конфиг для sites-enabled пример
```conf
server {
	listen 9093;
	server_name gate.local;  
	
	root /var/www/gate/public;
	index index.php index.html index.htm;  
	
	location / {
		try_files $uri $uri/ /index.php?$query_string;
	}
	
	# Обработка PHP файлов
	location ~ \.php$ {
		include fastcgi_params;
		fastcgi_pass php:9000;
		fastcgi_index index.php;
		fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
	}
	
	# Обработка статических файлов
	location ~* \.(css|js|png|jpg|jpeg|gif|ico|svg|woff|woff2|ttf|eot)$ {
		expires max;
		log_not_found off;
	}
	
	# Защита .htaccess файлов
	location ~ /\.ht {
		deny all;
	}
	
	error_log /var/log/nginx/error.log;
	access_log /var/log/nginx/access.log;
}
```