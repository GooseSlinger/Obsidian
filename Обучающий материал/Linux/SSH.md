### 1. Установка OpenSSH Server

Если OpenSSH Server не установлен, выполните:
```bash
sudo apt update
sudo apt install openssh-server
sudo systemctl status ssh
```

### 2. Настройка SSH

Основной конфигурационный файл SSH находится по пути `/etc/ssh/sshd_config`. Откройте его для редактирования:
```bash
sudo nano /etc/ssh/sshd_config
```

Иногда некоторые порты не открыты и их нужно открыть для этого пишем:
```bash
sudo ufw allow 2222/tcp
sudo ufw status
```

