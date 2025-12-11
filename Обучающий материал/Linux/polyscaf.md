# Сборка `polyscaf`

1. Установи pipx
```bash
brew install pipx
pipx ensurepath
```
2. Делаем глобальный путь для компьютера
```bash
pipx install /Users/goosesl1nger/Desktop/projects/Python/py_tools/cli

# Усиленая версия
pipx install /Users/goosesl1nger/Desktop/projects/Python/py_tools/cli
```
3. Проверяем работу
```bash
polyscaf --help
``` 

4. для билда на всякий
```bash
python -m build
```


# Переустановка

1. Удаление пакета
```bash
pipx uninstall polyscaf
```
2. Усиленая установка пакета
```bash
pipx install --force /Users/goosesl1nger/Desktop/projects/Python/py_tools/cli
```