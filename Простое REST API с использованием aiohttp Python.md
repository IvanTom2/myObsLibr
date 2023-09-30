Для создания веб-приложений и в частности REST API можно использовать `asyncio`. 

`asyncio` предоставляет функциональность веб-сервера в модуле `web` и при использовании его мы можем определить оконечные точки (маршруты) с помощью класса `RouteTableDef`, который предлагает декоратор, позволяющий задать тип запроса (GET, POST, PUT, DELETE) и строку с именем оконечной точки. Внутри декорированных сопрограмм уже можно определить произвольную логику приложения и возврата данных клиенту. 

Чтобы запустить приложение, его нужно сначала создать - `web.Application`. Добавить маршруты и запустить на каком-либо хосте. Далее к приложению можно обращаться, используя указанные маршруты. К приложению можно обращаться как угодно - браузер, запросы и так далее. 

___
```
from aiohttp import web
from datetime import datetime
from aiohttp.web_request import Request
from aiohttp.web_response import Response


routes = web.RouteTableDef()


@routes.get("/time")
async def time(request: Request) -> Response:
    today = datetime.today()

    result = {
        "month": today.month,
        "day": today.day,
        "time": str(today.time()),
    }

    return web.json_response(result)


if __name__ == "__main__":
    app = web.Application()
    app.add_routes(routes)

    web.run_app(
        app,
        host="127.0.0.1",
        port="8000",
    )

```
___

Relations: [[asyncio Python]] 
Tags: #code
References: [[Asyncio (книга)]] 
Query: 