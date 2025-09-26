# Mutagen — полный шпаргалка-гид

Инструмент для **живой синхронизации файлов** через SSH/Docker.  
В этом гайде: установка, подготовка SSH, ручные сессии, Projects (`mutagen.yml`) и троблшутинг. 

───────────────────────────────────
## Установка и демон

**Установка**
```bash
# macOS (Homebrew)
brew install mutagen-io/mutagen/mutagen

# Linux (webinstall)
curl -sS https://webi.sh/mutagen | sh

# Проверка
mutagen version   # показать версию
```

**Демон Mutagen (фоновый процесс)**
- `mutagen daemon start` — запустить демон (обязателен для работы сессий).
- `mutagen daemon stop` — остановить демон (сессии «засыпают», файлы не трогаются).
- `mutagen daemon register` — включить автозапуск демона при входе в систему.
- `mutagen daemon unregister` — отключить автозапуск.

───────────────────────────────────
## Подготовка SSH (рекомендуется)

**Конфиг SSH-алиаса (`~/.ssh/config`)**
```yml
# Это НЕ файл проекта Mutagen — это OpenSSH config (sshconfig).
Host work
  HostName 10.11.7.251
  User administrator
  Port 22
  IdentityFile ~/.ssh/id_rsa
  IdentitiesOnly yes
```

───────────────────────────────────
## Ручные сессии синхронизации (scp-стиль адреса) — РЕКОМЕНДОВАНО

**Создать сессию с игнорами**
```bash
mutagen daemon start
mutagen sync create --name=news \
  -i 'bin' -i 'tmp' -i 'node_modules' -i '*.log' -i '.DS_Store' \
  -i 'mutagen.yml' -i 'mutagen.yaml' -i '.air.toml' -i '.vscode' -i '.idea' -i '*.swp' \
  /Users/<you>/Desktop/projects/tonar/news \
  work:~/projects/news
```

**Статус и мониторинг**
```bash
mutagen sync list            # краткий статус всех сессий
mutagen sync list --long     # подробности (настройки, игноры, проблемы)
mutagen sync monitor         # «живой» лог событий (Ctrl+C для выхода)
```

**Управление сессией**
```bash
mutagen sync flush news      # принудительно дожать цикл синка
mutagen sync pause news      # пауза
mutagen sync resume news     # продолжить
mutagen sync reset news      # сбросить историю синка (как новая сессия)
mutagen sync terminate news  # удалить сессию (файлы на концах не удаляются)
```

**Рабочий цикл «по запросу»**
```bash
# старт дня
mutagen daemon start
# ...работа, изменения синкаются автоматически...
# при необходимости
mutagen sync flush news
# завершение дня
mutagen daemon stop
```

───────────────────────────────────
## Projects (оркестрация через `mutagen.yml`, опционально)

**Файл `mutagen.yml` (в корне проекта; используем scp-стиль endpoint)**
```yml
sync:
  news:
    alpha: .
    beta: work:~/projects/news
    mode: two-way-resolved
    flushOnCreate: true
    ignore:
      vcs: true
      paths:
        - bin
        - tmp
        - node_modules
        - "*.log"
        - ".DS_Store"
        - "mutagen.yml"
        - "mutagen.yaml"
        - ".air.toml"
        - ".vscode"
        - ".idea"
        - "*.swp"
```

**Команды проекта**
```bash
mutagen project start       # создать и запустить все сессии проекта
mutagen project list        # статус всех проектных сессий
mutagen project flush       # форс-синк всех sync-сессий проекта
mutagen project pause       # пауза всех сессий проекта
mutagen project resume      # продолжить все сессии проекта
mutagen project terminate   # остановить и удалить все сессии проекта
```

───────────────────────────────────
## Троблшутинг

```bash
# «Старая/залипшая» конфигурация → чистый рестарт Mutagen
mutagen sync terminate -a || true
mutagen project terminate || true
mutagen daemon stop || true
mutagen daemon start
```
