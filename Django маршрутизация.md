Маршрутизация может происходить прямым и многоуровневым образом. 
Маршрутизация происходит в папке конфигурации проекта в модуле `urls.py` в переменной `urlpatterns`, которая представляет из себя список. 

Здесь можно делать напрямую: добавлять желаемый URL и соответствующий ему контроллер. 

Либо с помощью нескольких уровней. В таком случае, указывается часть URL, которая будет обрабатываться пулом контроллеров приложения и маршрутизироваться уже по списку маршрутов уровня проекта. Для этого нужно создать в директории приложения файл `urls.py` и прописать `urlpatterns` в нем. 

___
```
I. Прямая маршрутизация в корневой urls.py
from django.urls import path
from app_name.views import view_name

urlpatterns = [
path("path/", view_name),
]

II. Многоуровневая маршрутизация
2.1) -> {project_name}.urls
from django.urls import path, include

urlpatterns = [
path("path/", include("{app_name}.urls"))
]

2.2) -> {app_name}.urls
from django.urls import path
from .views import view_name

urlpatterns = [
path("", view_name)
]
```
___
Relations: [[Django Python]] 
Tags: #code
References: 
Query: Маршрутизация URL в Django