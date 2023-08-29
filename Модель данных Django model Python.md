Модель данных реализуется через ORM паттерн (Object Related Model). В качестве объектов используются классы. Каждый класс - это определенная таблица. Атрибут класса - это столбец. А каждый экземпляр класс - это строка. 

Для того, чтобы обновить базу данных в соответствии с новыми или измененными моделями данных, выполнить ***миграцию данных***. По сути миграция - это `ALTER TABLE` и добавление новых таблиц, иными словами DDL команды `CREATE, ALTER, DROP`. 

___
```
from django.db import models

Item(models.Model)
"""Some Description"""
	text = models.TextField(default='') # это будущий столбец
	(...)


After model processing:
python3 -m manage makemigrations
```
___
Relations: [[Django Python]] 
Tags: #code
References: 
Query: Модель данных в Django