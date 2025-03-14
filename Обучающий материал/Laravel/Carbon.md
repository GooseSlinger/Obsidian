Carbon предоставляет огромное количество методов для работы с датами и временем. Полный список функций можно найти в [официальной документации Carbon](https://carbon.nesbot.com/docs/) . Однако, я могу предоставить вам обзор основных и наиболее часто используемых методов с примерами.

---

### 1. **Создание объекта Carbon**
```php
use Carbon\Carbon;

// Текущая дата и время
$now = Carbon::now();
echo $now; // 2023-10-05 14:30:00

// Создание даты из строки
$date = Carbon::parse('2023-10-01 10:00:00');
echo $date; // 2023-10-01 10:00:00

// Создание даты из timestamp
$timestamp = Carbon::createFromTimestamp(1696507200);
echo $timestamp; // 2023-10-05 12:00:00
```

### 2. **Форматирование даты**
```php
$now = Carbon::now();

// Форматирование в строку
echo $now->format('Y-m-d'); // 2023-10-05
echo $now->toDateString();  // 2023-10-05
echo $now->toTimeString();  // 14:30:00
echo $now->toDateTimeString(); // 2023-10-05 14:30:00
```

### 3. **Добавление и вычитание времени**
```php
$now = Carbon::now();

// Добавить время
echo $now->addDays(5);       // 2023-10-10 14:30:00
echo $now->addMonths(2);     // 2023-12-10 14:30:00
echo $now->addYears(1);      // 2024-12-10 14:30:00
echo $now->addHours(3);      // 2024-12-10 17:30:00

// Вычесть время
echo $now->subDays(2);       // 2024-12-08 17:30:00
echo $now->subMonths(1);     // 2024-11-08 17:30:00
echo $now->subYears(1);      // 2023-11-08 17:30:00
```

### 4. **Сравнение дат**
```php
$date1 = Carbon::parse('2023-10-01');
$date2 = Carbon::parse('2023-10-05');

if ($date1->lt($date2)) {
    echo 'Дата 1 раньше даты 2'; // true
}

if ($date1->gt($date2)) {
    echo 'Дата 1 позже даты 2';
}

if ($date1->eq($date2)) {
    echo 'Даты равны';
}

if ($date1->isSameDay($date2)) {
    echo 'Даты относятся к одному дню';
}
```

### 5. **Человекочитаемый вывод**
```php
$now = Carbon::now();
$past = Carbon::parse('2023-10-01 10:00:00');

echo $past->diffForHumans($now); // 4 дня назад
echo $now->diffForHumans($past); // через 4 дня
```

### 6. **Локализация**
```php
$now = Carbon::now();

// Установка локали
Carbon::setLocale('ru');

echo $now->diffForHumans(); // 5 минут назад
```

### 7. **Проверка дат**
```php
$now = Carbon::now();
$date = Carbon::parse('2023-10-01');

// Проверка на будущее или прошлое
if ($date->isPast()) {
    echo 'Дата в прошлом';
}

if ($date->isFuture()) {
    echo 'Дата в будущем';
}

// Проверка на конкретный день недели
if ($date->isMonday()) {
    echo 'Это понедельник';
}

if ($date->isWeekend()) {
    echo 'Это выходной';
}
```

### 8. **Работа с временными интервалами**
```php
$start = Carbon::parse('2023-10-01');
$end = Carbon::parse('2023-10-05');

// Разница в днях
echo $start->diffInDays($end); // 4

// Разница в часах
echo $start->diffInHours($end); // 96

// Разница в минутах
echo $start->diffInMinutes($end); // 5760
```

### 9. **Копирование дат**
```php
$date = Carbon::parse('2023-10-01');
$copy = $date->copy();

echo $copy; // 2023-10-01 00:00:00
```

### 10. **Работа с временными зонами**
```php
$date = Carbon::now('UTC');
echo $date; // 2023-10-05 14:30:00 UTC

// Изменение временной зоны
$date->setTimezone('Europe/Moscow');
echo $date; // 2023-10-05 17:30:00 MSK
```

### 11. **Модификация даты**
```php
$date = Carbon::parse('2023-10-01');

// Установка конкретного значения
$date->year(2024);
$date->month(12);
$date->day(25);

echo $date; // 2024-12-25 00:00:00
```

### 12. **Генерация случайных дат**
```php
$date = Carbon::now()->subDays(rand(1, 365));
echo $date; // Случайная дата за последний год
```

### 13. **Получение начала и конца периода**
```php
$date = Carbon::parse('2023-10-05');

// Начало дня
echo $date->startOfDay(); // 2023-10-05 00:00:00

// Конец дня
echo $date->endOfDay();   // 2023-10-05 23:59:59

// Начало месяца
echo $date->startOfMonth(); // 2023-10-01 00:00:00

// Конец месяца
echo $date->endOfMonth();   // 2023-10-31 23:59:59
```

### 14. **Получение дней недели**
```php
$date = Carbon::parse('2023-10-05');

// Получение дня недели (число)
echo $date->dayOfWeek; // 4 (четверг)

// Получение названия дня недели
echo $date->dayName;   // Thursday
```

