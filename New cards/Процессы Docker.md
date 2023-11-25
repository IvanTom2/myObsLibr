Запустить контейнер Docker можно с помощью `start`. 

При имеющихся запущенных Docker процессах возникает необходимость исследования что за контейнер запущен и сколько ресурсов он потребляет, с какими ошибками сталкивается. 
1. `docker inspect [container id; name]` - показывает Docker контейнер запущенного процесса. 
2. `docker stats [container id; name]` - показывает, сколько ресурсов потребляет запущенный процесс. 
3. `docker logs [container id; name]`. 

___
```
// запустить процесс
docker start [container id; name]
// запуск процесс в фоновом режиме
docker start -d [container id; name]

// поставить процесс на паузу
docker pause [container id; name]
docker unpause [container id; name]

// остановка процесса
docker stop [container id; name]
// грубая остановка процесса
docker kill [container id; name]

// для отображения логов и live-логов
docker logs [container id; name]
docker -f logs [container id; name]

```
___
Зачем это нужно: [[]] 
Примеры применения: [[]] 
Основные ошибки: [[]] 
___
Relations: [[Docker]] [[Контейнеры Docker]] 
Tags: #code
References: 
Query: 