## Базовые команды

1. **Просмотр работающих контейнеров**
```bash
docker ps
```

2. **Удаление контейнера**
```bash
docker rm CONTAINER_ID/NAME
```

3. **Для удаления всех остановленных контейнеров**
```bash
docker container prune
```

4. **Просмотр логов контейнера**
```bash
docker logs CONTAINER_ID/NAME
```

5. **Подключение к работающему контейнеру**
```bash
docker exec -it CONTAINER_ID/NAME /bin/bash
```

## Работа с сетями

6. **Список сетей:**
```bash
docker network ls
```

7. **Создание новой сети**
```bash
docker network create NETWORK_NAME
```

8. **Подключение контейнера к сети**
```bash
docker network connect NETWORK_NAME CONTAINER_ID/NAME
```

## Работа с томами

9. **Список томов**
```bash
docker volume ls
```

10. **Удаление тома**
```bash
docker volume rm VOLUME_NAME
```

11. **Удаление всех неиспользуемых томов**
```bash
docker volume prune
```

## Очистка

12. **Удаление всех неиспользуемых объектов**
```bash
docker system prune -a
```
