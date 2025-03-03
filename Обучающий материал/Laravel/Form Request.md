Form Request - это отдельный класс, предназначенный для:

1. Валидации входящих данных
2. Авторизации запросов
3. Централизованного управления логикой валидации

Когда вы создаете Form Request командой:

```bash
php artisan make:request название_реквеста
```

Laravel автоматически генерирует класс `RegistrationRequest` в директории `App\Http\Requests`.

### Для чего нужен Form Request?

1. **Организация кода** - выносит правила валидации из контроллера в отдельный класс
2. **Ре usability (переиспользование)** - можно использовать один и тот же Form Request в разных местах приложения
3. **Чистота контроллеров** - делает контроллеры более чистыми и понятными
4. **Авторизация** - позволяет легко проверять права доступа к определенным действиям

### Как пользоваться Form Request?

1. Созданный класс содержит два основных метода:
    - `authorize()` - определяет, имеет ли пользователь право делать данный запрос
    - `rules()` - содержит правила валидации для входящих данных

Пример содержимого сгенерированного класса:

```php
namespace App\Http\Requests;

use Illuminate\Foundation\Http\FormRequest;

class RegistrationRequest extends FormRequest
{
    public function authorize()
    {
        // Здесь можно указать логику авторизации
        // Например, проверить роль пользователя
        return true; // Возвращаем true, если запрос разрешен
    }

    public function rules()
    {
        return [
            'name' => 'required|string|max:255',
            'email' => 'required|email|unique:users,email',
            'password' => 'required|min:6|confirmed',
        ];
    }
}
```
2. Использование в контроллере:

```php
namespace App\Http\Controllers;

use App\Http\Requests\RegistrationRequest;
use App\Models\User;

class AuthController extends Controller
{
    public function register(RegistrationRequest $request)
    {
        // Если мы добрались до этой точки,
        // значит данные уже прошли валидаци
    }
}
```

### Преимущества использования Form Request:

1. **Автоматическая валидация** - если данные не проходят валидацию, Laravel автоматически возвращает ошибку
2. **Кастомные сообщения** - можно добавить метод `messages()` для настройки собственных сообщений об ошибках
3. **Атрибуты** - метод `attributes()` позволяет задать пользовательские названия полей
4. **Подготовка данных** - метод `prepareForValidation()` позволяет модифицировать входящие данные перед валидацией
5. Удаление поля message из ошибки валидации - failedValidation()

Пример расширенного использования:

```php
public function messages()
{
    return [
        'name.required' => 'Имя обязательно для заполнения',
        'email.email' => 'Неверный формат email',
    ];
}

public function attributes()
{
    return [
        'name' => 'имя пользователя',
        'email' => 'адрес электронной почты',
    ];
}

protected function prepareForValidation()
{
    $this->merge([
        'name' => trim($this->name),
        'email' => strtolower($this->email),
    ]);
}

public function failedValidation(Validator $validator)
{
	throw new HttpResponseException(response()->json([
		'errors' => $validator->errors(),
	], 422));
}
```
