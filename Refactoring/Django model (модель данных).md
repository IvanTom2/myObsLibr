___
Модель данных реализуется через ORM паттерн (Object Related Model). В качестве объектов используются классы. Каждый класс - это определенная таблица. Атрибут класса - это столбец. А каждый экземпляр класс - это строка. 

Для того, чтобы обновить базу данных в соответствии с новыми или измененными моделями данных, необходимо СДЕЛАТЬ МИГРАЦИЮ ДАННЫХ. По сути миграция - это ALTER TABLE и добавление новых таблиц, т.е. DDL (definition) команды (CREATE, ALTER, DROP). 
___
[[Code]]:
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
Language: [[Python]]
Key-words:  [[Django py]]
Question query?: модель данных в Django (Джанго)