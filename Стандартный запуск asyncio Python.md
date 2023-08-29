Запуск асинхронного кода в Python связан с использование модуля asyncio. Важно понимать, что любая асинхронная функция определяется через `async def` и ***не может быть вызвана*** из неасинхронного блока кода. 

Для вызова и запуска асинхронного кода используется вызов `asyncio.run`, который может быть реализован по разному. В представленному ниже коде вызов реализован через цикл событий, но можно вызывать асинхронные функции и без явного указания цикла. 

___
```
import asyncio


async def func1(dop: bool):
    loop = asyncio.get_event_loop()
    if dop:
        new_task = loop.create_task(func2())
        await new_task
    print("it is func1")


async def func2():
    print("it is func2")


async def main(vars):
    loop = asyncio.get_event_loop()
    tasks = [loop.create_task(func1(var)) for var in vars]

    await asyncio.gather(*tasks)


if __name__ == "__main__":
    vars = [0, 0, 1, 0]
    loop = asyncio.new_event_loop()
    loop.run_until_complete(main(vars))
```

___
Relations: [[asyncio Python]] 
Tags: #code
References: 
Query: Как запустить асинхронную функция в Python? 