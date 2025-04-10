Чтобы закинуть файл на удалённый сервер через SSH, можно использовать утилиту `scp` (secure copy). Эта команда позволяет безопасно копировать файлы между локальной машиной и удалённым сервером, используя протокол SSH для передачи данных.

---
### **Синтаксис команды `scp`:**
```bash
scp [опции] исходный_файл пользователь@хост:путь_к_назначению
```
- **исходный_файл:** Путь к файлу, который нужно скопировать.
- **пользователь:** Имя пользователя на удалённом сервере.
- **хост:** Адрес удалённого сервера (IP-адрес или доменное имя).
- **путь_к_назначению:** Путь на удалённом сервере, куда будет скопирован файл.

---
### **Примеры использования `scp`:**

#### 1. **Копирование одного файла на сервер:**
```bash
scp /path/to/local/file.txt пользователь@хост:/path/to/remote/directory/
```
- `/path/to/local/file.txt`: Путь к файлу на вашей локальной машине.
- `пользователь@хост`: Указание пользователя и адреса сервера.
- `/path/to/remote/directory/`: Путь к директории на сервере, куда нужно скопировать файл.
**Пример:**
```bash
scp document.txt user@example.com:/home/user/
```

#### 2. **Копирование нескольких файлов на сервер:**
Используйте маску `*` для выбора нескольких файлов:
```bash
scp /path/to/local/*.txt пользователь@хост:/path/to/remote/directory/
```
**Пример:**
```bash
scp *.txt user@example.com:/home/user/documents/
```

#### 3. **Копирование целой директории на сервер:**
```bash
scp -r /path/to/local/directory пользователь@хост:/path/to/remote/directory/
```
**Пример:**
```bash
scp -r my_project/ user@example.com:/var/www/html/
```

#### 4. **Копирование файла с сервера на локальную машину:**
```bash
scp пользователь@хост:/path/to/remote/file.txt /path/to/local/directory/
```
**Пример:**
```bash
scp user@example.com:/home/user/document.txt ~/Downloads/
```

#### 5. **Использование нестандартного порта SSH:**
```bash
scp -P порт /path/to/local/file.txt пользователь@хост:/path/to/remote/directory/
```
**Пример:**
```bash
scp -P 2222 document.txt user@example.com:/home/user/
```

---
### **Дополнительные опции `scp`:**

- **`-p`:** Сохранить метки времени файла (timestamp).
- **`-q`:** Тихий режим (нет вывода прогресса).
- **`-C`:** Включить сжатие данных при передаче.
- **`-v`:** Вывести подробную информацию о процессе передачи.