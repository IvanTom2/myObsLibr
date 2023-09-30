Семафоры ограничивают количество конкурентно работающих сопрограмм. 

Тем не менее, при использовании одного семафора в различных сопрограммах, может произойти ситуация взаимной блокировки. 
Это возникает в следующих ситуациях. Существует сопрограмма 1, которая занимает семафор. В сопрограмме 1 вызывается несколько раз сопрограмма 2, которая также занимает семафор. В какой-то момент может произойти ситуация, когда будет создано несколько задач сопрограммы 1, которые в момент своего выполнения будут вызывать задачи спорограммы 2 и в результате этого семафор будет полностью заполнен. У задач сопрограммы 1 не будет возможности освободить семафор, так как не будут завершены задачи сопрограммы 2 - а они не будут завершены по той причине, что даже не приступили к выполнения, так как их ограничивает полностью занятый семафор. 

___
```
### Semaphore
import asyncio
from asyncio import Semaphore
from aiohttp import ClientSession


async def get_url(url: str, session: ClientSession, semaphore: Semaphore):
    print("Жду возможности захватить семафор...")
    async with semaphore:
        print("Семафор захвачен, отправляется запрос...")
        response = await session.get(url)
        print("Запрос завершен")
        return response.status


async def main():
    semaphore = Semaphore(10)
    async with ClientSession() as session:
        tasks = [
            get_url("https://www.example.com", session, semaphore) for _ in range(1000)
        ]
        await asyncio.gather(*tasks)


if __name__ == "__main__":
    asyncio.run(main())

### Semaphore problem
import asyncio
from asyncio import Semaphore


async def acquire(semaphore: Semaphore):
    print("Ожидание возможности захвата")
    async with semaphore:
        print("Захвачен")
        await asyncio.sleep(5)
    print("Освобождается")


async def release(semaphore: Semaphore):
    print("Одиночное освобождение!")
    semaphore.release()
    print("Одиночное освобождение - готово!")


async def main():
    semaphore = Semaphore(2)
    print("Два захвата, три освобождения...")
    await asyncio.gather(acquire(semaphore), acquire(semaphore), release(semaphore))
    print("Три захвата...")
    await asyncio.gather(acquire(semaphore), acquire(semaphore), acquire(semaphore))


if __name__ == "__main__":
    asyncio.run(main())

### BoundedSemaphore
import asyncio
from asyncio import BoundedSemaphore

async def main():
	semaphore = BoundedSemaphore(1)
	await semaphore.acquire()
	semaphore.release()
	semaphore.release()

if __name__ == "__main__":
	asyncio.run(main())

```
___

Relations: [[Синхронизация и состояние гонки в asyncio Python]] [[asyncio Python]] 
Tags: #code
References: [[Asyncio (книга)]] 
Query: 