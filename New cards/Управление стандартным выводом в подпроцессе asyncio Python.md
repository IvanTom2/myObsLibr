Можно также управлять выводом подпроцесса, чтобы он не попадал напрямую в консоль. Например, вывод можно транслировать и игнорировать, либо выбирать оттуда необходимые значения. 

Стоит учесть, что здесь имеют место [[Взаимные блокировки]]. Они происходят в том случае, если подпроцесс начинает выводить в буфер большое количество информации, а его потребление организовано некорректно. Получается так, что в буфер поступает большое количество информации, которое никто не забирает (или не успевает забирать), а дальнейшая запись в буфер невозможна, так как он уже переполнен. Возникает взаимное ожидание - основной процесс Python дожидается окончание работы процесса, а процесс завис на записи, потому что основной процесс Python не забирает данные из буфера. ***Проблема актуально только если не читать данные из буфера***. 

Чтобы избежать каких-либо проблем с взаимоблокировками, можно использовать `Process.communicate()`. Он также как `wait` блокирует дальнейшее выполнение, пока процесс не завершится. Кроме того, он конкурентно потребляет стандартный ввод и вывод, возвращая все, что было выведено как только приложение завершится. 

___
```
import asyncio
from asyncio import StreamReader
from asyncio.subprocess import Process


async def write_output(prefix: str, stdout: StreamReader):
    while line := await stdout.readline():
        print(f"[{prefix}]: {line.rstrip().decode()}")


async def main():
    program = ["ls", "-la"]
    process: Process = await asyncio.create_subprocess_exec(
        *program, stdout=asyncio.subprocess.PIPE
    )

    print(f"pid процесса: {process.pid}")
    stdout_task = asyncio.create_task(write_output(" ".join(program), process.stdout))
    return_code, _ = await asyncio.gather(process.wait(), stdout_task)
    print(f"Процесс вернул: {return_code}")


asyncio.run(main())

```
___
Зачем это нужно: [[]] 
Примеры применения: [[]] 
Основные ошибки: [[]]
___
Relations: [[Управление подпроцессами asyncio Python]] 
Tags: #code
References: [[Asyncio (книга)]] 
Query: 