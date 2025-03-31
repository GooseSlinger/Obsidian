## CLI генератор FastAPI проекта на Typer

```python
import os
import typer

app = typer.Typer()
BASE_DIR = os.path.dirname(os.path.abspath(__file__))

# Функция создания папки с __init__.py
def create_folder_with_init(path, is_database=False):
    os.makedirs(path, exist_ok=True)
    init_path = os.path.join(path, '__init__.py')
    if not os.path.exists(init_path):
        with open(init_path, 'w') as f:
            if is_database:
                # Для database автоматически подключаем базовые объекты
                f.write("from .database import SessionLocal, engine, Base\n")
            else:
                f.write("# init file\n")

# Проверка на существование файла
def check_file_exists(file_path):
    if os.path.exists(file_path):
        typer.echo(f"❌ Файл уже существует: {file_path}")
        raise typer.Exit()

# Команда создания структуры проекта
@app.command()
def make_project():
    folders = ["models", "schemas", "routes", "service", "database"]
    for folder in folders:
        path = os.path.join(BASE_DIR, folder)
        if not os.path.exists(path):
            os.makedirs(path)
            typer.echo(f"✅ Папка {folder} создана")
            create_folder_with_init(path, is_database=(folder == "database"))
        else:
            typer.echo(f"⚠️ Папка {folder} уже существует")

    # Создание файла базы данных
    db_file = os.path.join(BASE_DIR, "database", "database.py")
    if not os.path.exists(db_file):
        with open(db_file, "w") as f:
            f.write(
                "from sqlalchemy import create_engine\n"
                "from sqlalchemy.ext.declarative import declarative_base\n"
                "from sqlalchemy.orm import sessionmaker\n\n"
                "SQLALCHEMY_DATABASE_URL = 'sqlite:///./database/base.db'\n\n"
                "engine = create_engine(SQLALCHEMY_DATABASE_URL, connect_args={\"check_same_thread\": False})\n"
                "SessionLocal = sessionmaker(autocommit=False, autoflush=False, bind=engine)\n"
                "Base = declarative_base()\n"
            )
        typer.echo("✅ Файл database.py создан")
    else:
        typer.echo("⚠️ Файл database.py уже существует")

# Команда создания модели
@app.command()
def make_model(name: str):
    path = os.path.join(BASE_DIR, "models")
    create_folder_with_init(path)
    file_path = os.path.join(path, f"{name}.py")
    check_file_exists(file_path)

    with open(file_path, "w") as f:
        f.write(
            "from database.database import Base\n"
            "from sqlalchemy import Column, Integer, String\n\n"
            f"class {name}(Base):\n"
            f"    __tablename__ = '{name.lower()}s'\n"
            f"    id = Column(Integer, primary_key=True, index=True)\n"
            f"    name = Column(String, index=True)\n"
        )
    typer.echo(f"✅ Модель {name} создана")

# Команда создания схемы
@app.command()
def make_schema(name: str):
    path = os.path.join(BASE_DIR, "schemas")
    create_folder_with_init(path)
    file_path = os.path.join(path, f"{name}.py")
    check_file_exists(file_path)

    with open(file_path, "w") as f:
        f.write(
            "from pydantic import BaseModel\n\n"
            f"class {name}(BaseModel):\n"
            f"    name: str\n"
        )
    typer.echo(f"✅ Схема {name} создана")

# Команда создания роутера
@app.command()
def make_rout(name: str):
    path = os.path.join(BASE_DIR, "routes")
    create_folder_with_init(path)
    file_path = os.path.join(path, f"{name}.py")
    check_file_exists(file_path)

    with open(file_path, "w") as f:
        f.write(
            "from fastapi import APIRouter\n\n"
            "router = APIRouter()\n\n"
            f"@router.get('/{name}')\n"
            f"async def get_{name}():\n"
            f"    return {{'message': 'Это роут {name}'}}\n"
        )
    typer.echo(f"✅ Путь {name} создан")

# Команда создания сервиса
@app.command()
def make_service(name: str):
    path = os.path.join(BASE_DIR, "service")
    create_folder_with_init(path)
    file_path = os.path.join(path, f"{name}.py")
    check_file_exists(file_path)

    with open(file_path, "w") as f:
        f.write(
            f"class {name.capitalize()}Service:\n"
            f"    def __init__(self):\n"
            f"        pass\n\n"
            f"    def example_method(self):\n"
            f"        return 'Hello from {name.capitalize()}Service'\n"
        )
    typer.echo(f"✅ Сервис {name} создан")

# Запуск CLI
if __name__ == "__main__":
    app()
