1. Редактирование env laravel
```env
REDIS_HOST=127.0.0.1
REDIS_PASSWORD=null
REDIS_PORT=6379

CACHE_STORE=redis

QUEUE_CONNECTION=redis

SESSION_DRIVER=redis
```

2. Проверить конфиги кеша<mark style="background: #D2B3FFA6;">(config/cache.php)</mark>, сессии<mark style="background: #D2B3FFA6;">(config/session.php)</mark> и очередей<mark style="background: #D2B3FFA6;">(config/queue.php)</mark>. В этих конфигах должны быть ваши значения и .env

3. Перезапуск кеша конфига
```php
php artisan config:cache
```

