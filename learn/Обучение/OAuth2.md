# Установка и настройка Passport

#### 1. Установка пакета Passport

Установите пакет Passport через Composer: 
```
composer require laravel/passport
```
#### 2. Миграция базы данных

Выполните миграции для создания необходимых таблиц в базе данных: `php artisan migrate`
#### 3. Запуск команды Passport

Запустите команду для генерации ключей шифрования и других конфигурационных данных: 

```
	php artisan passport:install
```

полученные в консоли token и id записываем в .env:

```
PASS_ID=*
PASS_SECRET=**********
```

теперь чтобы использовать их в коде нужно запихнуть их в конфиг, ищем service.php и запихиваем его туда:
```
'passport' => [
    'client_id' => env('PASS_ID'),
    'client_secret' => env('PASS_GRANT'),
],
```
#### 4. Обновление модели User

Добавьте `Laravel\Passport\HasApiTokens` трейт к вашей модели `User`:

```php
use Illuminate\Foundation\Auth\User as Authenticatable;
use Laravel\Passport\HasApiTokens;

class User extends Authenticatable
{
    use HasApiTokens, Notifiable;
}
```

#### 5. Настройка маршрутов

Добавьте маршруты для регистрации и входа в ваш файл `routes/api.php`:

```php
use App\Http\Controllers\AuthController;

Route::post('login', [AuthController::class, 'login']);
Route::post('register', [AuthController::class, 'register']);
```

#### 6. Создание контроллера AuthController

Создайте контроллер `AuthController` с методами для регистрации и входа:
```php
public function login(Request $request)

{
$request->validate([
'username' => 'required|min:1|max:255',
'password' => 'required|min:1|max:255',
]);  

$http = new \GuzzleHttp\Client; 

$response = $http->post(config('app.url') . '/oauth/token', [
'form_params' => [
'grant_type' => 'password',
'client_id' => config('services.passport.client_id'),
'client_secret' => config('services.passport.client_secret'),
'username' => $request->username,
'password' => $request->password,
'scope' => '',
],
]);  

return json_decode((string) $response->getBody(), true);
}
```


