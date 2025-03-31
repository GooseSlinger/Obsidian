### Вариации использования `os.path`

#### Получение абсолютного пути файла:
```python
import os
path = os.path.abspath('file.txt')
print(path)
```
**Описание:** Возвращает абсолютный путь до файла `file.txt`. Преобразует относительный путь в полный.

---

#### Проверка существования файла или директории:
```python
import os
exists = os.path.exists('file.txt')
print(exists)
```
**Описание:** Проверяет, существует ли файл или папка. Возвращает `True` или `False`.

---

#### Проверка, является ли путь файлом:
```python
import os
is_file = os.path.isfile('file.txt')
print(is_file)
```
**Описание:** Возвращает `True`, если путь ведёт к файлу.

---

#### Проверка, является ли путь директорией:
```python
import os
is_dir = os.path.isdir('my_folder')
print(is_dir)
```
**Описание:** Возвращает `True`, если путь ведёт к директории.

---

#### Получение имени файла из полного пути:
```python
import os
filename = os.path.basename('/path/to/file.txt')
print(filename)
```
**Описание:** Возвращает имя файла без пути — `'file.txt'`.

---

#### Получение директории из полного пути:
```python
import os
directory = os.path.dirname('/path/to/file.txt')
print(directory)
```
**Описание:** Возвращает путь к папке без имени файла.

---

#### Разделение пути на папку и файл:
```python
import os
folder, file = os.path.split('/path/to/file.txt')
print(folder, file)
```
**Описание:** Возвращает кортеж `(путь, файл)`.

---

#### Соединение частей пути:
```python
import os
full_path = os.path.join('/path', 'to', 'file.txt')
print(full_path)
```
**Описание:** Соединяет части пути в один полный путь.

---

#### Получение расширения файла:
```python
import os
root, ext = os.path.splitext('file.txt')
print(root, ext)
```
**Описание:** Делит имя файла на название и расширение.

---

#### Получение размера файла в байтах:
```python
import os
size = os.path.getsize('file.txt')
print(size)
```
**Описание:** Возвращает размер файла в байтах.

---

## Работа с файлами и папками через os

#### Создание новой папки:
```python
import os
os.mkdir('new_folder')
```
**Описание:** Создаёт папку `new_folder` в текущей директории. Если папка уже есть — будет ошибка.

---

#### Рекурсивное создание вложенных папок:
```python
import os
os.makedirs('parent_folder/child_folder')
```
**Описание:** Создаёт папки по пути, включая все вложенные.

---

#### Удаление файла:
```python
import os
os.remove('file.txt')
```
**Описание:** Удаляет файл `file.txt`. Если файла нет — ошибка.

---

#### Удаление пустой папки:
```python
import os
os.rmdir('empty_folder')
```
**Описание:** Удаляет папку, но только если она пустая.

---

#### Рекурсивное удаление папки со всем содержимым:
```python
import shutil
shutil.rmtree('folder_to_delete')
```
**Описание:** Удаляет папку и всё, что в ней находится.

---

#### Получение списка файлов и папок в директории:
```python
import os
items = os.listdir('.')
print(items)
```
**Описание:** Возвращает список файлов и папок в текущей директории.

---

#### Проход по всем папкам и файлам внутри директории:
```python
import os
for root, dirs, files in os.walk('.'):
    print('Папка:', root)
    print('Подпапки:', dirs)
    print('Файлы:', files)
```
**Описание:** Рекурсивно обходит все папки и файлы внутри заданной директории.
