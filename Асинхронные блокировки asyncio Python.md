Асинхронные блокировки из модуля `asyncio` работают точно также, как и прочие блокировки. Мы захватываем блокировку, выполняем код в критической секции и освобождаем ее, позволяя другим сопрограммам захватить блокировку. 

Главное отличие от прочих блокировок заключается в том, что асинхронные блокировки - это объекты, допускающие ожидание. Если программа ожидает блокировку, то в это время может выполняться любой другой код сопрограмм. Кроме того, асинхронные блокировки являются контекстными менеджерами. 

Важно отметить, что ***блокировки следует создавать внутри сопрограмм***. Если сделать блокировку глобальной переменной, то будет возникать ошибка, связанная с использование другого цикла событий. Такое поведение используется почти во всех объектах `asyncio` - у каждого объекта есть факультативный параметр `loop`, и если он не задан, то создается новый цикл событий (актуально до Python 3.10). 

___
```
import asyncio
from asyncio import Lock


class MockSocket:
    def __init__(self):
        self.socket_closed = False

    async def send(self, msg: str):
        if self.socket_closed:
            raise Exception("Сокет закрыт!")
        print(f"Отправляется: {msg}")
        await asyncio.sleep(1)
        print(f"Отправлено: {msg}")

    def close(self):
        self.socket_closed = True


user_names_to_sockets = {
    "John": MockSocket(),
    "Terry": MockSocket(),
    "Graham": MockSocket(),
    "Eric": MockSocket(),
}


async def user_disconnect(username: str, user_lock: Lock):
    print(f"{username} отключен!")
    async with user_lock:
        print(f"{username} удаляется из словаря")
        socket = user_names_to_sockets.pop(username)
        socket.close()


async def message_all_users(user_lock: Lock):
    print("Создаются задачи отправки сообщений")
    async with user_lock:
        messages = [
            socket.send(f"Привет, {user}")
            for user, socket in user_names_to_sockets.items()
        ]
    await asyncio.gather(*messages)


async def main():
    user_lock = Lock()
    await asyncio.gather(
        message_all_users(user_lock), user_disconnect("Eric", user_lock)
    )


if __name__ == "__main__":
    asyncio.run(main())

```
___

Relations: [[Синхронизация и состояние гонки в asyncio Python]] [[asyncio Python]] 
Tags: #code
References: [[Asyncio (книга)]] 
Query: 