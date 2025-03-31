### Документация по модулю `datetime`

#### Импортирование модуля:
```python
import datetime
```
**Описание:** Импортирует модуль `datetime` для работы с датой и временем.

---

#### Получение текущей даты и времени:
```python
from datetime import datetime
now = datetime.now()
print(now)
```
**Описание:** Возвращает текущую дату и время. Результат — объект `datetime`.

---

#### Получение только текущей даты:
```python
from datetime import date
today = date.today()
print(today)
```
**Описание:** Возвращает только дату без времени.

---

#### Получение текущего времени:
```python
from datetime import datetime
current_time = datetime.now().time()
print(current_time)
```
**Описание:** Возвращает текущее время без даты.

---

#### Создание объекта даты:
```python
from datetime import date
d = date(2024, 3, 21)
print(d)
```
**Описание:** Создаёт объект даты `2024-03-21`. Принимает год, месяц, день.

---

#### Создание объекта времени:
```python
from datetime import time
t = time(14, 30, 15)
print(t)
```
**Описание:** Создаёт объект времени `14:30:15`. Принимает часы, минуты, секунды.

---

#### Создание полной даты и времени:
```python
from datetime import datetime
dt = datetime(2024, 3, 21, 14, 30, 15)
print(dt)
```
**Описание:** Создаёт объект `datetime` с датой и временем.

---

#### Форматирование даты и времени в строку:
```python
from datetime import datetime
now = datetime.now()
formatted = now.strftime("%Y-%m-%d %H:%M:%S")
print(formatted)
```
**Описание:** Преобразует объект `datetime` в строку по заданному формату.

---

#### Разбор строки в дату:
```python
from datetime import datetime
date_str = '2024-03-21 14:30:15'
dt = datetime.strptime(date_str, '%Y-%m-%d %H:%M:%S')
print(dt)
```
**Описание:** Преобразует строку в объект `datetime` по заданному формату.

---

#### Разница между датами (timedelta):
```python
from datetime import date
d1 = date(2024, 3, 21)
d2 = date(2024, 3, 25)
delta = d2 - d1
print(delta.days)
```
**Описание:** Вычисляет разницу между двумя датами. Возвращает объект `timedelta`, количество дней можно получить через `.days`.

---

#### Использование timedelta для вычислений:
```python
from datetime import datetime, timedelta
now = datetime.now()
future = now + timedelta(days=7)
print(future)
```
**Описание:** Добавляет 7 дней к текущей дате и времени.

---

#### Получение компонентов даты и времени:
```python
from datetime import datetime
now = datetime.now()
print(now.year, now.month, now.day)
print(now.hour, now.minute, now.second)
```
**Описание:** Выводит отдельные элементы даты и времени.

---

#### Получение дня недели:
```python
from datetime import datetime
now = datetime.now()
print(now.weekday())  # 0 - понедельник, 6 - воскресенье
```
**Описание:** Возвращает номер дня недели (0 — понедельник).

---

#### Получение ISO формата даты:
```python
from datetime import datetime
now = datetime.now()
print(now.isoformat())
```
**Описание:** Возвращает дату и время в формате ISO 8601.

---

#### Проверка високосного года (через calendar):
```python
import calendar
print(calendar.isleap(2024))
```
**Описание:** Проверяет, является ли 2024 год високосным. Возвращает `True` или `False`.
