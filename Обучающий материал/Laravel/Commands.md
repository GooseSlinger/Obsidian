1. Команда перезапускает миграции и запускает сидеры
```bash
php artisan migrate:fresh --seed
```

2. Установка api 
```bash
php artisan install:api
```

3. Создание Form Request.
```bash
php artisan make:request название_реквеста
```

4. Очистка кеша
```bash
php artisan config:clear
php artisan cache:clear
php artisan route:clear
php artisan view:clear
```

5. Запуск очередей
```bash
php artisan queue:work
```

