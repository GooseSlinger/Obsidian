## Создание сидера

```bash
php artisan make:seeder PermissionAndRoleSeeder
```

## Запись данных в  сидер пример:

```php
$permissions = [
	'my_tonar',
	'logistics_management',
];  

foreach ($permissions as $permission) {
	Permission::create(['name' => $permission]);
}

// Создание ролей
$roles = [
	'admin' => [
		'permissions' => ['my_tonar', 'logistics_management'],
	],
	'management' => [
		'permissions' => ['my_tonar', 'logistics_management'],
	],
	'user' => [
		'permissions' => ['my_tonar'],
	],
];  

foreach ($roles as $roleName => $roleData) {
	$role = Role::create(['name' => $roleName]); 
	
	// Назначение разрешений роли
	if (!empty($roleData['permissions'])) {
		$permissions = Permission::whereIn('name', $roleData['permissions'])->get();
		$role->givePermissionTo($permissions);
	}
}  

$this->command->info('Роли и пермишены созданы!');
```

весь код нужно запихивать в `run`

## Добавление в `DatabaseSeeder` 

```php
$this->call(PermissionAndRoleSeeder::class);
```

## Запуск сидера

```bash
php artisan db:seed
```

Эта команда запускает сразу все сидеры которые прописаны в `DatabaseSeeder`, если есть потребность то можно запустить и сидер отдельно

```bash
php artisan db:seed --class=PermissionAndRoleSeeder
```