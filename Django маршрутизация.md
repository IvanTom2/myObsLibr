___
Маршрутизация может происходить прямым и многоуровневым образом. 
Маршрутизация происходит в папке конфигурации проекта в модуле urls.py в переменной urlpatterns, которая представляет из себя список.

Здесь можно делать напрямую: добавлять желаемый url-путь и соответствующий ему контроллер. Например, 1)

Либо с помощью нескольких уровней. В таком случае, указывается часть url-пути, которая будет обрабатываться пулом контроллеров приложения и маршрутизироваться уже по списку маршрутов уровня проекта. Например, 2)
Для этого нужно создать в директории приложения файл urls.py и прописать urlpatterns уже там!
___
[[Code]]:
```
1) 
from django.urls import path
from app_name.views import view_name

urlpatterns = [
path("path/", view_name),
]

2) 
2.1) -> project_name.urls
from django.urls import path, include

urlpatterns = [
path("path/", include("app_name.urls"))
]

2.2) -> app_name.urls
from django.urls import path
from .views import view_name

urlpatterns = [
path("", view_name)
]

```
___
Language: [[Python]]
Key-words:  [[Django py]]
Question query?: Маршрутизация url-адресов (юрл) в Django (джанго)