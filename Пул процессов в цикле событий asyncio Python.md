В Python нельзя использовать `multiprocessing.Pool` в качестве исполнителя в цикле событий. Это связано с тем, что у него неподходящее API для работы с циклом событий. Тем не менее, точно такой же пул может предоставить модуль `concurrent.futures`. 

Из модуля `concurrent.futures` вызывается пул процессов `ProcessPoolExecutor`. Стоит заметить, что в пул процессов задачу можно передавать лишь одну задачу. Для того, чтобы решить эту проблему, есть несколько подходов:
1. Создавать корутины `loop.run_in_executor` напрямую - т.е. с передачей аргументов в функцию `run_in_execotor`, так как она это позволяет делать. 
2. Создавать корутины `loop.run_in_execotor` с использованием засетапленных задач через `functools.partitial`. В таком случае перед передачей мы подготовим задачу уже с аргументами и передадим конкретно ее. 

Далее созданные корутины можно вызывать и использовать точно также, как и все прочие корутины [[Управлением массивом задач в asyncio Python]]. 

___
```
import asyncio
import time
from concurrent.futures import ProcessPoolExecutor
from functools import partial


def count(num: int) -> int:
    start = time.time()
    # print(num)

    counter = 0
    while counter < num:
        counter += 1

    # print(time.time() - start)
    return counter

def main():
    loop = asyncio.get_event_loop()
    process_pool = ProcessPoolExecutor(4)
    nums = [3000000, 5000000, 7000000, 10]

	### partitial calls
    # call_coroutines = []
    # calls = [partial(count, num) for num in nums]
    # for call in calls:
    #     call_coroutines.append(
    #         loop.run_in_executor(
    #             process_pool,
    #             call,
    #         )
    #     )

	### partitial calls another way
    # call_coroutines = [
    #     loop.run_in_executor(process_pool, call)
    #     for call in [partial(count, num) for num in nums]
    # ]

	### direct calls
    call_coroutines = [
        loop.run_in_executor(
            process_pool,
            count,
            num,
        )
        for num in nums
    ]


    # results = await asyncio.gather(*call_coroutines)
    # for res in results:
    #     print(res)

    for res in asyncio.as_completed(call_coroutines):
        print(await res)


if __name__ == '__main__':
	asyncio.run(main())

```
___

Relations: [[asyncio Python]] 
Tags: #code
References: [[Asyncio (книга)]] 
Query: 