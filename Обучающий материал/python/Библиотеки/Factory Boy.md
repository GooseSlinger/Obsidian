## Что такое Factory Boy?

**Factory Boy** - это библиотека Python для удобного создания тестовых данных. Она позволяет:
- Генерировать фиктивные данные для моделей
- Создавать связанные объекты (отношения между моделями)
- Настраивать различные состояния объектов для тестов
- Использовать шаблоны для повторного использования кода

**Зачем нужен Factory Boy в FastAPI + SQLAlchemy?**
- Упрощает создание тестовых данных для unit-тестов
- Позволяет легко поддерживать тесты при изменении моделей
- Снижает количество шаблонного кода в тестах
- Обеспечивает согласованность тестовых данных

## Краткий гайд по использованию Factory Boy с FastAPI и SQLAlchemy

### 1. Установка
```bash
pip install factory-boy
```

### 2. Базовый пример

Предположим, у нас есть модель SQLAlchemy:

```python
# models.py
from sqlalchemy import Column, Integer, String
from sqlalchemy.ext.declarative import declarative_base

Base = declarative_base()

class User(Base):
    __tablename__ = 'users'
    
    id = Column(Integer, primary_key=True)
    username = Column(String(50), unique=True)
    email = Column(String(100))
    is_active = Column(Boolean, default=True)
```

Создаем фабрику для этой модели:
```python
# factories.py
import factory
from factory.alchemy import SQLAlchemyModelFactory
from models import User, Base
from database import SessionLocal

class UserFactory(SQLAlchemyModelFactory):
    class Meta:
        model = User
        sqlalchemy_session = SessionLocal()
        sqlalchemy_session_persistence = "commit"  # или "flush"

	# это с факером
    username = factory.Faker('user_name')
    email = factory.Faker('email')
    is_active = True

	# Это прямой загон
	name = factory.Iterator([
		'salads',
		'soups',
		'mainCourses',
		'sides',
		'drinks',
		'healthy',
		'bakery'
	])
```

### 3. Создаем скрипт
```python
from database import SessionLocal, engine
from models import FoodCategory
from database.factories.StartDataFactory import StartDataFactory

def load_initial_data():
	db = SessionLocal()
	try:
		existing = db.query(FoodCategory).count()
		if existing > 0:
			print("Стартовые данные уже существуют")
			return
		StartDataFactory.create_batch(7)
		print("Данные загружены успешно")
	except Exception as e:
		print(f"Произошла ошибка в загрузке данных: {e}")
		db.rollback()
	finally:
		db.close()

if __name__ == "__main__":
	load_initial_data()
```

### 3. Запуск скрипта
```bash
python -m scripts.status_order_script
```
не забываем добавить -m а то будут танцы с бубном