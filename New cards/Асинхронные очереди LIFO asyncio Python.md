LIFO очередь также иногда называют стеком. Стек это такая структура данных, в который помещаются значения вверх списки и с верха списка забираются. 

___
```
import asyncio
from asyncio import Queue, LifoQueue
from dataclasses import dataclass, field


@dataclass(order=True)
class WorkItem:
    priority: int
    order: int
    data: str = field(compare=False)


async def worker(queue: Queue):
    while not queue.empty():
        work_item: WorkItem = await queue.get()
        print(f"Обрабатывается элемент {work_item}")
        queue.task_done()


async def main():
    lifo_queue = LifoQueue()

    work_items = [
        WorkItem(3, 1, "Lowest priority first"),
        WorkItem(3, 2, "Lowest priority second"),
        WorkItem(3, 3, "Lowest priority third"),
        WorkItem(2, 4, "Medium priority"),
        WorkItem(1, 5, "High priority"),
    ]

    worker_task = asyncio.create_task(worker(lifo_queue))
    for work in work_items:
        lifo_queue.put_nowait(work)


if __name__ == "__main__":
    asyncio.run(main())

```
___
Зачем это нужно: [[]] 
Примеры применения: [[]] 
Основные ошибки: [[]]
___
Relations: [[asyncio Python]] 
Tags: #code 
References: [[Asyncio (книга)]] 
Query: 