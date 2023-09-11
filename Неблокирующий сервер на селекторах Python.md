Данный сервер чем-то похож на реализацию цикла событий. 
Текущая реализация плоха тем, что чтобы написать сервер, было бы необходимо написать свой собственный цикл событий - подход на селекторах слишком низкоуровневый. 

___
```
import selectors
import socket
from selectors import SelectorKey
from typing import List, Tuple

selector = selectors.DefaultSelector()
server = socket.socket()

server.setsockopt(socket.SOL_SOCKET, socket.SO_REUSEADDR, 1)
server_address = ("127.0.0.1", 8000)

server.setblocking(False)
server.bind(server_address)
server.listen()

selector.register(server, selectors.EVENT_READ)

while True:
    events: list[Tuple[SelectorKey, int]] = selector.select(timeout=1)

    if len(events) == 0:
        # print("Событий нет")
        pass

    for event, _ in events:
        event_socket: socket.socket = event.fileobj

        if event_socket == server:
            connection, address = server.accept()
            connection.setblocking(False)
            selector.register(connection, selectors.EVENT_READ)

        else:
            data = event_socket.recv(1024)
            event_socket.send(data)

```
___

Relations: [[Python Hints]] [[Неблокирующий сервер на сокетах Python]] 
Tags: #code
References: [[Asyncio (книга)]] 
Query: 