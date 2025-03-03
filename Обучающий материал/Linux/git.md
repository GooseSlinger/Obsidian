### 1. Установка Git
```bash
sudo apt install git -y
```

### 2. Настройка Git
```bash
git config --global user.name "Ваше Имя"
git config --global user.email "ваш_email@пример.com"
git config --global core.autocrlf input
git config --global core.safecrlf warn
git config --global core.quotepath off
git config --list
```

### 3. Генерация SSH-ключа

1. **Проверка наличия существующих ключей**
```bash
ls -la ~/.ssh
```
Если вы видите файлы `id_rsa` или `id_ed25519`, это означает, что ключ уже существует.

2. **Генерация нового SSH-ключа**
```bash
ssh-keygen -t ed25519 -C "ваш_email@пример.com"
```

### 4. Просмотр ключа

```bash
cat ~/.ssh/id_ed25519.pub
```

