## Установка
В корне вашего проекта выполните следующую команду Alembic:
```bash
alembic init alembic
pymysql # для работы с mysql
```

## Подключение
После инициализации появится файл в корне `alembic.ini`  в нем нужно найти строчку и прописать туда подключение к бд:
```ini
sqlalchemy.url = mysql+pymysql://root:password@localhost:3306/mydatabase
```

## Правка `env.py`
В файле `alembic/env.py` найдите:
```python
# target_metadata = None
```
и замените на:
```python
from database.database import Base

target_metadata = Base.metadata
```

## Генерация миграции
```bash
alembic revision --autogenerate -m "init"
```

## Применяем миграции (создаём таблицы в базе)
```bash
alembic upgrade head
```
Это применит свежесозданную миграцию (в твоём случае `init`) и создаст таблицы в базе данных `pyapi`.

## Команды
```bash
alembic current # Показать текущую версию миграции
alembic history # Посмотреть историю всех миграций
alembic downgrade -1 #Откатить одну миграцию назад
alembic downgrade base # Откатить ВСЕ миграции
alembic upgrade +1 # Применить на одну миграцию вперёд
```
