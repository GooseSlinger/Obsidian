Установка:
```bash
pip install sqlalchemy
```
подключение в отдельный файл database.py:
```python
from sqlalchemy import create_engine
from sqlalchemy.orm import sessionmaker, declarative_base

DATABASE_URL = 'sqlite:///database./base.db'  # Меняешь на MySQL 
DATABASE_URL = 'SQLALCHEMY_DATABASE_URL = "mysql+pymysql://username:password@server_name/database_name?driver=ODBC+Driver+17+for+SQL+Server"'

engine = create_engine(
    DATABASE_URL, 
    connect_args={"check_same_thread": False}  # Только для SQLite нужно
)

SessionLocal = sessionmaker(autocommit=False, autoflush=False, bind=engine)
Base = declarative_base()
```
подключение в main.py:
```python
# импортируем из бд
from database import SessionLocal, engine, Base

# собственно это запуск всего в бд
Base.metadata.create_all(bind=engine)
```
