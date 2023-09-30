Традиционно клиент посылает серверу HTTP-запрос и сервер отвечает на этот запрос, отправляя какой-то контент. На этом транзакция заканчивается. 

Мы могли бы сделать динамически-обновляемый контент с помощью кода JavaScript, но это усложняет процесс разработки и несет дополнительную нагрузку для сервера. 

Например, счетчик количества пользователей. Код JavaScript может постоянно опрашивать сервер о количестве пользователей. В это же время сокет может выполнять такую же функцию, только отправляя данные самостоятельно - нет нужды его об этом спрашивать. 

Для создания оконечной точки типа WebSocket нужно унаследовать класс `WebSocketEndpoint` и реализовать в подклассе несколько сопрограмм. 

___
```
import asyncio
from starlette.applications import Starlette
from starlette.endpoints import WebSocketEndpoint
from starlette.routing import WebSocketRoute
from starlette.routing import Route
from starlette.responses import FileResponse


async def get_html(request):
    return FileResponse("web_sockets.html")


class UserCounter(WebSocketEndpoint):
    encoding = "text"
    sockets = []

    async def on_connect(self, websocket):  # A
        await websocket.accept()
        UserCounter.sockets.append(websocket)
        await self._send_count()

    async def on_disconnect(self, websocket, close_code):  # B
        UserCounter.sockets.remove(websocket)
        await self._send_count()

    async def on_receive(self, websocket, data):
        pass

    async def _send_count(self):  # C
        if len(UserCounter.sockets) > 0:
            count_str = str(len(UserCounter.sockets))
            task_to_socket = {
                asyncio.create_task(websocket.send_text(count_str)): websocket
                for websocket in UserCounter.sockets
            }

            done, pending = await asyncio.wait(task_to_socket)

            for task in done:
                if task.exception() is not None:
                    if task_to_socket[task] in UserCounter.sockets:
                        UserCounter.sockets.remove(task_to_socket[task])


Route("/web_sockets.html", endpoint=get_html),


app = Starlette(
    routes=[
        WebSocketRoute("/counter", UserCounter),
        Route("/web_sockets.html", endpoint=get_html),
    ]
)

```
___

Relations: [[Starlette - ASGI-совместимый каркас Python]] [[Асинхронный интерфейс серверного шлюза ASGI Python]] 
Tags: #code
References: [[Asyncio (книга)]] 
Query: 