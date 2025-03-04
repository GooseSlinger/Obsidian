### 1. Основные методы Schema
```php
// Создание новой таблицы
Schema::create($table, $callback); 
// Пример: создает новую таблицу users
Schema::create('users', function (Blueprint $table) {
    $table->id();
});

// Изменение существующей таблицы
Schema::table($table, $callback);
// Пример: добавляет новый столбец в таблицу users
Schema::table('users', function (Blueprint $table) {
    $table->string('new_column');
});

// Переименование таблицы
Schema::rename($from, $to);
// Пример: переименует таблицу old_table в new_table
Schema::rename('old_table', 'new_table');

// Условное удаление таблицы (если существует)
Schema::dropIfExists($table);
// Пример: удаляет таблицу users если она существует
Schema::dropIfExists('users');

// Удаление таблицы
Schema::drop($table);
// Пример: принудительно удаляет таблицу users
Schema::drop('users');

// Проверка существования таблицы
Schema::hasTable($table);
// Пример: проверяет существует ли таблица users
if (Schema::hasTable('users')) {
    // код
}
```

### 2. Методы для создания столбцов
```php
// AUTO_INCREMENT BIGINT первичный ключ
$blueprint->bigIncrements($column);
// Пример:
$table->bigIncrements('id');

// VARCHAR
$blueprint->string($column, $length = 255);
// Пример:
$table->string('name', 100);

// INTEGER
$blueprint->integer($column);
// Пример:
$table->integer('age');

// TEXT
$blueprint->text($column);
// Пример:
$table->text('description');

// BOOLEAN
$blueprint->boolean($column);
// Пример:
$table->boolean('is_active');

// DATETIME
$blueprint->dateTime($column);
// Пример:
$table->dateTime('created_at');

// DECIMAL
$blueprint->decimal($column, $precision = 8, $scale = 2);
// Пример:
$table->decimal('price', 10, 2);

// JSON
$blueprint->json($column);
// Пример:
$table->json('settings');
```

### 3. Методы для изменения столбцов
```php
// Изменение типа или атрибутов существующего столбца
$blueprint-><тип_столбца>($column)->change();
// Пример: изменение типа столбца description на text
$table->text('description')->change();

// Важно: метод change() требует установки пакета doctrine/dbal
// Установка: composer require doctrine/dbal
```

### 4. Методы для первичных ключей и индексов
```php
// Первичный ключ
$blueprint->primary($columns);
// Пример:
$table->primary('id');

// Уникальный индекс
$blueprint->unique($columns);
// Пример:
$table->unique('email');

// Обычный индекс
$blueprint->index($columns);
// Пример:
$table->index('category_id');

// Удаление первичного ключа
$blueprint->dropPrimary($index);
// Пример:
$table->dropPrimary('users_id_primary');

// Удаление уникального индекса
$blueprint->dropUnique($index);
// Пример:
$table->dropUnique('users_email_unique');
```

### 5. Методы для внешних ключей
```php
// Внешний ключ с автоматическим связыванием (Laravel 8+)
$blueprint->foreignId($column)->constrained();
// Пример:
$table->foreignId('user_id')->constrained();

// Традиционный способ создания внешнего ключа
$blueprint->foreign($columns)->references($columns)->on($table);
// Пример:
$table->foreign('user_id')->references('id')->on('users');

// Действие при удалении связанной записи
$blueprint->onDelete('cascade');
// Пример:
$table->foreign('user_id')
      ->references('id')->on('users')
      ->onDelete('cascade');

// Удаление внешнего ключа
$blueprint->dropForeign($index);
// Пример:
$table->dropForeign('posts_user_id_foreign');
```

### 6. Методы для временных меток
```php
// Создает created_at и updated_at
$blueprint->timestamps();
// Пример:
$table->timestamps();

// Создает created_at и updated_at с NULL
$blueprint->nullableTimestamps();
// Пример:
$table->nullableTimestamps();

// Добавляет поле deleted_at для мягкого удаления
$blueprint->softDeletes();
// Пример:
$table->softDeletes();

// Добавляет поле remember_token
$blueprint->rememberToken();
// Пример:
$table->rememberToken();
```

