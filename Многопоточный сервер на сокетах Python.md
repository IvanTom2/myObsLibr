Суть данного сервера заключается в том, что для каждого нового подключения создается отдельный поток. 

У данного подхода есть проблема, которая заключается в невозможности корректной остановки приложения. Если мы захотим остановить приложение (`CTRL+C`), то мы не сможем остановить его потоки. Будет возбуждена ошибка `KeyboardInterrupt` в главном потоке. 

Для решения используются:
1. Потоки-демоны. Такого рода потоки придуманы для выполнения длительных фоновых задач. Они не мешают приложению завершиться. Если работают только потоки-демоны, то приложение завершается само по себе. С демонами возникает проблема в том, что эти потоки работают сами по себе и завершаются без уведомления. 
2. Собственный способ снятия работающего потока. Например, определить свой класс потока с соответствующей логикой снятия. 

___
```
from threading import Thread
import socket


def echo(client: socket) -> None:
    while True:
        data = client.recv(2048)
        print(f"Получно {data}, отправляю!")
        client.sendall(data)


def run_server():
    with socket.socket(socket.AF_INET, socket.SOCK_STREAM) as server:
        server.setsockopt(socket.SOL_SOCKET, socket.SO_REUSEADDR, 1)
        server.bind(("127.0.0.1", 8000))
        server.listen()

        while True:
            connection, _ = server.accept()
            thread = Thread(target=echo, args=(connection,))
            thread.start()


if __name__ == "__main__":
    run_server()

```
___

Relations: [[Неблокирующий сервер на сокетах Python]] 
Tags: #code
References: [[Asyncio (книга)]] 
Query: 