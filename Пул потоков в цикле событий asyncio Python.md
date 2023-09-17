Создание и использование пула потоков схоже с [[Пул процессов в цикле событий asyncio Python]]. 

Стоит отметить, что пул потоков определен как дефолтный исполнитель в `run_in_executor`. 

Помимо этого, в последних версиях Python был добавлен метод `asyncio.to_thread`, который позволяет сразу же создавать поток, не прибегая к пулу потоков и оборачиванию задач через `partitial`. 

___
```
import functools
import requests
import asyncio
from concurrent.futures import ThreadPoolExecutor


def get_status_code(url: str) -> int:
    response = requests.get(url)
    return response.status_code


async def main():
    loop = asyncio.get_running_loop()
    urls = ["https://www.example.com" for _ in range(1000)]

    with ThreadPoolExecutor() as pool:
        urls = ["https://www.example.com" for _ in range(1000)]
        tasks = [
            loop.run_in_executor(pool, functools.partial(get_status_code, url))
            for url in urls
        ]
        results = await asyncio.gather(*tasks)
        print(results)


async def new_main():
    urls = ["https:// www.example.com" for _ in range(1000)]
    tasks = [asyncio.to_thread(get_status_code, url) for url in urls]
    results = await asyncio.gather(*tasks)
    print(results)


if __name__ == "__main__":
    asyncio.run(main())
    asyncio.run(new_main())

```
___

Relations: [[Блокирующие API в asyncio]] [[asyncio Python]] 
Tags: #code
References: [[Asyncio (книга)]] 
Query: 