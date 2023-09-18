В `asyncio` реализованы абстракции транспортных механизмов и протоколов. Это низкоуровневые API и их лучше всего использовать тогда, когда нужен прямой контроль над тем, что происходит при отправке и получении данных. 

Большинству приложений не нужен низкоуровневый контроль, поэтому предпочтительнее пользоваться ***высокоуровневыми API потоков данных***: `StreamReader`, `StreamWriter`. 

Ниже представлен код выполнения HTTP-запросов с использованием потоков данных. 

Кроме сетевых взаимодействий, потоки данных также можно использовать для [[Неблокирующий ввод данных из командной строки asyncio Python]] и прочего. 

___
```
import asyncio
from asyncio import StreamReader
from typing import AsyncGenerator


async def read_until_empty(
    stream_reader: StreamReader,
) -> AsyncGenerator[str, None]:
    while response := await stream_reader.readline():
        yield response.decode()


async def main():
    host: str = "www.example.com"
    request: str = (
        f"GET / HTTP/1.1\r\n" + f"Connection: close\r\n" + f"Host: {host}\r\n\r\n"
    )

    stream_reader, stream_writer = await asyncio.open_connection("www.example.com", 80)
    try:
        stream_writer.write(request.encode())
        await stream_writer.drain()

        responses = [response async for response in read_until_empty(stream_reader)]
        print("".join(responses))

    except Exception as ex:
        print(ex)

    finally:
        stream_writer.close()
        await stream_writer.wait_closed()


if __name__ == "__main__":
    asyncio.run(main())

```
___

Relations: [[asyncio Python]] 
Tags: #code
References: [[Asyncio (книга)]] 
Query: 