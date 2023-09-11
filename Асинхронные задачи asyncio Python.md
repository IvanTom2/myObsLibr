Задачи создаются через конструкцию `asyncio.create_task()`. В качестве аргумента передается созданный корутин с какими-либо аргументами. 

При создании задачи создается объект `asyncio.Task` - более подробно это описано в [[Объекты asyncio Python]]. 

Теоретически, задачи могут работать бесконечно долго, если во время выполнения им ничего не вернется. В таком случае приложение зависнет. 
Чтобы такого не происходило, можно отменять задачи. Для этого существуют различные методы:
1. `task = asyncio.Task.cancel()` - применяется к определенному объекту класса `asyncio.Task`. Если задача ***не выполнена до момента остановки*** и ***будет ожидаться***, то возникнет ошибка `asyncio.CancelledError`. 
2. `asyncio.wait_for(task, timeout)` - позволяет выставить таймаут для ожидания завершения задачи. 

От снятия задачи через `asyncio.wait_for` существует защита `asyncio.shield`, которая не позволяет снять определенную задачу - приложение будет выполнять ее в любом случае. ***Важно отметить***, что задачу нельзя будет дождаться через `await` - потому что возникнет исключение `asyncio.TimeoutError`. Но задача никуда не пропадет - она будет дорабатывать в фоновом режиме. Это ***не совсем понятная конструкция***. 

___
```
import asyncio 


async def coro(num: int) -> int:
	await asyncio.sleep(num)
	return num

async def main(nums: list[int]):
	task1 = asyncio.create_task(coro(nums[0]))
	task2 = asyncio.create_task(coro(nums[1]))
	task3 = asyncio.create_task(coro(nums[2]))
	task4 = asyncio.create_task(coro(nums[3]))
	task4 = asyncio.create_task(coro(nums[4]))

	num1 = await task1
	num2 = await task2
	task3.cancel() # отмена задачи
	num4 = await asyncio.wait_for(task4, 1) #asyncio.TimeoutError
	num5 = await asyncio.wait_for(asyncio.shield(task5), 1)

	num3 = await task3 #asyncio.CancelledError
	

asyncio.run(main([1, 2, 3, 4, 5]))

```
___
Relations: [[asyncio Python]] 
Tags: #code #todo 
References: 
Query: 