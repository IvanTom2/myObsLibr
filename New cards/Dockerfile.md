Dockerfile - это конфигурационный файл с инструкциями по созданию Docker-образов. Почти каждая команда создает новый слой в образе. Из Dockerfile создаются образы, из образов создаются сами контейнеры. 
![[Pasted image 20231126094101.png]]

***Важно!!!*** При создании Dockerfile если не указывать версии ПО, то по дефолту будет использоваться `latest`, что может привести к непредсказуемым результатами.  

Содержание Dockerfile:
1. Базовое изображение (например ubuntu). 


___
```
docker build . -t imagename:v0.1
```
___
Зачем это нужно: [[]] 
Примеры применения: [[]] 
Основные ошибки: [[]]
___
Relations: [[Docker]] 
Tags: #code
References: 
Query: 