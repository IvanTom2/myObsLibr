Очередь с приоритетами реализуется иначе, чем обычная FIFO очередь. Очередь с приоритетами реализуется не как список, а как ***пирамида*** (`heapq`). 

Пирамида - это двоичное дерево, обладающее свойствами, что значение родительского узла меньше значений любого его потомка. 
Для помещения задач в очередь придется реализовать кортеж, который состоит из приоритета в очереди и произвольных данных для задачи. Чем выше первое значение, отвечающее за приоритет, тем ниже у него приоритет в очереди. 

Либо можно сделать реализацию со специальным классом, который будет являться задачей. Класс можно реализовать с помощью `dataclass` или с помощью обычного класса, используя `dunder` методы. 

Стоит заметить, что ***при добавлении задач с одинаковым приоритетом, они могут выполнять не в той последовательности, в которой были добавлены***. 
Чтобы устранить неоднозначность, достаточно ввести второй порядок приоритета. 

___
```
import asyncio
from asyncio import Queue, PriorityQueue
from typing import Tuple
from dataclasses import dataclass, field


@dataclass(order=True)
class WorkItem:
    priority: int
    data: str = field(compare=False)


async def worker(queue: Queue):
    while not queue.empty():
        work_item: Tuple[int, str] = await queue.get()
        print(f"Обрабатывается элемент {work_item}")
        queue.task_done()


async def main():
    priority_queue = PriorityQueue()

    # work_items = [
    #     (3, "Lowest priority"),
    #     (2, "Medium priority"),
    #     (1, "High priority"),
    # ]

    work_items = [
        WorkItem(3, "Lowest priority"),
        WorkItem(2, "Medium priority"),
        WorkItem(1, "High priority"),
    ]

    worker_task = asyncio.create_task(worker(priority_queue))
    for work in work_items:
        priority_queue.put_nowait(work)
    await asyncio.gather(priority_queue.join(), worker_task)


if __name__ == "__main__":
    asyncio.run(main())

```
___
Зачем это нужно: [[]] 
Примеры применения: [[Асинхронная очередь с приоритетами в web приложении asyncio Python]] 
Основные ошибки: [[]] 
___
Relations: [[asyncio Python]] 
Tags: #code 
References: [[Asyncio (книга)]] 
Query: 