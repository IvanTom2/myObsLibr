___
Административный веб-сайт Django - это сайт, на котором можно управлять проектом Django. 

Для начала необходимо создать супер-пользователя. Затем - добавить приложение в файл admin.py.
___
[[Code]]:
```
1. Создать супер-пользователя
# bash
python3 -m manage createsuperuser
...

2. Зарегистрировать приложение
# {application}/admin.py
from django.contrib import admin
from {application}.models import ModelName

admin.site.register(ModelName)
```
___
Language: [[Python]]
Key-words:  [[Django py]]
Question query?: Как зарегистрировать приложение в административном веб-сайте Django (Джанго)