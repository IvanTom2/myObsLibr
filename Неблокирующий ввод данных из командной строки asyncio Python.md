Для неблокирующего ввода данных необходима реализация потокового читателя. В приведенном ниже коде создается потоковый читатель, затем он передается в протокол. Далее вызывается сопрограмма `connect_read_pipe`, которой передается лямбда-фукция, являющаяся фабрикой заранее определенного класса (протокола), а также передается `sys.stdin`. 

___
```
import asyncio
from asyncio import StreamReader
import sys


async def delay(delay_time: int) -> None:
    await asyncio.sleep(delay_time)
    print("Поспал", delay_time, "секунд")


async def create_stdin_reader() -> StreamReader:
    stream_reader = asyncio.StreamReader()
    protocol = asyncio.StreamReaderProtocol(stream_reader)
    loop = asyncio.get_running_loop()
    await loop.connect_read_pipe(lambda: protocol, sys.stdin)
    return stream_reader


async def main():
    stdin_reader = await create_stdin_reader()
    while True:
        delay_time = await stdin_reader.readline()
        asyncio.create_task(delay(int(delay_time)))


if __name__ == "__main__":
    asyncio.run(main())

```
___

Relations: [[Потоки данных asyncio Python]] 
Tags: #code
References: [[Asyncio (книга)]] 
Query: 