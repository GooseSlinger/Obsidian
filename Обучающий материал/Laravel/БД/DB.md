## 1. Основные методы для выборки данных

### a) Выборка данных
```php
// Получение всех записей из таблицы
DB::table('users')->get();

// Получение первой записи
DB::table('users')->first();

// Получение первого результата с указанными столбцами
DB::table('users')->value('name'); // Возвращает значение одного поля
DB::table('users')->pluck('name'); // Возвращает коллекцию значений одного поля
```
### b) Условия (WHERE)
```php
// Базовый where
$users = DB::table('users')->where('id', '=', 1)->get();

// orWhere
$users = DB::table('users')
    ->where('id', 1)
    ->orWhere('name', 'John')
    ->get();

// Группировка условий (nested where)
$users = DB::table('users')
    ->where(function ($query) {
        $query->where('votes', '>', 100)
              ->orWhere('title', 'Admin');
    })
    ->get();

// whereNull
$users = DB::table('users')->whereNull('deleted_at')->get();

// whereNotNull
$users = DB::table('users')->whereNotNull('email')->get();

// whereIn
$users = DB::table('users')->whereIn('id', [1, 2, 3])->get();

// whereNotIn
$users = DB::table('users')->whereNotIn('id', [1, 2, 3])->get();

// whereBetween
$users = DB::table('users')->whereBetween('age', [18, 65])->get();

// whereNotBetween
$users = DB::table('users')->whereNotBetween('age', [18, 65])->get();

// whereDate
$users = DB::table('users')->whereDate('created_at', '2023-01-01')->get();

// whereDay
$users = DB::table('users')->whereDay('created_at', '15')->get();

// whereMonth
$users = DB::table('users')->whereMonth('created_at', '05')->get();

// whereYear
$users = DB::table('users')->whereYear('created_at', '2023')->get();

// whereTime
$users = DB::table('users')->whereTime('created_at', '14:00:00')->get();

// Дополнительные операторы сравнения
$users = DB::table('users')->where('age', '>', 18)->get(); // больше
$users = DB::table('users')->where('age', '<', 65)->get(); // меньше
$users = DB::table('users')->where('age', '<=', 65)->get(); // меньше или равно
$users = DB::table('users')->where('age', '>=', 18)->get(); // больше или равно

// whereExists (проверка существования записей в другой таблице)
$users = DB::table('users')
    ->whereExists(function ($query) {
        $query->select(DB::raw(1))
              ->from('orders')
              ->whereRaw('orders.user_id = users.id');
    })
    ->get();

// whereHas (для отношений Eloquent)
$categories = DB::table('categories')
    ->whereHas('products', function ($query) {
        $query->where('status', 'active');
    })
    ->get();
```

## 2. Методы для вставки данных

### a) Вставка одной записи
```php
DB::table('users')->insert([
    'name' => 'John',
    'email' => 'john@example.com',
    'created_at' => now(),
    'updated_at' => now()
]);
```
### b) Вставка нескольких записей
```php
DB::table('users')->insert([
    ['name' => 'John', 'email' => 'john@example.com'],
    ['name' => 'Jane', 'email' => 'jane@example.com']
]);
```
### c) Вставка с получением ID
```php
$id = DB::table('users')->insertGetId([
    'name' => 'John',
    'email' => 'john@example.com'
]);
```

## 3. Методы для обновления данных

### a) Обновление записей
```php
DB::table('users')->where('id', 1)->update(['name' => 'John Doe']);
```
### b) Увеличение/уменьшение значения
```php
DB::table('users')->increment('votes'); // votes += 1
DB::table('users')->decrement('votes', 5); // votes -= 5
```

## 4. Методы для удаления данных

### a) Удаление записей
```php
DB::table('users')->where('id', 1)->delete();
DB::table('users')->delete(); // Удалить все записи
```

## 5. Методы агрегации
```php
// Подсчет записей
$count = DB::table('users')->count();

// Минимальное значение
$min = DB::table('users')->min('age');

// Максимальное значение
$max = DB::table('users')->max('age');

// Среднее значение
$avg = DB::table('users')->avg('age');

// Сумма значений
$sum = DB::table('users')->sum('votes');
```

## 6. Методы для работы с JOIN
```php
// INNER JOIN
DB::table('users')
    ->join('contacts', 'users.id', '=', 'contacts.user_id')
    ->select('users.*', 'contacts.phone')
    ->get();

// LEFT JOIN
DB::table('users')
    ->leftJoin('posts', 'users.id', '=', 'posts.user_id')
    ->get();

// RIGHT JOIN
DB::table('users')
    ->rightJoin('posts', 'users.id', '=', 'posts.user_id')
    ->get();
```

## 7. Методы для группировки и сортировки

### a) Группировка
```php
DB::table('orders')
    ->select('user_id', DB::raw('SUM(price) as total'))
    ->groupBy('user_id')
    ->get();
```
### b) Сортировка
```php
DB::table('users')
    ->orderBy('name', 'asc') // ASC или DESC
    ->get();
```

## 8. Методы для ограничения результатов
```php
// Лимитирование количества записей
DB::table('users')->take(10)->get();

// Пропуск записей
DB::table('users')->skip(10)->take(5)->get();
```

## 9. Методы для подзапросов
```php
// Подзапрос в WHERE
DB::table('users')
    ->whereIn('id', function ($query) {
        $query->select('user_id')
              ->from('orders')
              ->where('total', '>', 100);
    })
    ->get();

// Подзапрос в SELECT
DB::table('users')
    ->select('name', DB::raw('(SELECT COUNT(*) FROM posts WHERE posts.user_id = users.id) as post_count'))
    ->get();
```

## 10. Методы для транзакций
```php
DB::beginTransaction(); // Начало транзакции

try {
    DB::table('users')->insert([...]);
    DB::table('profiles')->insert([...]);
    DB::commit(); // Подтверждение транзакции
} catch (\Exception $e) {
    DB::rollBack(); // Откат транзакции
    throw $e;
}
```

## 11. Методы для управления таблицами

### a) Проверка существования таблицы
```php
if (DB::connection()->getSchemaBuilder()->hasTable('users')) {
    // Таблица существует
}
```
### b) Проверка существования столбца
```php
if (DB::connection()->getSchemaBuilder()->hasColumn('users', 'email')) {
    // Столбец существует
}
```

## 12. Другие полезные методы

### a) Работа с сырыми выражениями
```php
DB::table('users')
    ->select(DB::raw('COUNT(*) as user_count, status'))
    ->groupBy('status')
    ->get();
```
### b) Соединение с конкретной базой данных
```php
DB::connection('mysql')->table('users')->get(); // Использование соединения 'mysql'
```
### c) Получение последнего выполненного SQL-запроса
```php
DB::enableQueryLog();
DB::table('users')->get();
$queries = DB::getQueryLog();
dd($queries);
```

