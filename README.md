# ЛР: локальный приватный Docker Registry

## Выполнение лабораторной работы

### 1) Подъем локального registry

```bash
docker run -d -p 5000:5000 --name registry registry:2
```
Разбор команды:  
**-d** — запуск в фоновом режиме (detached)  
**-p 5000:5000** — проброс порта 5000 хоста на порт 5000 контейнера  
**--name registry** — имя контейнера  
**registry:2** — образ и версия  

<img width="1457" height="159" alt="image" src="https://github.com/user-attachments/assets/cf7da81a-efc1-439a-97ac-e21ee6285c7f" />

### 2) Проверка registry по API
```bash
curl http://localhost:5000/v2/_catalog
```
Разбор команды:  
**curl** — утилита для отправки HTTP-запросов  
**http://localhost:5000** — адрес локального registry (хост:порт)  
**/v2/** — версия API Docker Registry v2  
**_catalog** — специальный эндпоинт, возвращающий список всех репозиториев в registry  

<img width="651" height="111" alt="image" src="https://github.com/user-attachments/assets/c61bca42-b076-4161-b56b-5fa97ba3aaa6" />

### 3) Мини-проект
Создадим папку web-demo-ivutpa и файлы index.html и Dockerfile.
<img width="889" height="255" alt="image" src="https://github.com/user-attachments/assets/bfdaabcf-5a8b-46c7-a4bf-7dcd451e5f90" />  
<img width="697" height="97" alt="image" src="https://github.com/user-attachments/assets/0762ac38-d69a-4244-a269-a4b375e3779a" />  

### 4) Сборка образа
Соберем образ с помощью команды:
```bash
docker build -t web-demo-ivutpa:1.0 .
```

Разбор команды:  
**docker build** — команда для сборки образа из Dockerfile  
**-t** — флаг --tag - присвоить имя и тег образу  
**web-demo-ivutpa:1.0**	— имя образа: web-demo-ivutpa, тег: 1.0 (версия)  
**.**	- контекст сборки — текущая папка  

Образ появился.
<img width="1062" height="102" alt="image" src="https://github.com/user-attachments/assets/2c074dd6-b223-425e-840e-0dfb6d492b5d" />

### 5) Привязка образа к registry через tag
Перетегируем образ с помощью команды:
```bash
docker tag web-demo-ivutpa:1.0 localhost:5000/web-demo-ivutpa:1.0
```
Разбор команды:  
tag — создать псевдоним образа  
web-demo-ivutpa:1.0 — исходный локальный образ  
localhost:5000/web-demo-ivutpa:1.0 — новый тег с адресом registry  

<img width="798" height="73" alt="image" src="https://github.com/user-attachments/assets/b4e89137-1a77-4780-a057-0e6e0eb72bc4" />

### 6) Push в registry
Запушим образ в registry с помощью команды:
```bash
docker push localhost:5000/web-demo-ivutpa:1.0
```
<img width="1161" height="283" alt="image" src="https://github.com/user-attachments/assets/ca0915ca-66d5-490c-bd2c-5c024bac9276" />

### 7) Проверка, что образ реально в registry
Проверим список репозиториев командой:
```bash
curl http://localhost:5000/v2/_catalog
```
<img width="885" height="158" alt="image" src="https://github.com/user-attachments/assets/b2b1b31b-ff69-47cd-b2af-f93b1b716e83" />  

Проверим список тегов:
```bash
curl http://localhost:5000/v2/web-demo-ivutpa/tags/list
```
<img width="1088" height="147" alt="image" src="https://github.com/user-attachments/assets/6517ad56-367d-4c7c-a28f-128b1f936301" />

### 8) Проверка pull
Удалим только тег с localhost:
```bash
docker rmi localhost:5000/web-demo-ivutpa:1.0
```
<img width="1354" height="77" alt="image" src="https://github.com/user-attachments/assets/51c09f9b-9136-4f43-aa58-e659bf6287bc" />  

Образ пропал:
<img width="1081" height="122" alt="image" src="https://github.com/user-attachments/assets/66911609-8f99-4fba-8efd-b43cad83367d" />  

Теперь выполним pull из registry:
```bash
docker pull localhost:5000/web-demo-ivutpa:1.0
```
<img width="1004" height="117" alt="image" src="https://github.com/user-attachments/assets/e4b67ed2-0942-41f6-bb1c-9381992bac93" />  

Образ снова появился:  
<img width="1073" height="145" alt="image" src="https://github.com/user-attachments/assets/eb2a6064-24c7-439e-8ebc-3cb73ff66a45" />

### 9) Проверка запуском контейнера
Запустим контейнер из образа, скачанного из registry:
```bash
docker run -d -p 8080:80 --name web-1 localhost:5000/web-demo-ivutpa:1.0
```
<img width="1319" height="54" alt="image" src="https://github.com/user-attachments/assets/a70be394-9098-4830-bc87-b9a8b2b3c088" />  

Проверим, что контейнер работает:
<img width="1878" height="117" alt="image" src="https://github.com/user-attachments/assets/730f5d61-3628-43ba-8911-6636d3b96937" />  

Откроем браузер и перейдем по адресу http://localhost:8080:  
<img width="422" height="282" alt="image" src="https://github.com/user-attachments/assets/828c4aae-e810-495d-a6e1-520424b69191" />   
(Проблема с кодировкой, но все работает)