### 7. Условные методы
```php
// Добавление после указанного столбца
$blueprint->after($column);
// Пример:
$table->string('middle_name')->after('first_name');

// Добавление в начало таблицы
$blueprint->first();
// Пример:
$table->string('prefix')->first();

// Разрешение NULL значений
$blueprint->nullable();
// Пример:
$table->string('optional_field')->nullable();

// Значение по умолчанию
$blueprint->default($value);
// Пример:
$table->integer('status')->default(0);

// Комментарий к столбцу
$blueprint->comment($comment);
// Пример:
$table->string('title')->comment('Название статьи');
```

### 8. Методы для удаления столбцов
```php
// Удаление одного или нескольких столбцов
$blueprint->dropColumn($columns);
// Пример:
$table->dropColumn('old_column');

// Удаление первичного ключа
$blueprint->dropPrimary($index);
// Пример:
$table->dropPrimary('users_id_primary');

// Удаление уникального индекса
$blueprint->dropUnique($index);
// Пример:
$table->dropUnique('users_email_unique');

// Удаление обычного индекса
$blueprint->dropIndex($index);
// Пример:
$table->dropIndex('users_category_id_index');

// Удаление внешнего ключа
$blueprint->dropForeign($index);
// Пример:
$table->dropForeign('posts_user_id_foreign');
```

### 9. Методы для проверки существования
```php
// Проверка существования таблицы
Schema::hasTable($table);
// Пример:
if (Schema::hasTable('users')) {
    // код
}

// Проверка существования столбца
Schema::hasColumn($table, $column);
// Пример:
if (Schema::hasColumn('users', 'email')) {
    // код
}
```

### 10. Дополнительные методы
#### a) Настройка таблицы
```php
// Установка движка таблицы
$blueprint->engine = 'InnoDB';
// Пример:
$table->engine = 'InnoDB';

// Установка кодировки
$blueprint->charset = 'utf8mb4';
// Пример:
$table->charset = 'utf8mb4';

// Установка правила сортировки
$blueprint->collation = 'utf8mb4_unicode_ci';
// Пример:
$table->collation = 'utf8mb4_unicode_ci';
```
#### b) Создание временных таблиц
```php
// Создание временной таблицы
Schema::create('temp_table', function (Blueprint $table) {
    $table->id();
}, true); // Третий параметр указывает, что таблица временная
```
#### c) Создание хранимых процедур и триггеров
```php
// Выполнение RAW SQL для создания хранимой процедуры
DB::unprepared("
    CREATE PROCEDURE my_procedure()
    BEGIN
        SELECT * FROM users;
    END
");

// Создание триггера
DB::unprepared("
    CREATE TRIGGER my_trigger BEFORE INSERT ON users
    FOR EACH ROW SET NEW.created_at = NOW();
");
```
#### d) Переименование столбца
```php
// Переименование столбца
$blueprint->renameColumn($from, $to);
// Пример:
$table->renameColumn('old_name', 'new_name');
```
#### e) Проверка существования индекса
```php
// Проверка существования индекса
Schema::hasIndex($table, $index);
// Пример:
if (Schema::hasIndex('users', 'email_index')) {
    // Код
}
```
#### f) Проверка существования внешнего ключа
```php
// Проверка существования внешнего ключа
Schema::hasForeignKey($table, $foreign_key);
// Пример:
if (Schema::hasForeignKey('posts', 'user_id_foreign')) {
    // Код
}
```
#### g) Добавление комментария к таблице
```php
// Добавление комментария к таблице
$blueprint->comment($comment);
// Пример:
$table->comment('Это таблица пользователей');
```

### 11. Прочие методы работы с миграциями

#### a) Отключение foreign key checks
```php
// Отключение проверки внешних ключей
DB::statement('SET FOREIGN_KEY_CHECKS=0');

// Включение проверки внешних ключей
DB::statement('SET FOREIGN_KEY_CHECKS=1');
```
#### b) Оптимизация таблицы
```php
// Оптимизация таблицы
DB::statement('OPTIMIZE TABLE users');
```
#### c) Резервное копирование таблицы
```php
// Создание резервной копии таблицы
DB::statement('CREATE TABLE backup_users LIKE users');
DB::statement('INSERT INTO backup_users SELECT * FROM users');
```