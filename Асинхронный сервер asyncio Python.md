
___
```
import asyncio
import signal
import socket
import logging
from asyncio import AbstractEventLoop
from typing import Set


class GracefullExit(SystemExit):
    pass


def shutdown():
    raise GracefullExit()


async def echo(
    connection: socket.socket,
    loop: AbstractEventLoop,
) -> None:
    try:
        while data := await loop.sock_recv(connection, 1024):
            await loop.sock_sendall(connection, data)
        await loop.sock_sendall(connection, data)

    except Exception as ex:
        logging.exception(ex)

    finally:
        connection.close()


async def close_echo_tasks(echo_tasks: list[asyncio.Task]) -> None:
    waiters = [asyncio.wait_for(task, 2) for task in echo_tasks]
    for task in waiters:
        try:
            await task
        except asyncio.exceptions.TimeoutError:
            pass


async def listen(
    server: socket.socket,
    loop: AbstractEventLoop,
    echo_tasks: list[asyncio.Task],
) -> None:
    while True:
        connection, address = await loop.sock_accept(server)
        connection.setblocking(False)

        echo_task = asyncio.create_task(echo(connection, loop))
        echo_tasks.append(echo_task)


async def main(echo_tasks: list[asyncio.Task]) -> None:
    loop = asyncio.get_event_loop()

    server = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
    server.setsockopt(socket.SOL_SOCKET, socket.SO_REUSEADDR, 1)

    server_address = ("127.0.0.1", 8000)
    server.bind(server_address)
    server.setblocking(False)
    server.listen()

    for signame in {"SIGINT", "SIGTERM"}:
        loop.add_signal_handler(getattr(signal, signame), shutdown)

    await listen(server, loop, echo_tasks)


if __name__ == "__main__":
    echo_tasks: list[asyncio.Task] = []
    loop = asyncio.new_event_loop()

    try:
        loop.run_until_complete(main(echo_tasks))
    except GracefullExit:
        loop.run_until_complete(close_echo_tasks(echo_tasks))
    finally:
        loop.close()

```

___
Relations: [[asyncio Python]] [[Обработка ошибок в задачах asyncio Python]] 
Tags: #code
References: [[Asyncio (книга)]] 
Query: 