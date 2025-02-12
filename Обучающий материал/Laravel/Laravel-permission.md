## Установка

1. Устанавливаем
```bash
composer require spatie/laravel-permission
```

2. Публикуем настройки
```bash
php artisan vendor:publish --provider="Spatie\Permission\PermissionServiceProvider"
```

1. Добавляем трейт в модель User
```php
use Spatie\Permission\Traits\HasRoles;

class User extends Authenticatable
{
    use HasRoles;
}
```

## Создание ролей и разрешений

1. Создание ролей
```php
Role::create(['name' => 'admin']);
```

2. Создание разрешений
```php
Permission::create(['name' => 'manage_users']);
```

3. Назначение разрешения на роль
```php
// Нахождение ролей
$adminRole = Role::findByName('admin', 'api');

// Назначение разрешений ролям
$adminRole->givePermissionTo('access_api');
```

4. Назначение роли пользователю
```php
$user = User::find(1);
// Назначение роли пользователю
$user->assignRole('admin'); 
```

5. Удаление разрешений у роли
```php
$adminRole->revokePermissionTo('delete_post');
```

6. Удаление ролей у пользователя
```php
$user->removeRole('admin');
```

7. Возвращает полный список всех разрешений пользователя, включая как прямые разрешения, так и те, что получены через роли.
```php
$permissions = $user->getAllPermissions();
```

8. Возвращает имена ролей, которые назначены пользователю
```php
$roles = $user->getRoleNames();
```

9. Удаление ролей
```php
$role = Role::findByName($request->role_name);
$role->delete();
```
