Guzzle — это современный HTTP-клиент для PHP, который позволяет вам легко выполнять HTTP-запросы в ваших приложениях. Laravel предоставляет простой способ интеграции Guzzle через Composer. Ниже описан полный процесс использования GuzzleHttp\Client в Laravel, включая его установку, базовые примеры использования и основные возможности.

### 1. **Установка Guzzle**

Для начала вам нужно установить пакет `guzzlehttp/guzzle` через Composer:

```bash
composer require guzzlehttp/guzzle
```

### 2. **Базовая настройка**

Laravel автоматически поддерживает автозагрузку классов через Composer, поэтому вам не нужно дополнительно настраивать Guzzle. Вы можете сразу начать использовать его в своих контроллерах, сервисах или других компонентах приложения.

```php
use GuzzleHttp\Client;
```

### 3. **Основные функции и возможности Guzzle**

#### a) **Создание клиента**

Вы можете создать экземпляр клиента с помощью следующего кода:

```php
$client = new Client([
    'base_uri' => 'https://api.example.com', // Базовый URI для всех запросов
    'timeout'  => 5.0,                      // Таймаут соединения (в секундах)
]);
```

Если базовый URI не нужен, вы можете просто создать клиент без параметров:
```php
$client = new Client();
```

#### b) **Выполнение GET-запроса**

```php
$response = $client->request('GET', '/endpoint');
// Получение статуса ответа
echo $response->getStatusCode(); // Например, 200
// Получение тела ответа как строку
echo $response->getBody();
// Получение заголовков ответа
print_r($response->getHeaders());
```

Альтернативный синтаксис для GET-запроса:
```php
$response = $client->get('/endpoint');
```

#### c) **Выполнение POST-запроса**

```php
// Или отправка JSON-данных
$response = $client->request('POST', '/endpoint', [
    'json' => [
        'key1' => 'value1',
        'key2' => 'value2',
    ],
]);
```


Альтернативный синтаксис для POST-запроса:
```php
$response = $client->post('/endpoint', [
    'form_params' => [
        'key1' => 'value1',
        'key2' => 'value2',
    ],
]);
```

#### d) **Выполнение PUT/PATCH/DELETE-запросов**

PUT-запрос:
```php
$response = $client->put('/endpoint', [
    'body' => 'Raw data here',
]);
```

PATCH-запрос:
```php
$response = $client->patch('/endpoint', [
    'json' => [
        'key' => 'updated value',
    ],
]);
```

DELETE-запрос:
```php
$response = $client->delete('/endpoint');
```

#### e) **Обработка ошибок**

Guzzle выбрасывает исключения при возникновении ошибок. Вы можете обработать их с помощью блока `try-catch`:
```php
use GuzzleHttp\Exception\RequestException;

try {
    $response = $client->request('GET', '/endpoint');
    echo $response->getBody();
} catch (RequestException $e) {
    if ($e->hasResponse()) {
        echo 'Ошибка: ' . $e->getResponse()->getStatusCode();
    } else {
        echo 'Произошла ошибка при выполнении запроса.';
    }
}
```

#### f) **Загрузка файлов**

Для отправки файлов используйте параметр `multipart`:
```php
$response = $client->request('POST', '/upload', [
    'multipart' => [
        [
            'name'     => 'file',
            'contents' => fopen('/path/to/file', 'r'),
        ],
    ],
]);
```

#### g) **Использование токена авторизации**

Если API требует авторизацию через токен, вы можете добавить заголовок `Authorization`:
```php
$response = $client->request('GET', '/endpoint', [
    'headers' => [
        'Authorization' => 'Bearer YOUR_ACCESS_TOKEN',
    ],
]);
```

#### h) **Асинхронные запросы**

Guzzle поддерживает асинхронные запросы:
```php
$promise = $client->requestAsync('GET', '/endpoint');

$promise->then(function ($response) {
    echo $response->getBody();
});
```

### 4. **Продвинутые возможности**

#### a) **HTTP-прокси**

Вы можете настроить использование прокси:
```php
$client = new Client([
    'proxy' => 'tcp://127.0.0.1:8123',
]);
```

#### b) **Ограничение количества попыток**

Вы можете настроить повторные попытки при временных ошибках:
```php
$client = new Client([
    'retries' => 3,
]);
```

### 5. **Пример использования в контроллере Laravel**

Вот пример использования Guzzle в контроллере Laravel:
```php
namespace App\Http\Controllers;

use Illuminate\Http\Request;
use GuzzleHttp\Client;

class ApiController extends Controller
{
    public function fetchData()
    {
	    try {
	        $client = new Client;
	                
            $response = $http->post(config('app.auth') . '/api/registration', [
				'json' => [				
					'last_name' => $request->last_name,				
					'first_name' => $request->first_name,				
					'middle_name' => $request->middle_name,				
					'login' => $request->login,				
					'password' => $request->password,				
					'user_type' => $request->user_type				
				],				
			]);
			
            $data = json_decode($response->getBody(), true);

            return response()->json(['message' => $data]);
        } catch (\GuzzleHttp\Exception\RequestException $e) {
            return response()->json(['error' => 'Failed to fetch data'], 500);
        }
    }
}
```