Создавая [[Пул процессов в цикле событий asyncio Python]] мы добивались повышения производительности за счет распараллеливания задач. Тем не менее, мы не имели прямого доступа к управлению процессами.

Иногда возникает задача, когда необходимо использовать код, написанный на другом языке программирования. Это может быть код, предназначенный для повышения производительности вычислений (C++ или Rust). Либо реализация программы на другом языке, чтобы не переписывать ее на Python. Либо использование системных возможностей операционной системы через прямой вызов. 

В стандартной библиотеке Python имеется блокирующий модуль `subprocess`, который несовместим с `asyncio`, если не пребегать к многопроцессорной или многопоточной обработке. Сам же модуль `asyncio` предлагает похожую асинхронную реализацию подпроцессов (он повторяет функционал `subprocess`). 

Представленный ниже код позволяет выполнить команду `ls` из подпроцесса. 

Подводный камень использования подпроцесса в том, что мы можем создать сопрограмму, которая создает процесс. Мы ***можем остановить сопрограмму***. Мы ***не можем остановить процесс***. Для остановки процесса необходимо использовать специальный метод асинхронного подпроцесса `Process.terminate()` или `Process.kill()`. Эти методы посылают процессу сигналы `SIGTERM` и `SIGKILL` соответственно. ***Оба метода неблокирующие и не являются сопрограммами***. 

___
```
import asyncio
from asyncio.subprocess import Process


async def ls_proc():
    process: Process = await asyncio.create_subprocess_exec("ls", "-l")
    print(f"pid процесса: {process.pid}")
    status_code = await process.wait()
    print(f"Код состояния: {status_code}")


async def sleep_proc():
    process: Process = await asyncio.create_subprocess_exec("sleep", "3")
    print(f"pid процесса: {process.pid}")
    try:
        status_code = await asyncio.wait_for(process.wait(), timeout=1.0)
        print(status_code)
    except asyncio.TimeoutError:
        print("Тайм-аут, завершаю принудительно...")
        process.terminate()
        status_code = await process.wait()
        print(status_code)


asyncio.run(ls_proc())
asyncio.run(sleep_proc())

```
___
Зачем это нужно: [[]] 
Примеры применения: [[]] 
Основные ошибки: [[]]
___
Relations: [[asyncio Python]] [[Пул процессов в цикле событий asyncio Python]] 
Tags: #code
References: 
Query: 