# ЛР: локальный приватный Docker Registry

## Выполнение лабораторной работы

### 1) Подъем локального registry

```bash
docker run -d -p 5000:5000 --name registry registry:2
```
Разбор команды:

-d - запуск в фоновом режиме (detached)

-p 5000:5000 - проброс порта 5000 хоста на порт 5000 контейнера

--name registry - имя контейнера

registry:2 - образ и версия

<img width="1457" height="159" alt="image" src="https://github.com/user-attachments/assets/cf7da81a-efc1-439a-97ac-e21ee6285c7f" />

