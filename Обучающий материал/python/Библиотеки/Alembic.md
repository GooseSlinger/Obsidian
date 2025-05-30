## 🧰 1. Установка Alembic
```bash
pip install alembic
```
## 📁 2. Инициализация Alembic
Выполни это в корне своего проекта:
```bash
alembic init alembic
```
## 📄 3. Настройка `alembic.ini` и `env.py`

### ✅ `alembic.ini`

Убедись, что в файле `alembic.ini` указан правильный `script_location` (по умолчанию он указывает на созданную папку `alembic`):
```ini
script_location = alembic
```

### ✅ `alembic/env.py`

В файле `alembic/env.py` нужно подключить твой `SQLAlchemy Base` и `engine`.

Также проверь строку подключения к БД:
```python
from your_project.database import engine  # например

def run_migrations_online():
    connectable = engine
    with connectable.connect() as connection:
        context.configure(
            connection=connection,
            target_metadata=target_metadata
        )
        with context.begin_transaction():
            context.run_migrations()
```
## 📝 4. Создание миграции

После того как ты изменил модель (например, добавил новое поле), создай миграцию:
```bash
alembic revision --autogenerate -m "migration_init"
```
- `--autogenerate` — попытается автоматически определить разницу между моделями и текущей схемой.
- `-m` — комментарий к миграции.
## 🔧 5. Применение миграции

Чтобы применить миграцию к БД:
```bash
alembic upgrade head
```
## 🔙 6. Откат миграции

Откат к предыдущей версии:
```bash
alembic downgrade -1
```
Или к конкретной версии:
```bash
alembic downgrade abcdef123456
```
## 📋 7. Просмотр истории миграций
```bash
alembic history
```
## 🧪 8. Полезные команды

| Команда                    | Описание                                     |
| -------------------------- | -------------------------------------------- |
| `alembic current`          | Показать текущую версию БД                   |
| `alembic heads`            | Последняя миграция                           |
| `alembic branches`         | Показать ветки миграций                      |
| `alembic stamp <revision>` | Установить указатель версии без изменений БД |

---

## 💡 Советы

- Не забывай делать `--autogenerate`, чтобы Alembic мог сравнить модели с БД.
- Автогенерация не всегда идеальна — проверяй сгенерированный код.
- Храни миграции в системе контроля версий (Git).
- Для тестирования можно использовать отдельную БД.