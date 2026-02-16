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

### 2) Проверка registry по API
```bash
curl http://localhost:5000/v2/_catalog
```
Разбор команды:
curl - утилита для отправки HTTP-запросов  
http://localhost:5000 - адрес локального registry (хост:порт)  
/v2/ - версия API Docker Registry v2  
_catalog - специальный эндпоинт, возвращающий список всех репозиториев в registry  

<img width="651" height="111" alt="image" src="https://github.com/user-attachments/assets/c61bca42-b076-4161-b56b-5fa97ba3aaa6" />




